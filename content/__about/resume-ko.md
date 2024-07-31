---
title: 'about'
date: 2021-11-10 13:21:13
lang: 'ko'
---

# 김준환

<div align="right"><sub><i>Last updated: 2023.05.05</i></sub></div>

|             |                            |
| :---------: | -------------------------- |
| **GitHub**  | <https://github.com/xncbf> |
|  **Blog**   | <https://pypy.dev>         |
| **Contact** | <-py@kakao.com>            |

<br />

# 목차

- [경력](#experiences)
  - [FreeD Group](#freed-group)
  - [Ohmytrip](#ohmytrip)
  - [Asiana IDT](#asiana-idt)
  - [Justgo](#justgo)
  - [GeoAngel robot tech](#geoangel-robot-tech)
- [오픈소스 기여](#open-sources)
  - [django-dynamodb-cache](#django-dynamodb-cache)
  - [rcs-pydantic](#rcs-pydantic)
  - [Django](#django)
  - [Zappa](#zappa)
  - [Django-allauth](#django-allauth)

# Experiences

## FreeD Group

- [Harmony 이커머스 솔루션](#Harmony-이커머스-솔루션)
- [기업용 RCS 메세지 발송 시스템](#기업용-RCS-메세지-발송-시스템-(Rich-Communication-Service))
- [O2O 스마트오더 백엔드 리드](#o2o-스마트오더-백엔드-리드)
- [O2O 스마트오더 POS 클라이언트](#o2o-스마트오더-pos-클라이언트)
- [챗봇과 통신하는 메시징 서버](#챗봇과-통신하는-메시징-서버)
- [커머스 웹사이트](#커머스-웹사이트)

|              |                                                       |
| -----------: | ----------------------------------------------------- |
|   **period** | 17.06 ~ current                                       |
| **position** | Backend Engineer IV                                   |
|    **roles** | 백엔드 리드, DevOps, SRE, 아키텍트, AWS 전체 관리자, 기술인터뷰  |
| **projects** | Harmony 이커머스, RCS, O2O 스마트오더, 여행 챗봇, 커머스       |


### Harmony 이커머스 솔루션

<https://www.joinharmonycvm.com/ko>  
<https://shoppingeasy.co.kr/>

- 22.05 ~ current
- AWS (eks, secret manager, sqs, sns, route53, dynamodb, aurora rds, s3)
- fastapi, pydantic, python 3.10, sqlalchemy, mypy, pytest, ruff, black, MeiliSearch, docker, k8s

#### Description

- 아키텍트
- 데브옵스
- CI, CD 구성
- 모니터링 시스템 구성
- 이커머스 상품 서비스 개발

--------------

### 기업용 RCS 메세지 발송 시스템 (Rich Communication Service)

<https://www.rcsbizservice.com/ko/harmonyrcs-info>

- 21.11 ~ current
- AWS (eks, secret manager, route53, dynamodb, aurora rds)
- django 4.2, django-ninja, pydantic, python 3.10, mypy, pytest, ruff, black, docker, k8s

#### Description

- 아키텍트
- 데브옵스
- CI, CD 구성
- 모니터링 시스템 구성
- RCS 호환되는 라이브러리 개발 [rcs-pydantic](https://github.com/xncbf/rcs-pydantic)
- RCS 메세지 발송 시스템 개발
- RCS 양방향 챗봇 개발

----------------

### O2O 스마트오더 백엔드 리드

<https://arabiz.live>

- 19.03 ~ current
- AWS (lambda, api-gateway, s3, secret manager, route53, cloudfront, dynamodb, aurora rds)
- django 3.2, DRF, python 3.8, mypy, unittest

#### Description

- 아키텍트
- 데브옵스
- 테스트 자동화 파이프라인(CI) 구성 도입
- 배포 자동화 파이프라인(CD) 구성 도입
- 코드 리뷰 도입
- TDD 도입
- 서버리스 아키텍쳐 도입
- 챗봇(안드로이드 RCS)용, 웹 클라이언트용 REST API 개발
- 웹소켓 서버 개발

---------------

### O2O 스마트오더 POS 클라이언트

- 19.03 ~ 20.03
- nwjs 0.14.7, javascript(es5)

#### Description

- 지점에 설치되는 POS 클라이언트 개발
- windows xp와의 호환성을 위해 vanilla javascript(es5) 로 개발
- 웹소켓 서버와 통신하며 실시간 주문받기
- 시리얼통신으로 영수증 프린트
- 버전 업데이트시 자동 업데이터 개발

---------------

### 챗봇과 통신하는 메시징 서버

- 17.06 ~ 19.03
- AWS(elastic beanstalk, s3, route53, cloudfront, rds(postgresql)), django, drf, python

#### Description

- 클라이언트: 페이스북 메신저, 위챗, 자체 개발 메신저 위젯
- 클라이언트와 통신하는 소켓 서버
- NLP서버와 통신하는 서버 개발

---------------

### 커머스 웹사이트

<https://ara.travelflan.com/>  
<https://travelflan.com/>

- 17.06 ~ 20.01
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

# Open sources

## django-dynamodb-cache

> django cache framework 와 호환되는 dynamodb 캐시 백엔드

GitHub: <https://github.com/xncbf/django-dynamodb-cache>

### Contributes

저자

## rcs-pydantic

> 한국 통신사 rcs 를 위한 pydantic 모델

GitHub: <https://github.com/xncbf/rcs-pydantic>

### Contributes

저자

## Python
Github: <https://github.com/python/cpython>

### Contributes

- [gh-107895: Fix test_asyncio.test_runners when run it in CPython's "development mode"](https://github.com/python/cpython/pull/108168)
- [gh-107910 Update not needing newline error message](https://github.com/python/cpython/pull/107928)

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
