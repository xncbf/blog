---
title: 분당 메세지 12,000의 확장성이 있는 RCS messaging (문자메세지) 서비스 구현기
date: 2024-07-29 21:07:09
category: resume
thumbnail: { thumbnailSrc }
draft: true
---

- [개요](#개요)
- [요구사항](#요구사항)
- [서비스 다이어그램](#서비스-다이어그램)
- [구현 내용](#구현-내용)

## 개요

본 포스트는 본인이 RCS 메시징 서비스 프로젝트를 진행하면서 구현한 내용을 정리한 것이다.

## 요구사항

1. RCS 챗봇 서버에서 사용자로부터 받은 메세지를 가공해서 응답을 보내는 서비스를 구현한다.
2. 캠페인 메세지 대량 발송을 위해 비동기 메시지 전송 서비스를 구현한다.
3. 발송량(TPS)을 고려한 확장성 있는 서비스를 구현한다.

## 서비스 다이어그램
![RCS 메세지 서비스 아키텍처](/images/rcs-messaging-architecture.png)
```mermaid
sequenceDiagram
    participant User as 사용자 (휴대폰)
    participant RCS as RCS 챗봇 서버
    participant OrderHop as 테이블오더 서버
    participant Sender as RCS Sender 서버
    participant SenderDB as Sender DB
    participant SQS as Amazon SQS
    participant Worker as 비동기 워커
    participant Telecom as KT 통신사 서버

    User->>RCS: 양방향 문자메시지 전송
    RCS->>OrderHop: 데이터 요청 API 호출
    OrderHop-->>RCS: 데이터 응답
    RCS->>RCS: 답장 내용 초기 구성
    alt 이미지가 있는 경우
        RCS->>Sender: 이미지 업로드 요청
        Sender->>SenderDB: 이미지 존재 여부 확인
        alt 이미지가 DB에 없는 경우
            Sender->>Telecom: 이미지 업로드
            Telecom-->>Sender: 이미지 링크 반환
            Sender->>SenderDB: 이미지 링크 저장
        end
        Sender-->>RCS: 이미지 링크 반환
        RCS->>RCS: 이미지 링크를 답장 내용에 추가
    end
    RCS->>Sender: API 호출 (완성된 답장 내용 전달)
    Sender->>SQS: 메시지 전송 요청
    SQS->>Worker: 메시지 처리 요청
    Worker->>Telecom: 메시지 전송 요청
    Telecom-->>User: 메시지 전송
    Worker->>SenderDB: 메시지 발송 내역 저장
```

## 구현 내용

