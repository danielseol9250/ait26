---
layout: post
title:  "vLLM과 SGLang의 차이: 서빙 엔진 vs LLM 제어 언어"
date:   2026-03-14 09:00:00 +0900
categories: [IT, AI]
tags: [AI, LLM, vLLM, SGLang, 서빙, 추론, 엔진]
description: "vLLM과 SGLang의 본질적 차이를 이해하고, 각각이 해결하는 문제와 사용 시나리오를 명확히 구분합니다."
---

# vLLM과 SGLang의 차이: 서빙 엔진 vs LLM 제어 언어

LLM 서빙 스택을 다룰 때 자주 등장하는 두 도구가 있다. vLLM과 SGLang이다. 

겉보기에는 둘 다 "LLM을 실행한다"는 점에서 비슷해 보인다. 하지만 실제로는 완전히 다른 계층의 문제를 해결하는 도구다. 이 둘을 같은 선상에서 비교하려다 보면 항상 혼란이 생긴다.

이 글에서는 이 두 도구의 본질적 차이를 명확히 하고, 각각이 어떤 문제를 해결하는지, 그리고 실제로 어떻게 사용되는지 살펴보겠다.

## 가장 중요한 한 줄 정의

**vLLM은 "어떻게 빠르게 생성할 것인가"의 문제를 푸는 도구고, SGLang은 "LLM을 어떻게 사용·제어할 것인가"의 문제를 푸는 도구다.**

이 한 문장을 이해하면, 나머지는 자연스럽게 따라온다.

## vLLM: 추론을 위한 엔진

### vLLM이 해결하는 문제

vLLM이 풀고자 하는 문제는 매우 명확하다. GPU 위에서 LLM 토큰 생성을 최대한 빠르고 효율적으로 처리하는 것이다.

그래서 vLLM의 핵심 기술은 다음과 같다:

- **PagedAttention**: KV cache를 효율적으로 관리하여 메모리 사용량을 최적화
- **동적 batching**: 여러 요청을 효율적으로 묶어서 GPU utilization 극대화
- **KV cache 관리**: 중간 계산 결과를 재사용하여 불필요한 연산 제거
- **GPU 메모리 효율**: 제한된 GPU 메모리에서 최대한 많은 요청 처리

### vLLM의 사고 방식

vLLM은 LLM을 이렇게 본다:

> "LLM은 대량의 요청을 처리해야 하는 고비용 계산 자원이다."

따라서:
- 요청을 최대한 모아서
- GPU utilization을 극대화하고
- 응답을 빠르게 반환하는 것

이것이 최우선 목표다.

### 구조적 특징

vLLM은 **stateless**하다. 각 요청을 독립적으로 처리하며, 요청 간 상태를 유지하지 않는다.

```
Request → Prompt → Token Generation (GPU) → Response
```

vLLM은 "중간 판단"이나 "상태"에 관심이 없다. 오직 "다음 토큰을 얼마나 빠르게 생성하느냐"에만 집중한다.

### vLLM의 사용 시나리오

vLLM은 다음과 같은 상황에서 단독으로 사용된다:

- **챗봇**: 단순한 질문-답변 구조
- **대규모 동시 요청 처리**: 수백, 수천 개의 요청을 동시에 처리해야 하는 경우
- **latency/throughput 최우선**: 응답 속도와 처리량이 가장 중요한 경우

이런 경우 LLM은 단순히 "답변 생성기" 역할을 하며, 복잡한 로직은 거의 없다.

## SGLang: LLM을 위한 제어 언어 + 런타임

### SGLang이 해결하는 문제

SGLang이 풀고자 하는 문제는 전혀 다르다. LLM 호출을 단순한 문자열 생성이 아니라 "프로그램 가능한 추론 단계"로 다루는 것이다.

즉, 다음과 같은 흐름을 명시적으로 제어하고 싶다는 요구다:

- 여러 번 LLM을 호출하고
- 중간 결과를 저장하고
- 조건에 따라 분기하고
- tool을 실행하고
- 다시 LLM에 넘기는

### SGLang의 사고 방식

SGLang은 LLM을 이렇게 본다:

> "LLM은 추론 함수다. 논리는 우리가 짜고, LLM은 그중 일부 역할을 한다."

따라서:
- **상태(state)를 가진다**: 여러 단계에 걸쳐 정보를 유지
- **토큰 단위 제어를 한다**: 생성 과정을 세밀하게 제어
- **반복, 분기, 조건문이 자연스럽다**: 프로그래밍 언어처럼 제어 흐름을 표현

```python
state = {}

call LLM
update state

if state meets condition:
    call tool
else:
    call LLM again
```

이것은 서빙 엔진의 사고가 아니라 **프로그래밍 언어의 사고**다.

### SGLang의 사용 시나리오

SGLang은 다음과 같은 복잡한 워크플로우에서 사용된다:

- **에이전트 시스템**: 여러 단계의 추론과 행동이 필요한 경우
- **SOAR / Threat Intelligence**: 복잡한 분석 파이프라인
- **multi-step reasoning**: 여러 번의 LLM 호출이 필요한 추론 과정

## 계층 관점에서 본 차이

이 둘은 같은 계층에 있지 않다. 다음 계층 구조를 보면 명확하다:

```
[Application / Workflow]
        ↑
     SGLang
        ↑
     LLM API
        ↑
      vLLM
        ↑
      GPU
```

- **vLLM**: GPU 위에서 "어떻게 생성할 것인가"
- **SGLang**: LLM을 "어떻게 조합해서 쓸 것인가"

vLLM은 아래쪽 계층, SGLang은 위쪽 계층에 위치한다.

### 실제 현업에서의 조합

현실적인 고급 사용 사례에서는 이 둘을 함께 사용한다:

```
[SGLang Runtime]
   ├─ 판단
   ├─ 상태 관리
   ├─ 분기
   └─ tool 호출
         ↓
      [vLLM Server]
         ↓
        GPU
```

SGLang이 두뇌 역할을 하고, vLLM이 계산 엔진 역할을 한다.

## 성능 vs 통제력의 트레이드오프

이 둘을 비교할 때 중요한 것은 "우열"이 아니라 **의도된 트레이드오프**라는 점이다.

| 항목 | vLLM | SGLang |
|------|------|--------|
| 목적 | 최대 성능 | 최대 통제 |
| 상태 | 없음 | 있음 |
| 제어 흐름 | 없음 | 풍부 |
| 복잡도 | 낮음 | 높음 |
| 운영 난이도 | 낮음 | 높음 |
| 에이전트 적합성 | 낮음 | 높음 |

vLLM은 단순하고 빠르지만 제어가 제한적이다. SGLang은 복잡하지만 세밀한 제어가 가능하다.

## 왜 "vLLM vs SGLang" 비교가 자주 잘못되나

이 둘을 비교하는 글 대부분이 틀리는 이유는 단순하다. 서로 대체 관계가 아니기 때문이다.

"vLLM이 빠르다 / SGLang이 느리다" 또는 "vLLM이 낫다 / SGLang이 낫다"는 비교는 의미가 없다.

이것은 마치:
- DB 엔진 vs 애플리케이션 로직
- 컴파일러 vs 프로그래밍 언어

를 비교하는 것과 같다. 서로 다른 계층의 도구를 비교하는 것이다.

## LangGraph와의 관계

이 맥락에서 LangGraph의 위치도 명확해진다:

- **LangGraph**: 워크플로/상태 머신 레벨 - "어떤 단계로 갈 것인가"
- **SGLang**: LLM 호출 레벨 제어 - "LLM을 어떻게 쓸 것인가"
- **vLLang**: 추론 엔진 - "토큰을 어떻게 뽑을 것인가"

각각이 다른 추상화 레벨에서 작동한다.

## 실제 사용 예시

### vLLM 단독 사용

간단한 챗봇이나 QA 시스템에서 vLLM을 단독으로 사용한다:

```python
# vLLM 서버에 요청
response = vllm.generate(
    prompt="What is AI?",
    max_tokens=100
)
```

단순하고 빠르다. 하지만 복잡한 로직은 구현하기 어렵다.

### SGLang + vLLM 조합

복잡한 에이전트 시스템에서는 SGLang이 vLLM을 호출한다:

```python
# SGLang 런타임
state = initialize_state()

while not done:
    result = sglang.call_llm(
        prompt=build_prompt(state),
        backend="vllm"  # vLLM 서버 사용
    )
    
    state.update(result)
    
    if should_call_tool(state):
        tool_result = execute_tool(state)
        state.update(tool_result)
    
    if is_complete(state):
        done = True
```

SGLang이 제어 흐름을 관리하고, vLLM이 실제 추론을 수행한다.

## 결론

vLLM과 SGLang은 서로 경쟁하는 도구가 아니다. 각각 다른 문제를 해결하는 도구다.

- **vLLM**: "어떻게 빠르게 생성할 것인가" - 서빙 엔진
- **SGLang**: "LLM을 어떻게 사용·제어할 것인가" - 제어 언어

간단한 서빙이 필요하면 vLLM을, 복잡한 워크플로우가 필요하면 SGLang을, 그리고 둘 다 필요하면 함께 사용한다.

이 구분을 명확히 이해하면, LLM 서빙 스택을 설계할 때 어떤 도구를 선택해야 할지 판단하기 쉬워진다.

