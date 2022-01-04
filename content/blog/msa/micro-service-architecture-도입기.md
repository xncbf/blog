---
title: micro service architecture 도입기
date: 2021-12-03 17:12:06
category: msa
thumbnail: { thumbnailSrc }
draft: false
---

- [msa 를 도입한 이유](#msa-를-도입한-이유)
- [도입하기전 마음속으로 정했던 조건들](#도입하기전-마음속으로-정했던-조건들)
- [프로젝트를 시작하며 고려했던것들](#프로젝트를-시작하며-고려했던것들)
  - [서비스를 나누는 기준](#서비스를-나누는-기준)
  - [git repogitory](#git-repogitory)
  - [코드 관리](#코드-관리)
  - [PR, 테스팅, build pipeline](#pr-테스팅-build-pipeline)
  - [saas 서비스들](#saas-서비스들)
- [기타 자잘하게 생각했던 부분들](#기타-자잘하게-생각했던-부분들)
  - [web framework (django vs fastapi)](#web-framework-django-vs-fastapi)
  - [test (unittest vs pytest)](#test-unittest-vs-pytest)
- [마무리](#마무리)

팀내에서 새로 시작하는 프로젝트에서 `architect`를 맡아 `msa` 를 도입하게 되면서 느꼈거나 생각했던 점들을 적어보려고 한다.

## msa 를 도입한 이유

새롭게 시작하는 프로젝트는 앞으로의 확장 가능성을 최대한 고려해서 틀을 짜놓고 시작하고 싶었다.  
사실 시작부터 msa 를 한다기보다는 앞으로 서비스를 쪼개기 용이하도록 미리 준비를 해놓는 것에 가깝다.

## 도입하기전 마음속으로 정했던 조건들

1. 속도가 빨라야 한다 (low latency)
2. 서비스간의 최소한의 의존성만 가져야 한다
3. 프로젝트가 최대한 복잡해지지 않아야 한다
4. 유지보수가 용이해야한다

## 프로젝트를 시작하며 고려했던것들

### 서비스를 나누는 기준

한번에 조회되지 않는 데이터를 기준삼기로 했다.  
예를들면 상품페이지에서는 결제관련 정보가 필요없기때문에 `상품서비스`, `결제서비스` 와 같은 방식으로 나누었다.  
`결제서비스`에서 상품 정보가 필요한경우에는 상품의 `pk` 를 가지고있다가 `상품서비스`에 정보를 요청하면 되고
`join` 이나 `aggregation` 이 필요한 경우에는 일부 정보를 각각의 서비스에 함께 저장하는 `역정규화(DeNormalization)` 기법을 사용하기로 했다.

### git repogitory

파이프라인에서 code test 할 때 통합테스트가 용이하도록 모든 서비스는 하나의 레파지토리에서 관리하기로 했다.

### 코드 관리

그 동안 여러 프로젝트를 진행했던 경험을 바탕으로 코드를 관리하되, 좋았던 것들과 좋지 않았던 것들을 반영하려고 노력했다.

1. 의존성관리 툴은 `pipenv` 를 사용한다.
2. `flake8`, `isort`, `black` 을 사용하면서 가독성에 신중을 기한다.
3. 외부 의존성 관련 설정들은 [pep518](https://www.python.org/dev/peps/pep-0518) 에 의거하여 `pyproject.toml` 파일 하나에서 최대한 관리할 수 있도록 했다.
4. `테스팅`과 `PR` 을 필수로 하고 메인브랜치에는 아무도 강제 `push` 할 수 없도록 정책을 정한다.
5. 브랜치 전략은 회고에서 소개했던 [trunk based development](https://pypy.dev/dev/git-flow-%EC%9E%98%EB%AA%BB-%EC%93%B0%EA%B3%A0-%EC%9E%88%EC%97%88%EB%8B%A4-azure-devops-%EC%97%90%EC%84%9C-%EA%B6%8C%EC%9E%A5%ED%95%98%EB%8A%94-%EB%B8%8C%EB%9E%9C%EC%B9%98-%EC%A0%84%EB%9E%B5/#trunk-based-development) 를 사용한다.

### PR, 테스팅, build pipeline

`Pull request` 와 `테스팅`은 작년 초에 팀에 처음으로 도입해서 너무 좋은 경험을 해봤고, 이제는 테스팅 없이 내 코드를 믿을 수 없는 지경이 되어서 무조건 도입하기로 했다.  
다만 이번에는 서비스가 거대해지더라도 유지보수와 새로운 기능 추가에 부담이 되지 않도록 `가벼운 테스트`, `무거운 테스트`를 나눠서 테스트하고 싶었다.
외부 의존성이 없는 가벼운 비즈니스 로직을 먼저 테스트하고 실제 DB 연결이 필요하거나 외부 의존성이 있는 코드들을 따로 테스트하기 위해 여러개의 빌드 파이프라인을 준비했다.
계획한 코드 배포 순서는 다음과 같다.

1. 코드를 작성하고 PR 을 생성한다.
2. 코드가 수정된 서비스의 PR용 파이프라인이 완료된다 (lint 테스트, 가벼운 테스트)
3. 코드리뷰를 시작한다.
4. 리뷰를 시작함과 동시에 통합테스트용 파이프라인이 돌아간다 (무거운 테스트)
5. 리뷰 코멘트가 달리거나, 무거운 테스팅이 실패하면 코드를 수정하고 다시 2번부터 반복한다.
6. 리뷰가 끝나고 테스팅이 완료되면 merge 한다.
7. 배포용 파이프라인이 실행되고 파이프라인 내부에서 이미지를 빌드하고 artifact를 생성한다.
8. azure release 에서 maintainer 의 배포 승인을 받으면 artifact 를 가져와서 배포한다.

### saas 서비스들

우리는 관리할 인원이 적다는것을 고려해서 최대한 saas 의 도움을 받기로 했다.  
되도록이면 managed service 를 사용하도록 했고 다음은 고려대상과 선택 결과이다.  
앞쪽에 적은 것들을 선택했다

1. 웹서버: `ecs(fargate)` vs `ecs(ec2)` vs `eks` vs `elastic beanstalk`
2. 웹서비스: `docker-compose` vs `kubernetes`
3. Queue: `sqs` vs `elastiCache(Redis)` vs `AmazonMQ(RabbitMQ)`
4. 웹소켓 서버: `api gateway(socket protocol) + lambda` vs `Socket.IO`

## 기타 자잘하게 생각했던 부분들

### web framework (django vs fastapi)

우리는 메인 스택이 python이라서 `django + drf`와 `fastapi` 둘 중에서 고민하다 `fastapi` 를 선택했다.
그 이유는 다음과 같았다.

1. asgi 를 지원하기때문에 속도가 더 빠르다 (django도 3.x 버전으로 넘어오면서 asgi 를 지원하기 시작했지만 drf 는 아직 지원 안함)
2. `pydantic` 을 사용하면서 자연스럽게 만들어지는 swagger
3. 강제적인 typing
4. 구현할것이 상대적으로 많지만 가볍다
5. rdbms 에 의존하지 않는다

### test (unittest vs pytest)

지난 프로젝트에 unittest 를 사용하며 다음과 같은 아쉬운점이 있었기 때문에 pytest 를 사용하기로 결정했다.

1. 서드파티 라이브러리들의 지원부족 (pytest가 점점 표준으로 자리잡고 있는 것 같다)
2. 터미널 가독성 (`pytest`는 터미널 텍스트에 컬러링을 해준다)
3. 내장된 기능들

## 마무리

앞으로 크게 키워갈 자사 서비스를 만들기 시작해서 너무 기쁘지만 처음 시도해보는 부분들이 너무 많아서 걱정 반 기대 반이다.  
프로젝트 1차 MVP 개발 마무리는 내년 중에 될 것 같고 프로젝트가 완료되면 회고를 적어야겠다.
