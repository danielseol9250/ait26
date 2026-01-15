---
layout: post
title:  "LiteLLM Proxy 서버 모드와 Dashboard: 이걸 써야 하는 이유는 화면을 보면 바로 이해된다"
date:   2026-03-18 09:00:00 +0900
categories: [IT, AI]
tags: [AI, LLM, LiteLLM, Proxy, Dashboard, 운영, 모니터링]
description: "LiteLLM Proxy 서버 모드와 Dashboard의 역할과 사용법을 실전 관점에서 정리합니다."
---

# LiteLLM Proxy 서버 모드와 Dashboard
"이걸 써야 하는 이유는 화면을 보면 바로 이해된다"

**LiteLLM**은
라이브러리로만 써도 쓸 만하지만,
Proxy 서버 모드 + Dashboard까지 포함해야 기능의 의도가 명확해진다.

이 챕터에서는:

- 왜 Proxy가 필요한지
- Dashboard는 어디에 있고
- 거기서 뭘 보는지

만 정리한다.

## 1️⃣ LiteLLM Proxy 서버를 띄우면 생기는 것

LiteLLM을 Proxy 모드로 실행하면
LLM 호출의 구조가 이렇게 바뀐다.

```
Client / App
   ↓
LiteLLM Proxy
   ↓
OpenAI / Claude / Gemini
```

이 구조의 핵심은:

- **모든 LLM 요청이 Proxy를 반드시 거친다**
- 따라서 **모든 요청 정보를 중앙에서 수집할 수 있다**

이걸 "확인"하는 수단이 바로 Dashboard다.

## 2️⃣ LiteLLM Dashboard는 어디에 있나

LiteLLM Proxy를 실행하면
Dashboard는 기본적으로 같은 서버에 같이 뜬다.

**기본 접근 주소**
```
http://localhost:4000/ui
```

(포트는 설정에 따라 다를 수 있음)

공식 문서 기준 Dashboard 안내 페이지:
👉 [Dashboard 안내 페이지](https://docs.litellm.ai/docs/proxy/ui)

이 URL 하나만 알아두면 된다.

## 3️⃣ Dashboard에서 실제로 보는 핵심 정보

Dashboard는 화려하지 않다.
대신 운영에 바로 필요한 정보만 보여준다.

### 1. 모델별 요청 수 / 토큰 사용량

- 어떤 모델이
- 얼마나 호출되고
- 토큰을 얼마나 쓰는지

👉 **"지금 비용이 어디서 나가는지"** 바로 보인다.

### 2. 요청 단위 로그 (Request / Response 메타정보)

각 요청에 대해:

- 사용한 모델
- 토큰 수
- latency
- 성공 / 실패 여부

를 확인할 수 있다.

👉 **"이 호출 왜 느린지"**를
👉 코드가 아니라 Proxy 레벨에서 확인한다.

### 3. API Key / User / Service 기준 사용량

Proxy를 쓰는 이유 중 하나다.

- 어떤 Key가
- 어떤 서비스에서
- 얼마만큼 쓰고 있는지

를 나눠서 볼 수 있다.

👉 내부 실험 코드, 테스트 트래픽, 실제 서비스 트래픽이
👉 섞이지 않는다.

## 4️⃣ Proxy + Dashboard를 같이 봐야 하는 이유

Proxy만 있으면:

- 통제는 가능
- 하지만 **"지금 상태"는 감으로 추측**

Dashboard까지 있으면:

- 요청
- 비용
- 모델 사용 패턴

이 전부가 눈에 보인다.

그래서 LiteLLM은:

- **Proxy = 제어 레이어**
- **Dashboard = 관측 레이어**

로 같이 설계돼 있다.

## 5️⃣ 추천 사용 패턴 (간단하게)

**패턴**

- 앱 / 프론트엔드 → LiteLLM Proxy만 호출
- 모델 선택, fallback, 비용 관리는 Proxy 설정
- 상태 확인은 Dashboard에서만

이렇게 하면:

- 코드가 LLM Provider를 몰라도 되고
- 운영 판단은 화면 보고 하면 된다

## 정리

LiteLLM Proxy 서버 모드는
"중앙에서 호출을 막고" 끝나는 구조가 아니다.

Dashboard까지 포함해서:

- 누가
- 어떤 모델을
- 얼마나 쓰고 있는지

를 실시간으로 확인할 수 있을 때
비로소 의미가 생긴다.

**Proxy는 입구고,
Dashboard는 그 입구를 내려다보는 창이다.**

