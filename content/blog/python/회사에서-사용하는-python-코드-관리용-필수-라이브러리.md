---
title: 회사에서 사용하는 python 코드 관리용 필수 라이브러리
date: 2022-01-02 13:01:15
category: python
thumbnail: { thumbnailSrc }
draft: false
---

- [개요](#개요)
- [환경](#환경)
- [python 설치](#python-설치)
- [python 가상환경 설정 pep405](#python-가상환경-설정-pep405)
- [코드 스타일 가이드 pep8](#코드-스타일-가이드-pep8)
- [정적 유형 검사 pep484](#정적-유형-검사-pep484)
- [시스템 빌드 종속성 관리 pep518](#시스템-빌드-종속성-관리-pep518)
- [코드 테스트](#코드-테스트)
- [마무리](#마무리)

## 개요

이 글은 python 입문자와 실무자들을 위해 작성되었습니다.  
python 을 사용하는 사람들이 점점 더 많아지고 있지만 python 을 보다 잘 사용하기 위해서,  
기본적이지만 입문자분들은 시행착오를 줄였으면 하는 마음으로 작성했습니다.  
여러 가지 선택지들 중 선택되지 않은 것들은 자세하게 설명하지 않을 것 입니다.

## 환경

mac os 를 기준으로 작성되었습니다.

## python 설치

python 을 설치하는 방법은 다양합니다.

1. [공홈](https://www.python.org/downloads/)에서 설치한다.
2. brew 로 설치한다.

여러 python 버전을 동시에 사용하기 위해서는 공홈에서 설치하는것이 더 편하기 때문에 저는 공홈에서 설치합니다.

## python 가상환경 설정 pep405

python 코드를 위해 사용되는 라이브러리들은 동일한 라이브러리라고 할지라도 프로젝트마다 다른 버전을 사용하게 됩니다.
따라서 격리된 가상 환경이 필요합니다.

이를 도와주는 python 패키지 매니저들이 여러 종류가 존재합니다.

1. [venv](https://docs.python.org/ko/3/tutorial/venv.html)
2. [virtualenv](https://virtualenv.pypa.io/en/latest/)
3. [pipenv](https://pipenv.pypa.io/en/latest/)
4. [poetry](https://github.com/python-poetry/poetry)
5. [conda](https://github.com/conda/conda)

제가 추천하는 패키지 매니저는 `poetry` 입니다

`poetry` 는 `pipenv` 와 비슷하지만 Dependency resolver 에서 조금 더 나은 성능을 제공합니다.
그리고 [pep518](https://www.python.org/dev/peps/pep-0518/) 에서 정의된 `pyproject.toml` 파일을 기반으로 설정하는 것이 가능합니다.

기본 사용법은 [이 글을 읽어보세요](https://python-poetry.org/docs/basic-usage/)

## 코드 스타일 가이드 pep8

`python` 은 대개 가독성을 위해 [pep8](https://www.python.org/dev/peps/pep-0008/) 코드 스타일 가이드를 따른다는 것을 들어 보셨을 것입니다.  
`pep8` 을 더 잘 따르기 위해서 코드 스타일 검사를 라이브러리를 통해서 할 수 있습니다.

다음 라이브러리들을 사용합니다.

1. [flake8](https://github.com/PyCQA/flake8)
2. [isort](https://github.com/PyCQA/isort)
3. [black](https://github.com/psf/black)

자세한 설명은 위 링크를 클릭해서 보시면 됩니다.

- `flake8` 은 pep8 룰 검사 도구입니다.
- `isort` 는 import 코드 정렬 도구입니다.
- `black` 은 자동 코드 포맷터 입니다.

isort 와 black 은 자동으로 코드를 변경해주시만 flake8 은 검사만 해주므로 [autoflake](https://github.com/myint/autoflake) 를 사용해서 자동으로 flake8 검사에 통과될 수 있도록 코드를 변경할 수 있습니다.

## 정적 유형 검사 pep484

`python` 은 type 이 없는 변수를 사용합니다.  
그렇기 때문에 더 많은 버그를 생성할 수 있고, 가독성이 떨어질 수 있습니다.  
`type hint` 기능은 `python3.5` 버전에서 새롭게 등장했고, 다음과 같은 코드를 작성할 수 있게 되었습니다.

```python
def greeting(name: str) -> str:
    return 'Hello ' + name
```

이 코드를 토대로 [mypy](https://github.com/python/mypy) 를 이용해서 `typing test` 를 할 수 있습니다.

## 시스템 빌드 종속성 관리 pep518

[pep518](https://www.python.org/dev/peps/pep-0518/) 에서 고안된 `pyproject.toml` 에는 빌드에 필요한 시스템 요구 사항 및 정보가 포함되어 있습니다.  
위에서 언급했던 것처럼 `peotry` 설정도 `pyproject.toml` 파일에서 설정하고 `isort`, `black`, `pytest` 같은 대부분의 설정 파일을 해당 파일에 정리함으로써 설정 파일을 깔끔하게 관리 할 수 있습니다.

## 코드 테스트

이제 `TDD`를 하지 않는 회사를 찾아보기 힘든 것 처럼 `code test` 는 정식 규격처럼 자리잡았습니다.
[unittest](https://docs.python.org/ko/3/library/unittest.html) 와 [pytest](https://docs.pytest.org/) 라는 선택지가 있습니다. (이제 nose 는 잘 사용하지 않아요.)

`unittest` 만으로도 기본적인 테스트를 하기에 충분한 기능이 들어있지만 성능 프로파일링 등이 필요할때는 `pytest` 를 사용해야 합니다. `unittest` 로 어렵게 얻을 수 있는 기능들은 `pytest` 로 쉽게 얻을 수 있습니다.

## 마무리

더 나은 코드 퀄리티를 위해 필수로 사용해야 하는 라이브러리들을 정리해 봤습니다.  
각 항목에 대한 자세한 사용법 및 내용은 따로 포스트할 예정입니다.  
읽어주셔서 감사합니다.
