---
layout: post
title:  "파이썬 강의 3편: 연산자"
date:   2026-01-25 10:00:00 +0900
categories: [IT, 파이썬 강의]
tags: [파이썬, 연산자, 산술연산, 논리연산]
description: "파이썬의 산술 연산자, 비교 연산자, 논리 연산자, 할당 연산자 등을 배우고 실전 예제를 통해 활용합니다."
---

# 파이썬 강의 3편: 연산자

## 연산자란?

연산자(Operator)는 데이터를 처리하기 위한 기호입니다. 파이썬에는 다양한 연산자가 있습니다.

## 산술 연산자

기본적인 수학 연산을 수행합니다:

```python
a = 10
b = 3

print(a + b)  # 덧셈: 13
print(a - b)  # 뺄셈: 7
print(a * b)  # 곱셈: 30
print(a / b)  # 나눗셈: 3.333...
print(a // b) # 몫: 3 (정수 나눗셈)
print(a % b)  # 나머지: 1
print(a ** b) # 거듭제곱: 1000
```

**주의사항:**
- `/` : 항상 실수 결과 반환
- `//` : 정수 나눗셈 (몫)
- `**` : 거듭제곱 연산자

## 비교 연산자

두 값을 비교하여 `True` 또는 `False`를 반환합니다:

```python
a = 10
b = 5

print(a == b)  # 같음: False
print(a != b)  # 다름: True
print(a > b)   # 큼: True
print(a < b)   # 작음: False
print(a >= b)  # 크거나 같음: True
print(a <= b)  # 작거나 같음: False
```

## 논리 연산자

논리 연산을 수행합니다:

```python
a = True
b = False

print(a and b)  # AND: False
print(a or b)   # OR: True
print(not a)    # NOT: False
```

**진리표:**

| A | B | A and B | A or B | not A |
|---|---|---------|--------|-------|
| True | True | True | True | False |
| True | False | False | True | False |
| False | True | False | True | True |
| False | False | False | False | True |

## 할당 연산자

변수에 값을 할당합니다:

```python
a = 10
a += 5   # a = a + 5와 동일
print(a) # 15

a -= 3   # a = a - 3와 동일
print(a) # 12

a *= 2   # a = a * 2와 동일
print(a) # 24

a /= 4   # a = a / 4와 동일
print(a) # 6.0

a //= 2  # a = a // 2와 동일
print(a) # 3.0

a %= 2   # a = a % 2와 동일
print(a) # 1.0

a **= 3  # a = a ** 3와 동일
print(a) # 1.0
```

## 멤버십 연산자

값이 시퀀스(리스트, 문자열 등)에 포함되어 있는지 확인:

```python
numbers = [1, 2, 3, 4, 5]

print(3 in numbers)      # True
print(10 in numbers)     # False
print(3 not in numbers) # False
```

## 식별 연산자

두 객체가 같은 객체인지 확인:

```python
a = [1, 2, 3]
b = [1, 2, 3]
c = a

print(a is b)      # False (다른 객체)
print(a is c)      # True (같은 객체)
print(a == b)      # True (값은 같음)
```

**`is` vs `==`:**
- `is`: 같은 객체인지 확인 (메모리 주소 비교)
- `==`: 값이 같은지 확인

## 연산자 우선순위

연산자는 우선순위에 따라 실행됩니다:

1. 괄호 `()`
2. 거듭제곱 `**`
3. 곱셈, 나눗셈, 나머지 `*`, `/`, `//`, `%`
4. 덧셈, 뺄셈 `+`, `-`
5. 비교 연산자 `<`, `>`, `<=`, `>=`, `==`, `!=`
6. 논리 NOT `not`
7. 논리 AND `and`
8. 논리 OR `or`

**예시:**
```python
result = 2 + 3 * 4      # 14 (곱셈이 먼저)
result2 = (2 + 3) * 4   # 20 (괄호가 먼저)
```

## 실전 예제: 계산기 프로그램

```python
# 간단한 계산기
num1 = float(input("첫 번째 숫자: "))
operator = input("연산자 (+, -, *, /): ")
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

## 실전 예제: 성적 판정

```python
score = float(input("점수를 입력하세요: "))

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
elif score >= 60:
    grade = "D"
else:
    grade = "F"

print(f"점수: {score}, 등급: {grade}")
```

## 실전 예제: 짝수/홀수 판별

```python
number = int(input("숫자를 입력하세요: "))

if number % 2 == 0:
    print(f"{number}는 짝수입니다.")
else:
    print(f"{number}는 홀수입니다.")
```

## 문자열 연산

```python
# 문자열 연결
first_name = "홍"
last_name = "길동"
full_name = first_name + last_name
print(full_name)  # 홍길동

# 문자열 반복
print("Hello" * 3)  # HelloHelloHello

# 문자열 비교
print("apple" < "banana")  # True (사전순)
```

## 리스트 연산

```python
list1 = [1, 2, 3]
list2 = [4, 5, 6]

# 리스트 연결
combined = list1 + list2
print(combined)  # [1, 2, 3, 4, 5, 6]

# 리스트 반복
repeated = list1 * 2
print(repeated)  # [1, 2, 3, 1, 2, 3]

# 멤버십 확인
print(2 in list1)  # True
```

## 다음에 공부할 내용

다음 포스트에서는 제어문(if, elif, else)에 대해 공부해보겠습니다.

