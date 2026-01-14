---
layout: post
title:  "LoRA로 LLM 파인튜닝하기: 효율적인 모델 학습 가이드"
date:   2026-03-15 09:00:00 +0900
categories: [IT, AI]
tags: [AI, LLM, LoRA, 파인튜닝, fine-tuning, Qwen, PEFT]
description: "LoRA를 사용하여 LLM을 효율적으로 파인튜닝하는 방법을 배우고, 실제 코드 예제를 통해 단계별로 이해합니다."
---

# LoRA로 LLM 파인튜닝하기: 효율적인 모델 학습 가이드

대규모 언어 모델(LLM)을 파인튜닝하는 것은 비용이 많이 든다. 전체 모델을 학습시키려면 수백 GB의 GPU 메모리가 필요하고, 학습 시간도 오래 걸린다. 

LoRA(Low-Rank Adaptation)는 이런 문제를 해결하는 효율적인 파인튜닝 기법이다. 전체 모델을 학습시키는 대신, 작은 어댑터 레이어만 학습시켜서 메모리 사용량과 학습 시간을 크게 줄인다.

이 글에서는 실제 코드를 통해 LoRA 파인튜닝의 전체 과정을 단계별로 살펴보겠다.

## LoRA란 무엇인가?

LoRA는 모델의 가중치를 직접 수정하는 대신, **저차원 행렬을 추가**하여 모델을 적응시키는 기법이다. 

원래 가중치 행렬 W를 다음과 같이 분해한다:

```
W' = W + ΔW
ΔW = BA (B는 r×d, A는 d×r 행렬, r << d)
```

여기서 r은 rank로, 보통 8~64 사이의 작은 값이다. 이렇게 하면 학습해야 하는 파라미터 수가 기하급수적으로 줄어든다.

## 전체 코드 구조

코드는 크게 다음과 같은 단계로 구성된다:

1. 데이터 로드 및 전처리
2. 모델 및 토크나이저 로드
3. LoRA 설정 적용
4. 학습 설정 및 실행
5. 모델 저장

각 단계를 자세히 살펴보자.

## 1. 필요한 라이브러리 임포트

```python
import argparse
import json
import os
from pathlib import Path

import torch
from datasets import Dataset
from peft import LoraConfig, get_peft_model
from transformers import AutoModelForCausalLM, AutoTokenizer
from trl import SFTConfig, SFTTrainer
```

- **argparse**: 명령줄 인자 파싱
- **torch**: PyTorch (딥러닝 프레임워크)
- **datasets**: Hugging Face 데이터셋 라이브러리
- **peft**: Parameter-Efficient Fine-Tuning 라이브러리 (LoRA 포함)
- **transformers**: Hugging Face의 모델/토크나이저 라이브러리
- **trl**: Transformer Reinforcement Learning (SFTTrainer 포함)

## 2. 데이터 로드 함수

```python
def load_jsonl(path: Path) -> list[dict]:
    items = []
    with path.open("r", encoding="utf-8") as f:
        for line in f:
            if line.strip():
                items.append(json.loads(line))
    return items
```

이 함수는 JSONL(JSON Lines) 형식의 파일을 읽는다. JSONL은 각 줄이 독립적인 JSON 객체인 형식이다.

**예시 JSONL 파일:**
```
{"messages": [{"role": "user", "content": "안녕"}, {"role": "assistant", "content": "안녕하세요!"}]}
{"messages": [{"role": "user", "content": "파이썬이 뭐야?"}, {"role": "assistant", "content": "프로그래밍 언어입니다."}]}
```

각 줄을 읽어서 JSON으로 파싱하고 리스트에 추가한다.

## 3. 채팅 템플릿 변환 함수

```python
def build_chat_text(example: dict, tokenizer: AutoTokenizer) -> str:
    """
    messages = [
      {"role": "user", "content": "..."},
      {"role": "assistant", "content": "..."}
    ]
    """
    messages = example["messages"]

    # Qwen / LLaMA 계열 chat template 사용
    text = tokenizer.apply_chat_template(
        messages,
        tokenize=False,
        add_generation_prompt=False,
    )
    return text
```

이 함수는 채팅 형식의 메시지를 모델이 이해할 수 있는 텍스트로 변환한다.

**입력 예시:**
```python
{
    "messages": [
        {"role": "user", "content": "안녕하세요"},
        {"role": "assistant", "content": "안녕하세요! 무엇을 도와드릴까요?"}
    ]
}
```

**출력 예시 (Qwen 템플릿):**
```
<|im_start|>user
안녕하세요<|im_end|>
<|im_start|>assistant
안녕하세요! 무엇을 도와드릴까요?<|im_end|>
```

`apply_chat_template`은 모델에 맞는 특수 토큰을 자동으로 추가해준다. `tokenize=False`는 텍스트 문자열을 반환하고, `add_generation_prompt=False`는 생성 프롬프트를 추가하지 않는다.

## 4. 메인 함수: 인자 파싱

```python
def main():
    parser = argparse.ArgumentParser()
    parser.add_argument("--base_model", default="Qwen/Qwen2.5-7B-Instruct")
    parser.add_argument("--train_path", required=True)
    parser.add_argument("--eval_path", default="")
    parser.add_argument("--output_dir", required=True)

    parser.add_argument("--max_seq_length", type=int, default=2048)
    parser.add_argument("--learning_rate", type=float, default=1e-4)
    parser.add_argument("--num_train_epochs", type=int, default=3)
    parser.add_argument("--batch_size", type=int, default=1)
    parser.add_argument("--gradient_accumulation_steps", type=int, default=4)
    parser.add_argument("--save_steps", type=int, default=100)
    parser.add_argument("--logging_steps", type=int, default=10)
    parser.add_argument("--seed", type=int, default=42)

    # LoRA
    parser.add_argument("--lora_r", type=int, default=16)
    parser.add_argument("--lora_alpha", type=int, default=32)
    parser.add_argument("--lora_dropout", type=float, default=0.05)

    args = parser.parse_args()
```

명령줄 인자를 정의한다. 주요 인자들:

- **base_model**: 파인튜닝할 기본 모델 (기본값: Qwen2.5-7B-Instruct)
- **train_path**: 학습 데이터 경로 (필수)
- **eval_path**: 평가 데이터 경로 (선택)
- **output_dir**: 모델 저장 경로 (필수)
- **max_seq_length**: 최대 시퀀스 길이 (토큰 수)
- **learning_rate**: 학습률
- **num_train_epochs**: 학습 에포크 수
- **batch_size**: 배치 크기
- **gradient_accumulation_steps**: 그래디언트 누적 스텝 (메모리 부족 시 사용)
- **lora_r**: LoRA rank (낮을수록 파라미터 적음, 기본값 16)
- **lora_alpha**: LoRA alpha (스케일링 파라미터, 기본값 32)
- **lora_dropout**: LoRA dropout 비율

## 5. GPU 메모리 최적화 설정

```python
    os.environ["PYTORCH_CUDA_ALLOC_CONF"] = "expandable_segments:True"
```

PyTorch의 CUDA 메모리 할당 방식을 최적화한다. `expandable_segments`는 메모리 단편화를 줄여서 더 효율적으로 GPU 메모리를 사용할 수 있게 해준다.

## 6. 토크나이저 로드

```python
    # tokenizer 먼저 로드 (chat template 사용)
    tokenizer = AutoTokenizer.from_pretrained(
        args.base_model,
        trust_remote_code=True,
    )
    tokenizer.pad_token = tokenizer.eos_token
    tokenizer.padding_side = "right"
```

토크나이저를 먼저 로드한다. `trust_remote_code=True`는 모델이 커스텀 코드를 포함할 경우 실행을 허용한다.

**중요한 설정:**
- `pad_token = eos_token`: 패딩 토큰을 EOS(End of Sequence) 토큰으로 설정. 일부 모델은 패딩 토큰이 없을 수 있다.
- `padding_side = "right"`: 패딩을 오른쪽에 추가. 대부분의 모델이 이 방식을 사용한다.

## 7. 데이터셋 준비

```python
    # dataset
    train_items = load_jsonl(Path(args.train_path))
    train_ds = Dataset.from_list(train_items)
    train_ds = train_ds.map(
        lambda ex: {"text": build_chat_text(ex, tokenizer)},
        remove_columns=train_ds.column_names,
    )
```

학습 데이터를 로드하고 전처리한다:

1. JSONL 파일을 읽어서 리스트로 변환
2. Hugging Face Dataset 객체로 변환
3. 각 예제를 채팅 템플릿 형식으로 변환
4. 원본 컬럼 제거 (메모리 절약)

**변환 과정:**
```
원본: {"messages": [...]}
     ↓
변환: {"text": "<|im_start|>user\n안녕<|im_end|>\n..."}
```

평가 데이터도 동일한 방식으로 처리한다:

```python
    eval_ds = None
    if args.eval_path:
        eval_items = load_jsonl(Path(args.eval_path))
        if eval_items:
            eval_ds = Dataset.from_list(eval_items)
            eval_ds = eval_ds.map(
                lambda ex: {"text": build_chat_text(ex, tokenizer)},
                remove_columns=eval_ds.column_names,
            )
```

## 8. 기본 모델 로드

```python
    # base model
    model = AutoModelForCausalLM.from_pretrained(
        args.base_model,
        torch_dtype=torch.float16,
        device_map="auto",
        trust_remote_code=True,
    )
```

기본 모델을 로드한다:

- **torch_dtype=torch.float16**: 반정밀도(float16) 사용으로 메모리 사용량 절반으로 감소
- **device_map="auto"**: 자동으로 GPU/CPU에 모델을 분산 배치 (멀티 GPU 지원)
- **trust_remote_code=True**: 커스텀 모델 코드 실행 허용

## 9. LoRA 설정 적용

```python
    # LoRA
    peft_config = LoraConfig(
        r=args.lora_r,
        lora_alpha=args.lora_alpha,
        lora_dropout=args.lora_dropout,
        bias="none",
        task_type="CAUSAL_LM",
        target_modules=[
            "q_proj", "k_proj", "v_proj",
            "o_proj", "gate_proj",
            "up_proj", "down_proj",
        ],
    )
    model = get_peft_model(model, peft_config)
    model.print_trainable_parameters()
```

LoRA 설정을 정의하고 모델에 적용한다:

**LoRA 파라미터 설명:**
- **r (rank)**: 저차원 행렬의 차원. 작을수록 파라미터가 적지만 표현력이 떨어질 수 있음 (기본값: 16)
- **lora_alpha**: LoRA 가중치의 스케일링 팩터. 보통 r의 2배로 설정 (기본값: 32)
- **lora_dropout**: 드롭아웃 비율. 과적합 방지 (기본값: 0.05)
- **bias**: bias 학습 여부. "none"은 학습하지 않음
- **task_type**: 작업 유형. "CAUSAL_LM"은 언어 모델링
- **target_modules**: LoRA를 적용할 모듈. Attention과 FFN의 주요 레이어들

**target_modules 설명:**
- `q_proj`, `k_proj`, `v_proj`: Query, Key, Value 프로젝션 (Attention)
- `o_proj`: Output 프로젝션 (Attention)
- `gate_proj`, `up_proj`, `down_proj`: Feed-Forward Network 레이어

`get_peft_model`로 모델을 래핑하면, 지정된 모듈에만 LoRA 레이어가 추가된다. `print_trainable_parameters()`는 학습 가능한 파라미터 수를 출력한다.

**예시 출력:**
```
trainable params: 8,388,608 || all params: 6,738,415,616 || trainable%: 0.12
```

전체 모델의 0.12%만 학습하므로 메모리와 시간이 크게 절약된다.

## 10. 학습 설정

```python
    # SFT config
    training_args = SFTConfig(
        output_dir=args.output_dir,
        dataset_text_field="text",
        max_length=args.max_seq_length,
        packing=False,
        num_train_epochs=args.num_train_epochs,
        per_device_train_batch_size=args.batch_size,
        gradient_accumulation_steps=args.gradient_accumulation_steps,
        optim="paged_adamw_32bit",
        save_steps=args.save_steps,
        logging_steps=args.logging_steps,
        learning_rate=args.learning_rate,
        warmup_ratio=0.03,
        lr_scheduler_type="cosine",
        seed=args.seed,
        report_to=[],
    )
```

SFT(Supervised Fine-Tuning) 학습 설정:

**주요 설정:**
- **dataset_text_field**: 데이터셋에서 사용할 텍스트 필드명
- **max_length**: 최대 시퀀스 길이 (토큰 수)
- **packing**: 여러 예제를 하나의 시퀀스로 패킹할지 여부 (False는 각 예제를 독립적으로 처리)
- **per_device_train_batch_size**: 디바이스당 배치 크기
- **gradient_accumulation_steps**: 그래디언트 누적. 배치 크기를 효과적으로 키울 수 있음
  - 실제 배치 크기 = batch_size × gradient_accumulation_steps × num_gpus
- **optim**: 옵티마이저. "paged_adamw_32bit"는 메모리 효율적인 AdamW
- **warmup_ratio**: 워밍업 비율 (전체 스텝의 3%)
- **lr_scheduler_type**: 학습률 스케줄러. "cosine"은 코사인 감쇠
- **report_to**: 로깅 대상 (빈 리스트는 로깅 안 함)

## 11. 트레이너 생성 및 학습

```python
    trainer = SFTTrainer(
        model=model,
        tokenizer=tokenizer,
        train_dataset=train_ds,
        eval_dataset=eval_ds,
        peft_config=peft_config,
        args=training_args,
    )

    print("Starting chat SFT training...")
    trainer.train()
```

SFTTrainer를 생성하고 학습을 시작한다. SFTTrainer는 Hugging Face의 Trainer를 확장하여 텍스트 생성 모델 학습에 최적화된 기능을 제공한다.

학습이 시작되면 다음과 같은 정보가 출력된다:
- 학습 손실 (loss)
- 학습률 (learning rate)
- 학습 속도 (samples/sec)
- 남은 시간 (estimated time)

## 12. 모델 저장

```python
    out_dir = Path(args.output_dir)
    out_dir.mkdir(parents=True, exist_ok=True)
    trainer.model.save_pretrained(out_dir)
    tokenizer.save_pretrained(out_dir)
```

학습이 완료되면 모델과 토크나이저를 저장한다. LoRA 모델은 기본 모델과 어댑터 가중치만 저장되므로 용량이 매우 작다 (보통 수십 MB).

## 사용 예시

코드를 실행하는 방법:

```bash
python train_lora.py \
    --base_model Qwen/Qwen2.5-7B-Instruct \
    --train_path data/train.jsonl \
    --eval_path data/eval.jsonl \
    --output_dir ./output \
    --max_seq_length 2048 \
    --learning_rate 1e-4 \
    --num_train_epochs 3 \
    --batch_size 1 \
    --gradient_accumulation_steps 4 \
    --lora_r 16 \
    --lora_alpha 32
```

## LoRA 파라미터 튜닝 가이드

**rank (r) 선택:**
- **r=8**: 매우 적은 파라미터, 빠른 학습, 낮은 표현력
- **r=16**: 균형잡힌 선택 (기본값)
- **r=32**: 더 많은 파라미터, 더 높은 표현력
- **r=64**: 많은 파라미터, 높은 표현력, 느린 학습

**alpha 선택:**
- 보통 r의 2배로 설정 (r=16이면 alpha=32)
- alpha가 클수록 LoRA 가중치의 영향이 커짐

**메모리 부족 시:**
- `batch_size`를 1로 설정
- `gradient_accumulation_steps`를 늘려서 실제 배치 크기 유지
- `max_seq_length`를 줄임
- `lora_r`을 줄임

## 전체 코드

전체 코드는 위의 단계들을 모두 포함한다. 각 부분이 어떻게 연결되는지 이해하면, 자신만의 파인튜닝 파이프라인을 구축할 수 있다.

## 결론

LoRA를 사용하면 전체 모델을 학습시키는 것보다 훨씬 적은 리소스로 LLM을 파인튜닝할 수 있다. 이 코드는 실제 프로덕션에서 사용할 수 있는 수준의 완전한 예제다.

주요 장점:
- **메모리 효율**: 전체 모델 대비 1% 미만의 파라미터만 학습
- **학습 속도**: 빠른 수렴과 짧은 학습 시간
- **유연성**: 여러 작업에 대해 서로 다른 LoRA 어댑터를 학습 가능
- **호환성**: 기본 모델은 그대로 유지하면서 어댑터만 교체 가능

이 코드를 기반으로 자신만의 데이터셋으로 모델을 파인튜닝해보자.

