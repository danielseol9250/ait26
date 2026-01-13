---
layout: post
title:  "파이썬 강의 4편: 제어문 (if, elif, else)"
date:   2026-01-26 10:00:00 +0900
categories: [IT, 파이썬 강의]
tags: [파이썬, 제어문, if문, 조건문, elif]
description: "파이썬의 조건문(if, elif, else)을 배우고 다양한 실전 예제를 통해 조건에 따른 프로그램 흐름 제어를 학습합니다."
---

# 파이썬 강의 4편: 제어문 (if, elif, else)

## 제어문이란?

제어문은 프로그램의 실행 흐름을 제어하는 문장입니다. 조건에 따라 다른 코드를 실행할 수 있게 해줍니다.

## if 문

조건이 참일 때 코드를 실행합니다:

### 기본 형식

```python
if 조건:
    # 조건이 참일 때 실행할 코드
    실행할 코드
```

**주의사항:**
- 파이썬은 들여쓰기(Indentation)로 코드 블록을 구분합니다
- 들여쓰기는 4칸 스페이스 또는 탭을 사용합니다

### 예시

```python
age = 20

if age >= 18:
    print("성인입니다.")
```

## if-else 문

조건이 거짓일 때도 코드를 실행합니다:

```python
score = 85

if score >= 60:
    print("합격입니다!")
else:
    print("불합격입니다.")
```

## if-elif-else 문

여러 조건을 확인합니다:

```python
score = 85

if score >= 90:
    print("A등급")
elif score >= 80:
    print("B등급")
elif score >= 70:
    print("C등급")
else:
    print("F등급")
```

**elif는 여러 개 사용 가능:**
```python
temperature = 25

if temperature > 30:
    print("더워요")
elif temperature > 20:
    print("따뜻해요")
elif temperature > 10:
    print("시원해요")
else:
    print("추워요")
```

## 중첩 if 문

if 문 안에 또 다른 if 문을 사용할 수 있습니다:

```python
age = 25
has_license = True

if age >= 18:
    if has_license:
        print("운전 가능합니다.")
    else:
        print("면허가 필요합니다.")
else:
    print("미성년자입니다.")
```

## 조건식 (삼항 연산자)

간단한 조건문을 한 줄로 작성할 수 있습니다:

```python
a = 10
b = 5

# 방법 1: if-else 사용
max_value = a if a > b else b
print(max_value)  # 10

# 방법 2: 일반 if문
if a > b:
    max_value = a
else:
    max_value = b
```

## 논리 연산자와 함께 사용

```python
age = 25
score = 85

# and 연산자
if age >= 18 and score >= 60:
    print("성인이고 합격입니다.")

# or 연산자
if age < 18 or age > 65:
    print("할인 대상입니다.")

# not 연산자
if not (age < 18):
    print("성인입니다.")
```

## 실전 예제: 성적 계산기

```python
score = float(input("점수를 입력하세요: "))

if score < 0 or score > 100:
    print("잘못된 점수입니다. (0-100 사이)")
elif score >= 90:
    print("A등급입니다.")
elif score >= 80:
    print("B등급입니다.")
elif score >= 70:
    print("C등급입니다.")
elif score >= 60:
    print("D등급입니다.")
else:
    print("F등급입니다.")
```

## 실전 예제: 계산기 프로그램

```python
num1 = float(input("첫 번째 숫자: "))
operator = input("연산자를 입력하세요 (+, -, *, /): ")
num2 = float(input("두 번째 숫자: "))

if operator == "+":
    result = num1 + num2
elif operator == "-":
    result = num1 - num2
elif operator == "*":
    result = num1 * num2
elif operator == "/":
    if num2 != 0:
        result = num1 / num2
    else:
        print("0으로 나눌 수 없습니다!")
        result = None
else:
    print("잘못된 연산자입니다!")
    result = None

if result is not None:
    print(f"{num1} {operator} {num2} = {result}")
```

## 실전 예제: 로그인 시스템

```python
correct_username = "admin"
correct_pass = "1234"

username = input("사용자명: ")
user_pass = input("비밀번호: ")

if username == correct_username and user_pass == correct_pass:
    print("로그인 성공!")
else:
    print("로그인 실패! 사용자명 또는 비밀번호가 잘못되었습니다.")
```

## 실전 예제: 윤년 판별

```python
year = int(input("년도를 입력하세요: "))

if (year % 4 == 0 and year % 100 != 0) or (year % 400 == 0):
    print(f"{year}년은 윤년입니다.")
else:
    print(f"{year}년은 평년입니다.")
```

## 실전 예제: BMI 계산기

```python
weight = float(input("몸무게(kg): "))
height = float(input("키(m): "))

bmi = weight / (height ** 2)

print(f"BMI: {bmi:.2f}")

if bmi < 18.5:
    print("저체중입니다.")
elif bmi < 25:
    print("정상입니다.")
elif bmi < 30:
    print("과체중입니다.")
else:
    print("비만입니다.")
```

## in 연산자와 함께 사용

```python
fruits = ["사과", "바나나", "오렌지"]

fruit = input("과일 이름: ")

if fruit in fruits:
    print(f"{fruit}이(가) 목록에 있습니다.")
else:
    print(f"{fruit}이(가) 목록에 없습니다.")
```

## pass 문

아무것도 하지 않지만 문법상 필요할 때 사용:

```python
age = 20

if age >= 18:
    pass  # 나중에 코드를 추가할 예정
else:
    print("미성년자입니다.")
```

## 주의사항

1. **들여쓰기**: 반드시 일관되게 사용 (4칸 스페이스 권장)
2. **콜론(:)**: if, elif, else 뒤에 반드시 콜론 필요
3. **비교 연산자**: `==` (같음), `!=` (다름) 주의

## 다음에 공부할 내용

다음 포스트에서는 반복문(for, while)에 대해 공부해보겠습니다.

