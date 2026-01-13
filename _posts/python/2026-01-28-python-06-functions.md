---
layout: post
title:  "파이썬 강의 6편: 함수"
date:   2026-01-28 10:00:00 +0900
categories: [IT, 파이썬 강의]
tags: [파이썬, 함수, function, 모듈화, 재사용]
description: "파이썬 함수의 정의와 호출, 매개변수, 반환값, 기본값 인자, 가변 인자 등 함수의 모든 기능을 배웁니다."
---

# 파이썬 강의 6편: 함수

## 함수란?

함수(Function)는 특정 작업을 수행하는 코드 블록입니다. 코드를 재사용하고 모듈화할 수 있게 해줍니다.

## 함수의 장점

1. **코드 재사용**: 같은 코드를 여러 번 작성하지 않아도 됨
2. **모듈화**: 프로그램을 작은 단위로 나눌 수 있음
3. **가독성**: 코드가 더 읽기 쉬워짐
4. **유지보수**: 수정이 필요할 때 한 곳만 수정하면 됨

## 함수 정의

### 기본 형식

```python
def 함수명(매개변수):
    # 함수 본문
    return 값
```

### 예시: 간단한 함수

```python
def greet():
    print("Hello, World!")

# 함수 호출
greet()  # Hello, World!
```

## 매개변수가 있는 함수

```python
def greet(name):
    print(f"안녕하세요, {name}님!")

greet("홍길동")  # 안녕하세요, 홍길동님!
```

## 반환값이 있는 함수

```python
def add(a, b):
    return a + b

result = add(5, 3)
print(result)  # 8
```

## 여러 매개변수

```python
def introduce(name, age, city):
    print(f"이름: {name}, 나이: {age}, 거주지: {city}")

introduce("홍길동", 25, "서울")
```

## 기본값 인자 (Default Arguments)

매개변수에 기본값을 설정할 수 있습니다:

```python
def greet(name, greeting="안녕하세요"):
    print(f"{greeting}, {name}님!")

greet("홍길동")              # 안녕하세요, 홍길동님!
greet("홍길동", "반갑습니다")  # 반갑습니다, 홍길동님!
```

## 키워드 인자 (Keyword Arguments)

매개변수 이름을 지정하여 호출할 수 있습니다:

```python
def introduce(name, age, city):
    print(f"이름: {name}, 나이: {age}, 거주지: {city}")

# 순서와 관계없이 호출 가능
introduce(age=25, city="서울", name="홍길동")
```

## 가변 인자 (*args)

개수가 정해지지 않은 인자를 받을 수 있습니다:

```python
def sum_all(*args):
    total = 0
    for num in args:
        total += num
    return total

print(sum_all(1, 2, 3))        # 6
print(sum_all(1, 2, 3, 4, 5)) # 15
```

## 키워드 가변 인자 (**kwargs)

키워드 인자를 딕셔너리로 받을 수 있습니다:

```python
def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="홍길동", age=25, city="서울")
```

출력:
```
name: 홍길동
age: 25
city: 서울
```

## 실전 예제: 계산기 함수들

```python
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

def multiply(a, b):
    return a * b

def divide(a, b):
    if b != 0:
        return a / b
    else:
        return "0으로 나눌 수 없습니다!"

# 사용
print(add(10, 5))        # 15
print(subtract(10, 5))   # 5
print(multiply(10, 5))   # 50
print(divide(10, 5))     # 2.0
```

## 실전 예제: 최대값/최소값 찾기

```python
def find_max(numbers):
    max_num = numbers[0]
    for num in numbers:
        if num > max_num:
            max_num = num
    return max_num

def find_min(numbers):
    min_num = numbers[0]
    for num in numbers:
        if num < min_num:
            min_num = num
    return min_num

numbers = [5, 2, 8, 1, 9]
print(f"최대값: {find_max(numbers)}")  # 9
print(f"최소값: {find_min(numbers)}")  # 1
```

## 실전 예제: 팩토리얼 함수

```python
def factorial(n):
    if n <= 1:
        return 1
    result = 1
    for i in range(2, n + 1):
        result *= i
    return result

print(factorial(5))  # 120
```

## 재귀 함수

함수가 자기 자신을 호출하는 함수입니다:

```python
def factorial_recursive(n):
    if n <= 1:
        return 1
    return n * factorial_recursive(n - 1)

print(factorial_recursive(5))  # 120
```

## 람다 함수 (Lambda)

간단한 함수를 한 줄로 정의할 수 있습니다:

```python
# 일반 함수
def add(a, b):
    return a + b

# 람다 함수
add_lambda = lambda a, b: a + b

print(add(5, 3))         # 8
print(add_lambda(5, 3))  # 8
```

**람다 함수 활용:**
```python
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x ** 2, numbers))
print(squared)  # [1, 4, 9, 16, 25]
```

## 지역 변수와 전역 변수

### 지역 변수

함수 내부에서 선언된 변수:

```python
def function():
    local_var = 10
    print(local_var)

function()  # 10
# print(local_var)  # 오류! 접근 불가
```

### 전역 변수

함수 밖에서 선언된 변수:

```python
global_var = 100

def function():
    print(global_var)  # 접근 가능

function()  # 100
```

### global 키워드

함수 내에서 전역 변수를 수정하려면 `global` 키워드 사용:

```python
count = 0

def increment():
    global count
    count += 1

increment()
print(count)  # 1
```

## docstring (문서 문자열)

함수에 대한 설명을 추가할 수 있습니다:

```python
def add(a, b):
    """
    두 숫자를 더하는 함수
    
    매개변수:
        a: 첫 번째 숫자
        b: 두 번째 숫자
    
    반환값:
        두 숫자의 합
    """
    return a + b

# 도움말 보기
help(add)
```

## 실전 예제: 학생 성적 관리

```python
def calculate_average(scores):
    """점수 리스트의 평균을 계산"""
    if len(scores) == 0:
        return 0
    return sum(scores) / len(scores)

def get_grade(score):
    """점수에 따른 등급 반환"""
    if score >= 90:
        return "A"
    elif score >= 80:
        return "B"
    elif score >= 70:
        return "C"
    elif score >= 60:
        return "D"
    else:
        return "F"

# 사용
student_scores = [85, 90, 78, 92, 88]
average = calculate_average(student_scores)
grade = get_grade(average)

print(f"평균: {average:.2f}, 등급: {grade}")
```

## 함수 작성 시 주의사항

1. **명확한 이름**: 함수가 하는 일을 이름으로 알 수 있어야 함
2. **단일 책임**: 하나의 함수는 하나의 작업만 수행
3. **적절한 크기**: 함수가 너무 길면 작은 함수로 나누기
4. **문서화**: 복잡한 함수는 docstring으로 설명

## 다음에 공부할 내용

다음 포스트에서는 리스트와 딕셔너리에 대해 공부해보겠습니다.

