---
layout: post
title:  "파이썬 강의 5편: 반복문 (for, while)"
date:   2026-01-27 10:00:00 +0900
categories: [IT, 파이썬 강의]
tags: [파이썬, 반복문, for문, while문, 반복]
description: "파이썬의 for문과 while문을 배우고, 리스트 순회, 범위 함수, 중첩 반복문 등 다양한 반복문 활용법을 학습합니다."
---

# 파이썬 강의 5편: 반복문 (for, while)

## 반복문이란?

반복문은 특정 코드를 여러 번 실행할 수 있게 해주는 제어문입니다. 같은 작업을 반복해야 할 때 유용합니다.

## for 문

정해진 횟수만큼 또는 시퀀스의 각 요소에 대해 반복합니다:

### 기본 형식

```python
for 변수 in 시퀀스:
    # 반복 실행할 코드
```

### 리스트 순회

```python
fruits = ["사과", "바나나", "오렌지"]

for fruit in fruits:
    print(fruit)
```

출력:
```
사과
바나나
오렌지
```

### range() 함수 사용

```python
# 0부터 9까지
for i in range(10):
    print(i)

# 1부터 10까지
for i in range(1, 11):
    print(i)

# 0부터 10까지, 2씩 증가
for i in range(0, 11, 2):
    print(i)
```

### 문자열 순회

```python
word = "Python"

for char in word:
    print(char)
```

출력:
```
P
y
t
h
o
n
```

## while 문

조건이 참인 동안 반복합니다:

### 기본 형식

```python
while 조건:
    # 반복 실행할 코드
```

### 예시

```python
count = 0

while count < 5:
    print(count)
    count += 1
```

출력:
```
0
1
2
3
4
```

## 반복문 제어

### break 문

반복문을 즉시 종료합니다:

```python
for i in range(10):
    if i == 5:
        break
    print(i)
```

출력:
```
0
1
2
3
4
```

### continue 문

현재 반복을 건너뛰고 다음 반복으로 넘어갑니다:

```python
for i in range(10):
    if i % 2 == 0:
        continue  # 짝수면 건너뛰기
    print(i)
```

출력:
```
1
3
5
7
9
```

### else 문

반복문이 정상적으로 완료되면 실행됩니다:

```python
for i in range(5):
    print(i)
else:
    print("반복 완료!")
```

**break로 종료되면 else 실행 안 됨:**
```python
for i in range(5):
    if i == 3:
        break
    print(i)
else:
    print("이 메시지는 출력되지 않습니다.")
```

## 중첩 반복문

반복문 안에 또 다른 반복문을 사용할 수 있습니다:

```python
# 구구단 출력
for i in range(1, 10):
    for j in range(1, 10):
        print(f"{i} x {j} = {i * j}")
    print()  # 줄바꿈
```

## 실전 예제: 숫자 합계 구하기

```python
# 1부터 100까지의 합
total = 0
for i in range(1, 101):
    total += i

print(f"1부터 100까지의 합: {total}")
```

## 실전 예제: 팩토리얼 계산

```python
n = int(input("숫자를 입력하세요: "))
factorial = 1

for i in range(1, n + 1):
    factorial *= i

print(f"{n}! = {factorial}")
```

## 실전 예제: 소수 찾기

```python
n = int(input("숫자를 입력하세요: "))
is_prime = True

if n < 2:
    is_prime = False
else:
    for i in range(2, int(n ** 0.5) + 1):
        if n % i == 0:
            is_prime = False
            break

if is_prime:
    print(f"{n}는 소수입니다.")
else:
    print(f"{n}는 소수가 아닙니다.")
```

## 실전 예제: 별 찍기

```python
# 직각삼각형
n = int(input("줄 수를 입력하세요: "))

for i in range(1, n + 1):
    print("*" * i)

# 역직각삼각형
for i in range(n, 0, -1):
    print("*" * i)

# 피라미드
for i in range(1, n + 1):
    spaces = " " * (n - i)
    stars = "*" * (2 * i - 1)
    print(spaces + stars)
```

## 실전 예제: 숫자 맞추기 게임

```python
import random

answer = random.randint(1, 100)
attempts = 0

print("1부터 100 사이의 숫자를 맞춰보세요!")

while True:
    guess = int(input("숫자를 입력하세요: "))
    attempts += 1
    
    if guess == answer:
        print(f"정답입니다! {attempts}번 만에 맞췄습니다.")
        break
    elif guess < answer:
        print("더 큰 숫자입니다.")
    else:
        print("더 작은 숫자입니다.")
```

## 실전 예제: 리스트 처리

```python
# 점수 리스트에서 평균 구하기
scores = [85, 90, 78, 92, 88]
total = 0

for score in scores:
    total += score

average = total / len(scores)
print(f"평균 점수: {average:.2f}")

# 최고점과 최저점 찾기
max_score = scores[0]
min_score = scores[0]

for score in scores:
    if score > max_score:
        max_score = score
    if score < min_score:
        min_score = score

print(f"최고점: {max_score}, 최저점: {min_score}")
```

## enumerate() 함수

인덱스와 값을 함께 가져올 수 있습니다:

```python
fruits = ["사과", "바나나", "오렌지"]

for index, fruit in enumerate(fruits):
    print(f"{index + 1}. {fruit}")
```

출력:
```
1. 사과
2. 바나나
3. 오렌지
```

## zip() 함수

여러 리스트를 동시에 순회할 수 있습니다:

```python
names = ["홍길동", "김철수", "이영희"]
ages = [25, 30, 28]

for name, age in zip(names, ages):
    print(f"{name}: {age}세")
```

출력:
```
홍길동: 25세
김철수: 30세
이영희: 28세
```

## 무한 루프

조건이 항상 참인 반복문입니다:

```python
# 무한 루프 (주의!)
while True:
    user_input = input("명령어를 입력하세요 (종료: quit): ")
    if user_input == "quit":
        break
    print(f"입력한 명령어: {user_input}")
```

## for vs while 언제 사용할까?

- **for**: 반복 횟수가 정해져 있거나 시퀀스를 순회할 때
- **while**: 조건에 따라 반복할 때

## 다음에 공부할 내용

다음 포스트에서는 함수에 대해 공부해보겠습니다.

