---
title: 'about'
date: 2024-08-15 13:21:13
lang: 'ko'
---

# 김준환

<div align="right"><sub><i>Last updated: 2024.08.15</i></sub></div>

|             |                            |
| :---------: | -------------------------- |
| **GitHub**  | <https://github.com/xncbf> |
|  **Blog**   | <https://pypy.dev>         |
| **Contact** | <-py@kakao.com>            |

<br />

# 소개

서비스를 만드는것을 좋아하고 최근에는 백엔드 엔지니어로서 다양한 도메인에 대한 경험을 쌓았습니다.  
사이드 프로젝트와 오픈소스 기여를 통해 다양한 기술에 대한 이해를 높이고 있습니다.

# 핵심 역량

여행, 커머스, 챗봇, 메세지 발송, 테이블 오더 등 다양한 도메인을 거치면서 백엔드 엔지니어로서 MAU 30만, DAU 15만 이상의 분산시스템에 대한 경험을 쌓았습니다.  
- 백엔드 파트 리드로서 각 프로젝트당 3명의 팀원을 리딩.  
- 백엔드 전체 서비스의 클라우드 인프라를 관리.  
- 개발 프로세스 개선을 위해 CI/CD 도입, 코드 리뷰 도입, TDD 도입 등을 진행.  
- 모니터링 시스템(APM)을 구성하고 경보 시스템을 구성.  
- 2020년도부터 테크니컬 인터뷰어.


# 경력

## 프리디소프트

|              |                                                       |
| -----------: | ----------------------------------------------------- |
|   **기간** | 17.06 ~ 현재                                       |
| **포지션** | Backend Engineer IV                                   |
|    **역할** | 백엔드 프로젝트 파트 리드  |
| **프로젝트** | 커머스, RCS, 테이블오더, 챗봇                               |


### Harmony 이커머스 솔루션

이커머스 마켓을 쉽게 구성할 수 있게 도와주는 기업용 커머스 B2B 솔루션.  
아래 사이트 외에도 수십개의 사이트를 운영중입니다.

- [현대카드 M포인트몰](https://shoppingeasy.co.kr/)
- [BMW 고객용 쇼핑몰](https://bmwjoymall.com/) (BMW 앱에서만 접근가능)
- KB PAY 구독몰 (KB PAY 앱에서만 접근가능)
- [농카 구독몰](https://nongca.co.kr) (농협카드 앱에서만 접근가능)
- [토이플러스](https://toyplus.co.kr/)
- [온누리쇼핑](https://onnurishop.co.kr/)

|              |                                                       |
| -----------: | ----------------------------------------------------- |
|   **기간** | 22.05 ~ 현재                                       |
| **기술스택** | fastapi, sqlalchemy, python, k8s, AWS                          |
|    **역할** | 백엔드 프로젝트 파트 리드 (기여도 20%, 팀원 3명) |

#### 기여

- 파이프라인 성능/비용 개선 [[관련 블로그 글]](https://pypy.dev/dev/vmss/)
- ecs 환경에서 eks 환경으로 무중단 마이그레이션
- newrelic apm 모니터링 구성
- CI/CD 환경 구성
- 데이터베이스 모델링 및 REST API 개발
- swagger 통합 문서 구성 [[관련 블로그 글]](https://pypy.dev/msa/micro-service-architecture-swagger-%ED%86%B5%ED%95%A9%ED%95%98%EA%B8%B0-(openapi)/)

--------------

### 기업용 RCS 메세지 발송 시스템 (Rich Communication Service)

일일 1000만 건 이상의 메시지 처리 능력을 갖춘 기업용 양방향 문자메세지 발송 시스템
<https://www.rcsbizservice.com/ko/harmonyrcs-info>

|              |                                                       |
| -----------: | ----------------------------------------------------- |
|   **기간** | 21.11 ~ 현재                                       |
| **기술스택** | django, django-ninja, k8s, AWS                          |
|    **역할** | 백엔드 프로젝트 리드 (기여도 30%, 팀원 3명) |

#### 기여

- 전체 시스템 구성도와 속도 / 중복발송 등 이슈 해결 [[관련 블로그 글]](https://pypy.dev/resume/rcs-messaging/)
- lambda 콜드스타트 개선 [[관련 블로그 글]](https://pypy.dev/serverless/django-+-zappa-%EB%8F%99%EC%8B%9C%EC%84%B1-%EB%AC%B8%EC%A0%9C-%ED%95%B4%EA%B2%B0-%EC%8B%9C%EB%8F%84-(cold-start)/)
- lambda 환경에서 eks 환경으로 무중단 마이그레이션
- CI/CD 환경 구성
- aws 경보 구성 [[관련 블로그 글]](https://pypy.dev/aws/sending-traffic-alert-to-messenger/)
- newrelic apm 모니터링 및 경보 구성
- RCS 호환되는 라이브러리 개발 [rcs-pydantic](https://github.com/xncbf/rcs-pydantic)
- 데이터베이스 모델링 및 REST API 개발

----------------

### 테이블오더 솔루션 (오더홉)

QR 코드와 nfc 태그를 통한 주문 시스템을 제공하는 B2B 솔루션

|              |                                                       |
| -----------: | ----------------------------------------------------- |
|   **기간** | 19.03 ~ 현재                                       |
| **기술스택** | django, django-ninja, drf, python, k8s, AWS                          |
|    **역할** | 백엔드 프로젝트 리드 (기여도 30%, 팀원 3명) |

#### 기여

- CI/CD, 코드 리뷰, TDD 도입
- 서버리스 아키텍쳐 도입
- 웹소켓 서버 개발
- 데이터베이스 모델링 및 REST API 개발

---------------

### 테이블오더 (오더홉) POS 클라이언트
지점의 POS 컴퓨터에 설치되는 프로그램
주문을 받고, 영수증 출력, 주문 내역 조회 등의 기능을 제공

|              |                                                       |
| -----------: | ----------------------------------------------------- |
|   **기간** | 19.03 ~ 20.03                                       |
| **기술스택** | nwjs, nodejs, javascript(es5)                           |
|    **역할** | 백엔드 엔지니어 (기여도 50%, 팀원 2명) |

#### 기여

- 지점에 설치되는 POS 클라이언트 개발
- windows xp와의 호환성을 위해 vanilla javascript(es5) 로 개발
- 웹소켓 서버와 통신하며 실시간 주문받기
- 시리얼통신으로 영수증 프린트
- 버전 업데이트시 자동 업데이터 개발

---------------

### 챗봇과 통신하는 메시징 서버

|              |                                                       |
| -----------: | ----------------------------------------------------- |
|   **기간** | 17.06 ~ 19.03                                       |
| **기술스택** | django, drf, python, beanstalk                          |
|    **역할** | 백엔드 엔지니어 (기여도 20%, 팀원 5명) |

#### 기여

- 페이스북 메신저, 위챗, 자체 개발 메신저 위젯에서 발송되는 메세지 핸들링
- 클라이언트와 통신하는 소켓 서버 개발
- NLP서버와 통신하는 서버 개발

---------------

### 자사 커머스 웹사이트

|              |                                                       |
| -----------: | ----------------------------------------------------- |
|   **기간** | 17.06 ~ 20.01                                       |
| **기술스택** | django, drf, python, postgresql                          |
|    **역할** | 백엔드 엔지니어 (기여도 20%, 팀원 5명) |


#### 기여

- 데이터베이스 모델링 및 REST API 개발
- 웹소켓 서버 개발

<br />

# 기타 활동

## 사이드 프로젝트

### STUDIOFY (음악 연습실 찾는 앱)

|              |                                                       |
| -----------: | ----------------------------------------------------- |
|   **기간** | 24.01 ~ 현재                                        |
| **기술스택** | django, aws lambda, cloudflare, react, nextjs, flutter                          |
|    **site** | https://studiofy.kr |
|    **android** | https://play.google.com/store/apps/details?id=kr.studiofy.studiofy&hl=ko |
|    **ios** | https://apps.apple.com/kr/app/id6475640698 |

#### 기여

- 모든 부분 혼자 개발했습니다. (기여도 100%)
- 임베딩 검색 기능 구현 [[관련 블로그 글]](https://pypy.dev/llm/embedding-search/)


## 오픈소스 기여

### django-dynamodb-cache (저자)

> django cache framework 와 호환되는 dynamodb 캐시 백엔드

GitHub: <https://github.com/xncbf/django-dynamodb-cache>


### rcs-pydantic (저자)

> 한국 통신사 rcs 를 위한 pydantic 모델

GitHub: <https://github.com/xncbf/rcs-pydantic>


### Python

Github: <https://github.com/python/cpython>

#### 기여

- 이슈: test.test_asyncio.test_runners가 "개발 모드"에서 실패하는 이슈 [링크](https://github.com/python/cpython/issues/107895)  
  PR: [gh-107895: Fix test_asyncio.test_runners when run it in CPython's "development mode"](https://github.com/python/cpython/pull/108168)

- 이슈: 오류메세지의 가독성이 떨어지는 이슈 [링크](https://github.com/python/cpython/issues/107910)
  PR: [gh-107910 Update not needing newline error message](https://github.com/python/cpython/pull/107928)

### Django

> Deadline 이 있는 완벽주의자를 위한 웹 프레임워크.

GitHub: <https://github.com/django/django>

#### 기여

- postgres rangefields 의 범위 설명을 명확히 하는 [Ticket](https://code.djangoproject.com/ticket/31577)
- 한국어 세팅인 경우 관리자 모드에서 ui 가 깨지는 이슈 [Ticket](https://code.djangoproject.com/ticket/32141)
- QuerySet.union(), interchange(), difference() 다음에 update()/delete() 작업에 대한 설명적 오류를 발생시키는 이슈. [Ticket](https://code.djangoproject.com/ticket/31148)

### Zappa

> python을 위한 serverless 프레임워크

GitHub: <https://github.com/zappa/zappa>

#### 기여

- 이슈: 새롭게 올라온 동시성의 첫 호출에 콜드스타트가 8초 이상 걸리는 이슈
  PR: [Coldstart can be drastically reduced by calling LambdaHandler externally](https://github.com/zappa/Zappa/pull/982)

### Django-allauth

> 다양한 social oauth를 위한 3rd party 라이브러리

GitHub: <https://github.com/pennersr/django-allauth>

#### 기여

- Daum 로그인 기능 추가 [PR 링크](https://github.com/pennersr/django-allauth/pull/1617)
- 더 이상 지원되지 않는 네이버 로그인에 대한 기능 수정 [PR 링크](https://github.com/pennersr/django-allauth/pull/1693)

<br />


<div align="center" class="final">

_감사합니다._

<br/>

<sub><sup>Back-End Engineer, <a href="https://github.com/xncbf">@xncbf</a></sup></sub>

</div>
