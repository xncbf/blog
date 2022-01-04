---
title: git flow 회고 - azure devops 에서 권장하는 브랜치 전략
date: 2021-04-19 10:04:75
category: dev
thumbnail: { thumbnailSrc }
draft: false
---

- [개요](#개요)
- [git flow 가 뭔가요?](#git-flow-가-뭔가요)
- [왜 git flow 를 사용했나요?](#왜-git-flow-를-사용했나요)
- [어떤 점이 안좋았나요?](#어떤-점이-안좋았나요)
- [그럼 finish 만 안쓰면 되는거 아닌가요?](#그럼-finish-만-안쓰면-되는거-아닌가요)
- [trunk based development](#trunk-based-development)
  - [개발](#개발)
  - [릴리즈](#릴리즈)
  - [핫픽스](#핫픽스)
  - [스프린트 이동](#스프린트-이동)
- [결론](#결론)

## 개요

git 을 관리하는 기법이 정해진것은 없겠지만 그동안 제가 git flow 를 사용하면서 느꼈던점을 정리해 보려고 합니다

## git flow 가 뭔가요?

git flow 는 git 을 관리하는 도구 중 하나입니다.
![image](./images/4-0.png)

[git flow cheatsheet](https://danielkummer.github.io/git-flow-cheatsheet/index.ko_KR.html)
위 글은 제가 생각하기에 git flow 를 제일 잘 설명한 글입니다

## 왜 git flow 를 사용했나요?

시작은 이 글이었습니다. [우린 Git-flow를 사용하고 있어요](https://woowabros.github.io/experience/2017/10/30/baemin-mobile-git-branch-strategy.html)

우형 정도 회사라면 모든 부분이 배울점이라고 생각했기 때문에 무작정 따라서 사용해보게 되었습니다.

[내가 사용했던 git flow](https://pypy.dev/dev/%EC%9A%B0%EB%A6%AC%EA%B0%80-%EC%BD%94%EB%93%9C%EB%A5%BC-%EA%B4%80%EB%A6%AC%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95-%EC%BD%94%EB%93%9C-%EC%9E%91%EC%84%B1%EB%B6%80%ED%84%B0-%EB%B0%B0%ED%8F%AC%EA%B9%8C%EC%A7%80/)

저희 팀은 jira 대신 azure devops(구 vsts) 를 사용했고 git flow 설명에 나온대로 티켓마다 feature/\* 브랜치를 생성해 작업하고, 시스템에서 머지하는 방식을 채택했습니다.

## 어떤 점이 안좋았나요?

git flow 를 있는 그대로 사용하면 `git flow feature start` 와 같은 명령어로 시작하고 `git flow feature finish` 로 닫아야 합니다.

`git flow feature start` 를 하게되면 `develop` 브랜치에서 파생된 `feature` 브랜치가 생성되고

`git flow feature finish` 를 하면 다시 `develop` 브랜치로 머지되는 방식입니다.

그런데 저희 팀은 `azure devops` 에서 직접 리뷰된 코드를 merge하게 되면서 문제가 발생했습니다.

merge 를 했으니 이미 develop 에 반영된 상태이기 때문에 `git flow feature finish` 를 할 수 없게 됩니다.

`hotfix` 또는 `release` 브랜치를 `master` 에 직접 머지해야 한다는 불안감도 있었습니다.

## 그럼 finish 만 안쓰면 되는거 아닌가요?

이런 문제를 겪으면서 git flow 를 쓰는게 맞는것인가 하는 근본적인 의문점이 생겼습니다.

브랜치를 생성하는건 굳이 `git flow feature start` 를 하지 않아도 `git checkout -b feature/*` 를 하면 되기 때문입니다.

그리고 쓸모가 없는 master 브랜치에 대한 의문점도 함께 생겨났습니다

그래서 새롭게 찾아보았습니다

## trunk based development

[trunk based development](https://trunkbaseddevelopment.com/) 는 [azure devops 에서 추천했던 개발 전략](https://docs.microsoft.com/en-us/azure/devops/learn/devops-at-microsoft/release-flow)입니다

요약하자면 다음과 같습니다

### 개발

1. 모든 브랜치는 master 브랜치에서 파생됩니다. (모든 개발은 기존 develop 브랜치로 머지되던것과 다르게 master 로 머지됩니다.)
2. 새로운 기능은 `users/<username>/feature` 브랜치를 사용합니다.
3. [Pull Request](https://docs.microsoft.com/en-us/azure/devops/repos/git/pull-requests?view=azure-devops&viewFallbackFrom=vsts) 기능을 사용하여 마스터로 병합되는 방식을 제어합니다.
4. PR 은 브랜치 정책이 충족되도록 설정합니다 (최소 단위 리뷰어의 리뷰, 모든 코멘트 resolve, work item 연결, 테스트 파이프 라인 통과)
5. master 로 머지 후 시간이 더 많이 걸리는 추가 승인 테스트 실행
6. master 브랜치에서 계속 개발하고 스프린트 케이던스에 맞춰 3주 단위로 릴리즈합니다

### 릴리즈

- `releases/n` 브랜치를 생성하고 이 브랜치에서 직접 배포합니다.

### 핫픽스

1. 일반적인 개발 플로우와 동일하게 master 에서 분기를 만들고, 리뷰 후 master 에 머지합니다
2. master 에서 가장 마지막에 릴리즈되었던 `releases/n` 브랜치로 해당 핫픽스만 체리픽합니다.

### 스프린트 이동

1. 3주 후 새로운 스프린트에 기능 추가를 마치면 배포하기 위해 `releases/n+1` 브랜치를 만들고 배포합니다.
2. 이 상황에서 핫픽스가 발생하는 경우 n 브랜치와 n+1 브랜치에 둘 다 배포합니다.
3. 스프린트 배포가 완료되면 n 브랜치 이전의 브랜치는 완전히 버려집니다.

## 결론

정확히 이해하지 못하고 무작정 따라한 결과 새로운 개발 전략을 도입하게 되었습니다.

이제 trunk based development 전략을 사용하면 master 브랜치와 release 브랜치, feature 브랜치만 용도 명확하게 사용하게 될것입니다.

이 배포 전략을 수개월 사용해보고 후기를 남기겠습니다.

읽어주셔서 감사합니다.
