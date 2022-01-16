---
title: 'about'
date: 2021-11-10 13:21:13
lang: 'ko'
---

# 김준환

<div align="right"><sub><i>Last updated: 2021.11.10</i></sub></div>

<!-- **저는 `______` 하는 엔지니어입니다.**

1. 좋은 코드와 설계를 고민하는
2. 지속 가능한 프로덕트를 개발하는데 집중하는
3. 자동화를 통한 생산성 향상에 신경쓰는
4. 업무 프로세스 그리고 이를 뒷받침하는 조직 문화에 관심이 많은

**저는 `______` 하는 조직을 선호합니다.**

1. 투명한 의사결정이 이루어지고 공유되는
2. 구성원 간 신뢰를 기반으로 자율적으로 일하는
3. 불필요한 커뮤니케이션을 줄여 효율적으로 움직이는
4. 하는 일이 조직의 밸류 체인에서 어떠한 임팩트를 갖는지 고민하는
5. 기술이 비즈니스의 핵심 동력이며 제 코드가 비즈니스에 도움이 될 수 있는 -->

|             |                            |
| :---------: | -------------------------- |
| **GitHub**  | <https://github.com/xncbf> |
|  **Blog**   | <https://pypy.dev>         |
| **Contact** | <-py@kakao.com>            |

<br />

# Experiences

## FreeD Group (travelflan)

|              |                                   |
| -----------: | --------------------------------- |
|   **period** | 17.06 ~ current                   |
| **position** | Backend Engineer                  |
| **projects** | O2O 스마트오더, 여행 챗봇, 커머스 |

### O2O 스마트오더 백엔드 리드

<https://ara.live>

- 19.03 ~ current
- AWS[lambda, api-gateway, s3, secret manager, route53, cloudfront, dynamodb, aurora rds, cloudformation], django, drf, python, mypy, unittest

#### Description

- 인프라 개발
- 테스트 자동화 파이프라인(CI) 구성 도입
- 배포 자동화 파이프라인(CD) 구성 도입
- 코드 리뷰 도입
- TDD 도입
- 서버리스 아키텍쳐 도입
- 챗봇(안드로이드 RCS)용, 웹 클라이언트용 REST API 개발
- 웹소켓 서버 개발

### O2O 스마트오더 POS 클라이언트

- 19.03 ~ 20.03
- nwjs 0.14.7, javascript(es5)

#### Description

- 지점에 설치되는 POS 클라이언트 개발
- windows xp와의 호환성을 위해 vanilla javascript(es5) 로 개발
- 웹소켓 서버와 통신하며 실시간 주문받기
- 시리얼통신으로 영수증 프린트
- 버전 업데이트시 자동 업데이터 개발

### 챗봇과 통신하는 메시징 서버

- 17.06 ~ 19.03
- AWS(elastic beanstalk, s3, route53, cloudfront, rds(postgresql)), django, drf, python

#### Description

- 클라이언트: 페이스북 메신저, 위챗, 자체 개발 메신저 위젯
- 클라이언트와 통신하는 소켓 서버
- NLP서버와 통신하는 서버 개발

### 커머스 웹사이트

<https://ara.travelflan.com/>  
<https://travelflan.com/>

- 17.06 ~ current
- AWS(ec2, beanstalk, api-gateway, ecs), alicloud(docker, k8s), Django, drf, Celery, python, aurora(postgresql)

#### Description

- REST API 서버 개발
- 웹소켓 서버 개발

## Ohmytrip

|              |                     |
| -----------: | ------------------- |
|   **period** | 16.07 ~ 17.06       |
| **position** | Full Stack Engineer |
| **projects** | 사내 백오피스 개발  |

### 사내 백오피스 개발

자사 웹사이트 백오피스 개발 및 운영

- 14.12 ~ 15.12
- aws, elastic beanstalk, Django v1.11, python v3.4, javascript, mysql

## Asiana IDT

|              |                  |
| -----------: | ---------------- |
|   **period** | 15.12 ~ 16.05    |
| **position** | Query writer     |
| **projects** | 산출용 쿼리 작성 |

### 산출용 쿼리 작성

보험사의 데이터베이스 오라클기반 대용량 DB 쿼리 작성

- 15.12 ~ 16.05
- Oracle RDBMS

## Justgo

|              |                     |
| -----------: | ------------------- |
|   **period** | 14.12 ~ 15.12       |
| **position** | Full Stack Engineer |
| **projects** | 사내 백오피스 개발  |

### 사내 백오피스 개발

자사 웹사이트 백오피스 개발 및 운영

- 14.12 ~ 15.12
- ASP.NET, C#, javascript, MSSQL

## GeoAngel robot tech

|              |                                             |
| -----------: | ------------------------------------------- |
|   **period** | 10.04 ~ 11.04, 군복무, 13.04 ~ 14.12        |
| **position** | Embedded Software Engineer                  |
| **projects** | C++ 교육용 키트 제작, 회로 설계, 프로그래밍 |

### C++ 교육용 키트 제작

ATmega128 프로세서를 활용한 교육용 키트 제작

- 10.04 ~ 14.12
- C++

#### Description

- 서보모터, RC모터, 화면 입출력, RS232 시리얼통신, led 점멸 등 간단한 실험들을 할 수 있는 키트 제작
  - 키트 제작을 위한 PCB 회로설계
  - 키트 예제를 위한 프로그래밍
  - 교육 자료 제작

<br />

# Contributions

## Django

> Deadline 이 있는 완벽주의자를 위한 웹 프레임워크.

GitHub: <https://github.com/django/django>

### Contributes

- postgres rangefields 의 범위 설명을 명확히 하는 [Ticket](https://code.djangoproject.com/ticket/31577)
- [Translated content becomes ugly due to line-down on new admin side-bar](https://code.djangoproject.com/ticket/32141)
- [Raise a descriptive error on update()/delete() operations following QuerySet.union(), intersection(), and difference().](https://code.djangoproject.com/ticket/31148)

## Zappa

> python을 위한 serverless 프레임워크

GitHub: <https://github.com/zappa/zappa>

### Contributes

- [Coldstart can be drastically reduced by calling LambdaHandler externally](https://github.com/zappa/Zappa/pull/982)

## Django-allauth

> 다양한 social oauth를 위한 3rd party 라이브러리

GitHub: <https://github.com/pennersr/django-allauth>

### Contributes

- [Added Daum OAuth2](https://github.com/pennersr/django-allauth/pull/1617)
- [Naver's extract_common_fields should be deleted.](https://github.com/pennersr/django-allauth/pull/1693)

<br />

<div align="center" class="final">

_감사합니다._

<br/>

<sub><sup>Back-End Engineer, <a href="https://github.com/xncbf">@xncbf</a></sup></sub>

</div>
