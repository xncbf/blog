---
title: Django + Zappa 동시성 문제 해결 시도 (cold start)
date: 2021-05-24 18:05:35
category: serverless
thumbnail: { thumbnailSrc }
draft: false
---

# Zappa 란?

[zappa](https://github.com/zappa/Zappa) 는 파이썬 코드를 aws lambda 에서 쉽게 웹서비스 할 수 있도록 만들어주는 패키지이다

# 발생한 문제점

lambda 동시 호출 수가 늘어날때 lambda 에서 자동으로 스케일업을 해주는데, 이때 접속한 유저는 timeout (약 10초) 을 경험함 (오른쪽 표시된 부분의 바닥의 울퉁불퉁한 부분이 그것이다)

![](<./images/django-+-zappa-동시성-문제-해결-시도-(cold-start)-0.png>)

# 문제 발생 원인

## cold start

lambda 에서 스케일업을 할때 cold start 가 발생하는데, 문서에 따르면 python 의 cold start 시간은 길어봐야 0.5초 내외인것을 확인할 수 있다

![](<./images/django-+-zappa-동시성-문제-해결-시도-(cold-start)-1.png>)

## 그렇다면 나는 왜?

이번 이슈는 lambda 의 한계점 때문에 발생한 이슈이다

[lambda는 저장공간의 제약](https://docs.aws.amazon.com/lambda/latest/dg/gettingstarted-limits.html)이 있다

![](<./images/django-+-zappa-동시성-문제-해결-시도-(cold-start)-2.png>)

배포할때 패키지의 크기가 50MB 를 넘으면 zappa 에서 제공하는 slim handler 기능을 이용해서 512MB 까지는 업로드할 수 있도록 해주는데

원리는 s3 에 업로드한 후 lambda 에서 파일을 `/tmp` 디렉토리에 다운로드해서 사용하는 방식이다.

s3에서 다운로드하는 부분이 느려서 발생한 원인이라고 판단했다

한번 로드되면 keep warm 되기 때문에 동시성 1개당 1 Invocations 씩 느림을 경험하는것이다

concurrent executations 가 튈때마다 duration 이 8초로 튀는것을 확인할수있다.

![](<./images/django-+-zappa-동시성-문제-해결-시도-(cold-start)-3.png>)

![](<./images/django-+-zappa-동시성-문제-해결-시도-(cold-start)-4.png>)

# 시도해본것들

## efs

파일시스템을 로드해놓고 동일한 vpc를 가진 lambda 에서 빠르게 마운트해서 사용하는 기능이다

lambda 는 vpc 를 가지면 public access 가 불가능하기 때문에 지금 상황과는 맞지 않아 적용하지 않기로 했다 (NAT 게이트웨이를 사용해서 public 아웃바운드 할 수는 있다)

## 프로젝트 사이즈 줄이기

압축된 패키지 사이즈를 50MB 이하로 줄여서 slim handler 기능을 사용하지 않으면 (아마도 이론상) s3 로 업로드 하지않고 handler 에서 바로 구동할것이고 1초 이내로 로드가 가능할 것 같았다

slim_handler 기능을 제거하고 테스트해 본 결과는 똑같았다

![](<./images/django-+-zappa-동시성-문제-해결-시도-(cold-start)-5.png>)

![](<./images/django-+-zappa-동시성-문제-해결-시도-(cold-start)-6.png>)

유저가 늘어나면 늘어날수록 동시에 응답시간도 점차 늘어났다

![](<./images/django-+-zappa-동시성-문제-해결-시도-(cold-start)-7.png>)

## container

작년 말에 새롭게 도입된 기능이다.

위에 이미지와 같이 docker 에서 cold start 가 느려도 1.5초 내로 로드된다는 문서를 보고 도입할만하다고 생각했다 업로드 제한도 10GB이다

다만 zappa 의 범위를 넘어서는 시도이기 때문에 프로젝트가 커져서 패키지 사이즈를 감당하지 못할때 시도해볼만하다

zappa 가 만들어주는 패키지를 컨테이너로 감싸 실행해보았다

하지만 실행해보니 여전히 지연이 존재했다.

## 프로비저닝된 동시성 구성

cold start 가 느린것이니 미리 띄워놓으면 starting initaiation time 을 줄일 수 있지 않을까?

좀 더 자세히 찾아보았다.

### 프로비저닝 동시성을 오토스케일 컨트롤

aws 웹에서 보는 lambda 의 설정에는 찾을 수 없는 숨은 기능이 있었다

프로비저닝 동시성을 아래 그림처럼 오토스케일해주는 기능이 있었다

![](<./images/django-+-zappa-동시성-문제-해결-시도-(cold-start)-8.png>)

안 쓸 이유가 없는것 같았다

바로 써보았다

```zsh
aws lambda put-provisioned-concurrency-config --function-name my-function \
--qualifier BLUE --provisioned-concurrent-executions 100
```

```zsh
aws application-autoscaling register-scalable-target --service-namespace lambda \
      --resource-id function:my-function:BLUE --min-capacity 1 --max-capacity 100 \
      --scalable-dimension lambda:function:ProvisionedConcurrency
```

```zsh
aws application-autoscaling put-scaling-policy --service-namespace lambda \
--scalable-dimension lambda:function:ProvisionedConcurrency --resource-id function:my-function:BLUE \
--policy-name delivery-autoscale-policy --policy-type TargetTrackingScaling \
--target-tracking-scaling-policy-configuration '{ "TargetValue": 0.7, "PredefinedMetricSpecification": { "PredefinedMetricType": "LambdaProvisionedConcurrencyUtilization" }}'
```

하지만 역시 동일한 지연이 존재했다.

왜...?

콜드스타트가 아닌 다른 근본적인 문제가 있는것같다

## zappa 해킹

zappa 에서 handler 를 호출하는 시점을 앞당기는 시도를 해보았다

`LambdaHandler` 가 lazy load 되고 있어서 lambda 가 실행되기 전 concurrency 가 올라올때 실행될수있도록 변경해 보았더니 cold start 가 1초 미만으로 획기적으로 줄어들었다
zappa 레포 주인이 업데이트를 잘 안해주어서 일단 해결될때까진 별도의 레파지토리로 운영하기로 했다

![](<./images/django-+-zappa-동시성-문제-해결-시도-(cold-start)-9.png>)

![](<./images/django-+-zappa-동시성-문제-해결-시도-(cold-start)-10.png>)

![](<./images/django-+-zappa-동시성-문제-해결-시도-(cold-start)-11.png>)

# lambda와 별개로 시도한것들

## RDS 의 read replication 을 생성

RDS 는 조금만 커밋이 늘어나도 금방 CPU 로드가 100을 찍는 경험을 할수 있었다

최대한 DB 의 부하를 나눠주고자 읽기전용 복제본을 생성하고 django routers 를 통해 적절히 분배해주었다

딱 나눈만큼 정직하게 부하가 줄어들었다

## 최대한 모든 api 를 캐싱

인메모리 디비(elastic cache) 를 사용해서 동일하게 반복되는 api 의 response 를 캐싱했다

DB의 부하가 효과적으로 줄어들었다

## RDS proxy 생성

RDS proxy 를 사용하면 DB 커넥션 풀을 효과적으로 사용할 수 있는 것 같다

rds proxy 는 동일한 vpc 내에 있어야한다 고로 vpc 를 생성해야하는 efs 와 동일한 이유로 선택하지않았다 VPC 를 구성할 때 고려할 만한 선택지인 것 같다

# 마무리

실행시점에 따라서 7~9초나 달라지는것은 이해가 안되지만 우연히 발견한 방법으로 실행시간을 줄일 수 있었다

제일 도움이 될거라 생각했지만 비용때문에 눈물을 머금고 시도해본 provisioned concurrency 는 별로 도움이 되지 않았다

slim_handler 옵션을 끄는것도 별로 도움이 되지 않았다 (프로젝트 사이즈가 커지면 도움이 될지도?)

lambda 를 운영환경에서 사용하는것에 대해 의구심이 들었지만 aws 에서는 lambda 웹서버를 적극적으로 밀고있고 나도 적극적으로 사용해보려고 한다

참고로 zappa 를 사용하지않고 mangum + fastapi 조합으로 사용해본 프로젝트는 문서에 적힌대로 0.5초 이내의 cold start 실행시간을 보여주었다
