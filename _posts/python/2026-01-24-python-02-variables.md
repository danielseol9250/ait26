---
layout: post
title:  "파이썬 강의 2편: 변수와 데이터 타입"
date:   2026-01-24 10:00:00 +0900
categories: [IT, 파이썬 강의]
tags: [파이썬, 변수, 데이터타입, 기초]
description: "파이썬의 변수 선언 방법과 기본 데이터 타입(정수, 실수, 문자열, 불린)을 배우고 실전 예제를 통해 이해합니다."
---

안녕하세요! 오늘은 프로그래밍의 핵심인 **변수**에 대해 배워볼게요. 파이썬의 변수는 정말 사용하기 쉬워요. C언어를 배우신 분들이라면 "이렇게 쉬워도 되나?" 싶을 정도예요! 😊

# 파이썬 강의 2편: 변수와 데이터 타입

## 변수란 무엇일까요?

변수(Variable)를 쉽게 설명하면, **데이터를 저장하는 상자**예요. 마치 우리가 물건을 보관할 때 상자에 넣고 이름표를 붙이는 것처럼, 컴퓨터의 메모리 공간에 이름을 붙여서 데이터를 저장하는 거예요.

**실생활 비유:**
- 학교 사물함을 생각해보세요. 각 사물함에는 번호가 있고, 그 안에 물건을 넣을 수 있어요.
- 변수도 마찬가지예요. 변수 이름(사물함 번호)이 있고, 그 안에 값(물건)을 저장할 수 있어요.

```python
age = 25  # "age"라는 이름의 상자에 25라는 숫자를 넣었어요
name = "홍길동"  # "name"이라는 상자에 "홍길동"이라는 글자를 넣었어요
```

## 파이썬 변수의 특별한 점

파이썬의 변수는 다른 언어와 비교하면 정말 특별해요!

**C언어와 비교:**
```c
// C언어: 타입을 미리 지정해야 해요
int age = 25;
float height = 175.5f;
char grade = 'A';
```

```python
# 파이썬: 타입을 지정하지 않아도 돼요!
age = 25
height = 175.5
grade = 'A'
```

파이썬은 **타입을 자동으로 추론**해요! 정말 편하죠? 😊

## 변수 선언하기

파이썬에서 변수를 선언하는 건 정말 간단해요:

```python
name = "홍길동"
age = 25
height = 175.5
is_student = True
```

이게 전부예요! 타입을 신경 쓸 필요가 없어요.

## 기본 데이터 타입 알아보기

파이썬에는 여러 종류의 데이터 타입이 있어요. 하나씩 알아볼게요!

### 1. 정수 (Integer) - `int`

정수는 소수점이 없는 숫자예요. 나이, 개수, 점수 같은 것들을 저장할 때 사용해요.

```python
age = 25
count = -10
big_number = 1000000
temperature = -5

print(type(age))  # <class 'int'>
```

**특징:**
- 양수, 음수, 0 모두 가능해요
- 크기 제한이 거의 없어요 (메모리가 허용하는 한)
- 다른 언어와 달리 `long` 타입이 따로 없어요 (모두 `int`예요)

### 2. 실수 (Float) - `float`

실수는 소수점이 있는 숫자예요. 키, 몸무게, 가격 같은 것들을 저장할 때 사용해요.

```python
height = 175.5
pi = 3.14159
temperature = -5.5
price = 1234.56

print(type(height))  # <class 'float'>
```

**특징:**
- 소수점이 있으면 자동으로 `float` 타입이에요
- 과학적 표기법도 사용 가능해요: `1.5e3` (1500.0)

### 3. 문자열 (String) - `str`

문자열은 글자들을 저장할 때 사용해요. 이름, 주소, 메시지 같은 것들을 저장할 때 사용해요.

```python
name = "홍길동"
message = '안녕하세요'
multiline = """여러 줄
문자열입니다"""

print(type(name))  # <class 'str'>
```

**문자열 사용법:**
- 작은따옴표(`'`) 또는 큰따옴표(`"`) 사용 가능해요
- 둘 다 똑같아요! 편한 거 사용하시면 돼요
- 삼중따옴표(`"""` 또는 `'''`)로 여러 줄 문자열 가능해요

**예시:**
```python
# 모두 같은 문자열이에요
text1 = "Hello"
text2 = 'Hello'
text3 = """Hello"""

# 여러 줄 문자열
poem = """동해물과 백두산이
마르고 닳도록
하느님이 보우하사
우리나라 만세"""
```

### 4. 불린 (Boolean) - `bool`

불린은 참(True) 또는 거짓(False)만 저장할 수 있어요. 조건을 체크할 때 사용해요.

```python
is_student = True
is_working = False
is_raining = True

print(type(is_student))  # <class 'bool'>
```

**주의사항:**
- `True`와 `False`는 **대문자로 시작**해야 해요!
- `true`, `false`는 안 돼요 (에러 발생)

### 5. 리스트 (List) - `list`

리스트는 여러 개의 값을 순서대로 저장할 수 있어요. 마치 서랍장처럼요!

```python
numbers = [1, 2, 3, 4, 5]
names = ["홍길동", "김철수", "이영희"]
mixed = [1, "문자열", 3.14, True]  # 다양한 타입 섞어서 사용 가능!

print(type(numbers))  # <class 'list'>
```

**특징:**
- 대괄호 `[]`로 감싸요
- 여러 타입을 섞어서 저장할 수 있어요 (파이썬의 장점!)
- 순서가 있어요 (나중에 배울 거예요)

### 6. 딕셔너리 (Dictionary) - `dict`

딕셔너리는 키(key)와 값(value)을 쌍으로 저장해요. 마치 사전처럼요!

```python
student = {
    "name": "홍길동",
    "age": 25,
    "grade": "A"
}

print(type(student))  # <class 'dict'>
```

**특징:**
- 중괄호 `{}`로 감싸요
- 키로 값을 찾을 수 있어요
- 나중에 자세히 배울 거예요!

## 변수명 짓기 - 좋은 이름의 힘

변수 이름을 잘 지으면 코드를 읽기가 훨씬 쉬워져요!

### 변수명 규칙

1. **영문자, 숫자, 언더스코어(_)만 사용 가능**
2. **숫자로 시작할 수 없음**
3. **예약어 사용 불가** (if, for, while, def 등)

**좋은 예:**
```python
student_name = "홍길동"    # ✅ 의미가 명확해요
student_age = 25           # ✅ 언더스코어 사용
max_score = 100            # ✅ 여러 단어는 언더스코어로 구분
totalCount = 50            # ✅ 대소문자 구분 가능 (하지만 언더스코어 추천)
```

**나쁜 예:**
```python
2name = "홍길동"          # ❌ 숫자로 시작
student-name = "홍길동"    # ❌ 하이픈 사용 불가
if = 10                   # ❌ 예약어 사용 불가
a = 10                    # ❌ 의미가 불명확해요
```

**변수명 짓기 팁:**
- 의미 있는 이름 사용: `x`보다는 `age`가 좋아요
- 여러 단어는 언더스코어 사용: `student_age` (스네이크 케이스) - 파이썬 스타일!
- 너무 길지 않게: `studentAgeInYears`보다는 `student_age`가 좋아요
- 짧지만 명확하게: `num`보다는 `count`가 좋아요

## 타입 확인하기

`type()` 함수로 변수의 타입을 확인할 수 있어요:

```python
age = 25
name = "홍길동"
height = 175.5
is_student = True

print(type(age))        # <class 'int'>
print(type(name))       # <class 'str'>
print(type(height))     # <class 'float'>
print(type(is_student)) # <class 'bool'>
```

**왜 타입을 확인하나요?**
- 변수가 어떤 타입인지 확인하고 싶을 때
- 디버깅할 때 (에러를 찾을 때)
- 타입 변환이 필요할 때

## 타입 변환 (Type Casting)

다른 타입으로 변환할 수 있어요. 정말 유용해요!

```python
# 정수를 문자열로
age = 25
age_str = str(age)
print(age_str, type(age_str))  # 25 <class 'str'>

# 문자열을 정수로
number_str = "100"
number = int(number_str)
print(number, type(number))  # 100 <class 'int'>

# 정수를 실수로
num = 10
num_float = float(num)
print(num_float, type(num_float))  # 10.0 <class 'float'>

# 실수를 정수로 (소수점 버림)
pi = 3.14159
pi_int = int(pi)
print(pi_int)  # 3 (소수점이 버려져요)
```

**자주 사용하는 타입 변환:**
- `str()`: 문자열로 변환
- `int()`: 정수로 변환
- `float()`: 실수로 변환
- `bool()`: 불린으로 변환

## 변수 값 변경하기

변수는 언제든지 값을 변경할 수 있어요:

```python
age = 25
print(age)  # 25

age = 26
print(age)  # 26

# 타입도 변경 가능해요! (파이썬의 특별한 점)
age = "스물여섯"
print(age, type(age))  # 스물여섯 <class 'str'>
```

**C언어와의 차이:**
- C언어: 변수 타입을 바꿀 수 없어요
- 파이썬: 변수 타입을 자유롭게 바꿀 수 있어요!

## 여러 변수에 값 할당하기

파이썬은 여러 변수에 값을 한 번에 할당할 수 있어요:

```python
# 같은 값 할당
a = b = c = 10
print(a, b, c)  # 10 10 10

# 다른 값 할당 (언패킹)
name, age, height = "홍길동", 25, 175.5
print(name, age, height)  # 홍길동 25 175.5

# 변수 값 교환하기 (정말 쉬워요!)
x = 10
y = 20
x, y = y, x  # 한 줄로 교환!
print(x, y)  # 20 10
```

**C언어와 비교:**
```c
// C언어: 임시 변수가 필요해요
int temp = x;
x = y;
y = temp;
```

```python
# 파이썬: 한 줄로 끝!
x, y = y, x
```

정말 편하죠? 😊

## 실전 예제 1: 학생 정보 관리

이제 배운 내용으로 학생 정보를 관리하는 프로그램을 만들어볼게요:

```python
# 학생 정보 변수 선언
student_name = "홍길동"
student_age = 20
student_score = 85.5
is_passed = True

# 정보 출력
print("=== 학생 정보 ===")
print("이름:", student_name)
print("나이:", student_age, "세")
print("점수:", student_score, "점")
print("합격 여부:", "합격" if is_passed else "불합격")
print("=================")

# 정보 업데이트
student_age = 21
student_score = 90.0

print("\n=== 업데이트된 정보 ===")
print("나이:", student_age, "세")
print("점수:", student_score, "점")
```

**출력 결과:**
```
=== 학생 정보 ===
이름: 홍길동
나이: 20 세
점수: 85.5 점
합격 여부: 합격
=================

=== 업데이트된 정보 ===
나이: 21 세
점수: 90.0 점
```

## 실전 예제 2: 간단한 계산기

변수를 사용해서 간단한 계산을 해볼게요:

```python
# 원의 넓이 계산
radius = 5
pi = 3.14159
area = pi * radius * radius

print("반지름:", radius)
print("원의 넓이:", area)
print("원의 넓이 (소수점 2자리):", round(area, 2))

# 사각형의 넓이 계산
width = 10
height = 5
rectangle_area = width * height

print("\n사각형의 넓이:", rectangle_area)
```

**출력 결과:**
```
반지름: 5
원의 넓이: 78.53975
원의 넓이 (소수점 2자리): 78.54

사각형의 넓이: 50
```

## 실전 예제 3: 타입 변환 활용

타입 변환을 활용한 예제예요:

```python
# 사용자 입력 받기 (나중에 배울 거예요, 지금은 이해만 하세요)
# input()은 항상 문자열을 반환해요
age_str = "25"  # 문자열
print("나이 (문자열):", age_str, type(age_str))

# 정수로 변환
age = int(age_str)
print("나이 (정수):", age, type(age))

# 내년 나이 계산
next_year_age = age + 1
print("내년 나이:", next_year_age)

# 문자열과 숫자 결합하기
message = "내년에는 " + str(next_year_age) + "세가 됩니다!"
print(message)
```

**출력 결과:**
```
나이 (문자열): 25 <class 'str'>
나이 (정수): 25 <class 'int'>
내년 나이: 26
내년에는 26세가 됩니다!
```

## 변수와 메모리 이해하기

파이썬에서는 변수가 객체를 참조해요. 조금 어려운 개념이지만 이해하면 좋아요:

```python
a = 10
b = a  # b는 a와 같은 객체를 참조
print(a, b)  # 10 10

a = 20  # a는 새로운 객체를 참조
print(a, b)  # 20 10 (b는 여전히 10을 참조)
```

**비유:**
- `a = 10`: "a라는 이름표를 10이라는 상자에 붙였어요"
- `b = a`: "b라는 이름표도 같은 상자에 붙였어요"
- `a = 20`: "a 이름표를 20이라는 새로운 상자로 옮겼어요"
- `b`는 여전히 10 상자를 가리키고 있어요

## 자주 하는 실수와 해결 방법

### 실수 1: 문자열과 숫자 연산
```python
age = "25"  # 문자열
# next_age = age + 1  # ❌ 에러! 문자열과 숫자는 더할 수 없어요
next_age = int(age) + 1  # ✅ 타입 변환 후 연산
```

### 실수 2: 변수명 오타
```python
student_age = 20
# print(student_ag)  # ❌ 오타! NameError 발생
print(student_age)  # ✅ 올바름
```

### 실수 3: 예약어 사용
```python
# def = 10  # ❌ 예약어 사용 불가
def_count = 10  # ✅ 언더스코어 추가
```

### 실수 4: 타입 확인 안 하기
```python
user_input = input("나이를 입력하세요: ")  # 항상 문자열!
# age = user_input + 1  # ❌ 에러!
age = int(user_input) + 1  # ✅ 타입 변환 필요
```

## 연습 문제

다음 프로그램을 작성해보세요:

1. **나이 계산기**: 올해 나이와 내년 나이를 출력하는 프로그램
2. **BMI 계산기**: 키와 몸무게를 변수에 저장하고 BMI를 계산하는 프로그램
   - BMI = 몸무게(kg) / (키(m))²
3. **타입 변환 연습**: 문자열 "100"을 정수로 변환하고 50을 더한 후 다시 문자열로 변환하기

**힌트:**
```python
# 1번 문제
age = 25
print("올해 나이:", age)
print("내년 나이:", age + 1)

# 2번 문제
weight = 70.0
height = 1.75
bmi = weight / (height ** 2)
print("BMI:", round(bmi, 2))

# 3번 문제
num_str = "100"
num = int(num_str)
result = num + 50
result_str = str(result)
print(result_str)
```

## 다음 강의 예고

오늘은 변수와 데이터 타입에 대해 배웠어요. 변수는 프로그래밍의 기본이에요!

다음 강의에서는 **연산자**에 대해 배울 거예요. 덧셈, 뺄셈 같은 산술 연산자부터 비교 연산자, 논리 연산자까지 다양한 연산자를 배워볼게요.

파이썬의 연산자는 정말 직관적이고 사용하기 쉬워요. 기대되시나요? 😊

궁금한 점이 있으시면 언제든지 댓글로 질문해주세요! 다음 강의에서 만나요!
