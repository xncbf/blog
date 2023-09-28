---
title: gpt api, 프롬프트를 이용한 사내 챗봇 개발기
date: 2023-09-28 18:07:00
category: llm
thumbnail: { thumbnailSrc }
draft: false
---

- [개요](#개요)
- [기존의 방식](#기존의-방식)
- [gpt api 를 이용한 방식](#gpt-api-를-이용한-방식)
- [개발 과정](#개발-과정)
- [결론](#결론)


## 개요

우리 회사에는 비밀리에 사용되고있는 사내 챗봇이 있다. ~~(사실 아직 개발중이라 공개하지 않았음)~~

최근 한가지 gpt api 로 개발된 기능이 있는데 그것은 회의실 현황을 확인하는 기능이다.

이 기능을 만들기까지의 과정을 간단하게 정리해보려고 한다.

## 기존의 방식

최근 LLM 이 급부상하기 전까지는 채팅으로 회의실 현황을 확인하는 기능을 만들기 위해서 자연어로 인풋을 받는다는것은 너무 복잡한 기능을 야기했기 때문에 정확한 인풋을 요구하는 방식으로 만들었어야 했다.

예를 들면 다음과 같이 날짜, 회의실 이름, 시간 등등을 각 대화마다 인풋으로 받아서 처리해야 했다.
```
A: /회의실
BOT: 어떤 회의실을 확인하시겠습니까? (ex: n층)
A: 2층
BOT: 어떤 날짜를 확인하시겠습니까? (ex: mm.dd)
A: 11.15
BOT: 어떤 시간을 확인하시겠습니까? (ex: nn시)
A: 10시
BOT: 1번 회의실은 오늘 10시에 사용중입니다.
```

하지만 각 인풋마다 정해진 정확한 타입을 입력하지 않으면 원하는 결과를 얻지 못 할 수도 있다.

그렇기 때문에 챗봇으로 무언가를 만든다는것은 유저 사용 경험이 현저히 떨어지기 때문에 웹사이트에서 처리하는것이 훨씬 더 만들기에도, 사용하기에도 쉬웠다.

## gpt api 를 이용한 방식

OpenAI 에서 공개한 gpt api 를 이용하면 위와 같은 번거로운 프로세스를 거치지 않고도 다음과 같이 자연어로 인풋을 받아서 처리할 수 있게 되었다.

```
A: 2층 회의실 오늘 10시 비었는지 확인해줘.
BOT: 1번 회의실은 오늘 10시에 사용중입니다.
```

이와 같은 프로세스는 어떻게 개발할 수 있었을까?

다음과 같은 개발 과정이 있었다.

## 개발 과정

회의실 현황을 조회하기 위해 gpt 가 사용되는 구간은 총 두군데이다.

다른 기능으로도 챗봇을 사용할 수 있기 때문에 어떤 기능을 사용하는지 카테고라이징 하는 첫번째 구간, 대화 내용을 파싱하는 두번째 구간, 이렇게 두군데로 나뉜다.

다음은 질문을 파싱하는 첫번째 구간의 코드이다.

```py
async def get_parsed_question_category(user_input) -> int | None:
    try:
        response = openai.ChatCompletion.create(
            model="gpt-3.5-turbo",
            messages=[
                {
                    "role": "user",
                    "content": f"""
                        사용자 질문을 ---로 구분된 카테고리중에 적절한 값을 골라서 int 타입의 value와 "category" key 를 가진 JSON 타입으로 리턴해줘

                        ---
                        명령어 도움 요청: 0
                        미팅룸 확인: 1
                        그 외: 2
                        ---
                        사용자 질문: {user_input}
                        예시: {{"category": 1}}
                    """,
                },
            ],
            timeout=30,
        )
        return json.loads(response["choices"][0]["message"]["content"])["category"]
    except Exception:
        return None
```
위 코드에서는 사용자의 입력을 자연어로 받아서 여러가지 카테고리중에 한가지를 선택하도록 하는 프롬프트 명령어를 사용했다.

그 결과 `get_parsed_question_category` 메서드는 항상 정확한 포멧으로 결과를 리턴해주기 때문에 안정적으로 사용할 수 있었다.

다음은 질문에서 값을 파싱하는 두번째 구간의 코드이다.
```py
async def get_parsed_question_for_meeting_schedule(user_input) -> dict:
    # Send the user question to the model and get a response
    start_time = datetime.now()
    try:
        response = openai.ChatCompletion.create(
            model="gpt-3.5-turbo",
            messages=[
                {
                    "role": "user",
                    "content": f"""
                        현재시간: {start_time.strftime('%Y년 %m월 %d일 %H시')}
                        사용자의 질문을 파싱하고 날짜, 시간, 몇층인지에 대한 데이터를 'datetime', 'floor' 키를 가지는 JSON 타입으로 리턴해줘
                        층에 대한 정보가 없다면 null로 리턴해줘
                        floor는 list 에 포함된 int, datetime은 타임존 없는 ISO8601 포맷으로 리턴해줘
                        사용자 질문: {user_input}
                    """,
                },
            ],
            timeout=30,
        )

        # Print the model's response
        return json.loads(response["choices"][0]["message"]["content"])
    except Exception as e:
        print(response["choices"][0]["message"]["content"] + "<br>" + str(e))
        return None
```

두번째 구간 또한 첫번째 구간과 비슷한 방식으로 프롬프트 명령어를 사용했다.

다만 첫번째 구간과 다른점은 오늘, 내일 등 상대적 날짜를 인풋으로 입력하는 경우를 대비하기 위해 현재 시간을 알려주었고, `날짜`, `시간`, `몇 층` 인지에 대한 데이터를 파싱할 수 있도록 했다.

따라서 `2층 회의실 오늘 10시 비었는지 확인해줘.` 와 같은 사용자 입력을 받는다면 위 메서드는 다음과 같은 결과를 리턴해준다.
```
{
    "datetime": "2023-09-28T10:00:00",
    "floor": [2]
}
```

위 정보를 바탕으로 사내 시스템에 입력된 데이터를 조회해서 결과를 리턴해주어 다음과 같이 채팅방에서 바로 확인할 수 있었다.

![image](./images/internal-chatbot-development-using-gpt-api-1.png)

## 결론

[앤드류 응 교수가 무료로 공개한 프롬프트 강의](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/)를 듣기 전까지는 gpt 를 사용하면 답변이 일정하지 않을 것 같다는 걱정이 있었다.

하지만 강의를 듣고 나서는 프롬프트를 잘 만들면 일정한 답변을 얻을 수 있다는 것을 알게 되었고 이를 통해 사내 챗봇에 적용할 수 있었다.

프롬프트 엔지니어링에 관심이 있다면 반드시 들어보길 추천한다.
