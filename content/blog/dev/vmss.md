---
title: Azure DevOps 파이프라인 이슈/성능/비용 개선기
date: 2024-07-30 22:07:38
category: dev
thumbnail: { thumbnailSrc }
draft: false
---


## 개요

본 포스트는 본인이 Azure DevOps를 이용하여 구축한 CI/CD 파이프라인의 성능을 개선한 내용을 정리한 것이다.

## 이슈 발생

우리는 Azure DevOps를 이용하여 CI/CD 파이프라인을 구축하고 있었다. 

그러나, 최근 Azure DevOps 의 장애가 잦아지면서 [기본 제공되는 Microsoft 호스팅 에이전트](https://learn.microsoft.com/ko-kr/azure/devops/pipelines/agents/agents?view=azure-devops&tabs=yaml%2Cbrowser#microsoft-hosted-agents)으로 구동되는 CI/CD 파이프라인이 실행되지 않거나, 기본으로 제공되는 스크립트 task 가 실행되지 않는 이슈가 발생했다.

또한 기본 제공되는 에이전트의 I/O 성능이 좋지 않아서인지, 테스트 수행 이후 coverage report 를 생성하는 작업이 너무 오래 걸려서, CI/CD 파이프라인이 너무 오래 걸리는 문제가 종종 발생했다.

## 이슈 대응

이에 대응하여, Azure DevOps의 [VMSS (Virtual Machine Scale Set)](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/scale-set-agents?view=azure-devops) 를 이용하여 CI/CD 파이프라인을 구축하였고 Azure DevOps 의 장애에도 불구하고, VMSS 를 이용하여 구축한 CI/CD 파이프라인은 장애가 발생하지 않았다.

어떻게 구축하였는지는 다른 포스트에서 자세히 다루도록 하겠다.

VMSS 를 구축했을때 다음과 같은 기대 효과가 있었다.

1. 확장성 있는 CI/CD 파이프라인을 구축할 수 있다.
2. 더 빠른 속도로 CI/CD 파이프라인을 실행할 수 있다.
3. 더 저렴한 비용으로 CI/CD 파이프라인을 실행할 수 있다.

## 성능 개선

VMSS 를 이용하여 비싼 (성능이 좋은) VM 머신을 사용하면 속도가 크게 개선될것이라는 기대가 있었으나, 실제로는 그렇지 않았다.

그러나 기본 제공되는 에이전트는 완전히 stateless 하지만 VMSS 를 이용하면 docker 빌드를 할때 의존성 설치 레이어를 캐싱할 수 있어서, 두번째부터 실행되는 빌드에서는 실제로 빌드 속도가 10분에서 8분정도로 개선되었다.

## 비용 개선

Azure DevOps 에서 파이프라인 에이전트를 확장성있게 사용하려면 두가지 옵션이 있는데, Microsoft-hosted 와 Self-hosted 가 있다.

![Azure DevOps 에이전트 가격](/images/vmss-1.png)

위 제공된 이미지와 같이 2024년 7월 기준으로 

Microsoft-hosted 에이전트는 패러럴잡 1개당 한화로 ₩55,550원 이고, Self-hosted 에이전트는 패러럴잡 1개당 한화로 ₩20,831원 이다.

20개의 패러럴잡을 사용한다고 가정하면, Microsoft-hosted 는 ₩1,111,000원 이고, Self-hosted 는 ₩416,620원 이다.

즉, Self-hosted 를 사용하면 Microsoft-hosted 에이전트를 사용하는것보다 62% 저렴하다.

VMSS는 Self-hosted 이므로 패러럴잡 비용 + VMSS 의 비용이 합산되어 계산되는데, 

A4 (8core, 14GB ram, 605 GB 임시스토리지) 1대를 기준 한달(730시간) 내내 켜놨을때 $385.44 이고,

VMSS 는 사용할때만 비용이 발생하므로, 코어 워킹타임 하루에 8시간, 20일 근무 기준으로 160시간 사용한다고 가정하면 $385.44 * 160 / 730 = $84.24 (약 116,502원) 이다.

얼마나 사용하는지에 따라서 비용이 달라지겠지만, 대부분의 경우에는 항상 모든시간에 파이프라인이 돌아가고 있지 않기 때문에, VMSS 를 사용해서 비용을 절감할 수 있었다.

## 결론

Azure DevOps 의 장애에 대응하기 위해 VMSS 를 이용하여 CI/CD 파이프라인을 구축하였고, 원했던 기대 효과인 확장성, 성능, 비용 개선을 얻을 수 있었다.