---
layout: post
title:  "파이썬 강의 7편: 리스트와 딕셔너리"
date:   2026-01-29 10:00:00 +0900
categories: [IT, 파이썬 강의]
tags: [파이썬, 리스트, 딕셔너리, list, dict, 자료구조]
description: "파이썬의 리스트와 딕셔너리를 배우고, 요소 추가/삭제/수정, 슬라이싱, 내장 함수 등 다양한 활용법을 학습합니다."
---

# 파이썬 강의 7편: 리스트와 딕셔너리

## 리스트란?

리스트(List)는 여러 개의 데이터를 순서대로 저장하는 자료구조입니다. 다른 언어의 배열과 유사하지만 더 유연합니다.

## 리스트 생성

```python
# 빈 리스트
empty_list = []

# 숫자 리스트
numbers = [1, 2, 3, 4, 5]

# 문자열 리스트
fruits = ["사과", "바나나", "오렌지"]

# 다양한 타입 혼합
mixed = [1, "문자열", 3.14, True]
```

## 리스트 접근

인덱스로 요소에 접근합니다 (0부터 시작):

```python
fruits = ["사과", "바나나", "오렌지"]

print(fruits[0])  # 사과 (첫 번째 요소)
print(fruits[1])  # 바나나
print(fruits[-1]) # 오렌지 (마지막 요소, -1 사용)
```

## 리스트 수정

```python
fruits = ["사과", "바나나", "오렌지"]
fruits[0] = "포도"
print(fruits)  # ['포도', '바나나', '오렌지']
```

## 리스트 슬라이싱

리스트의 일부를 가져올 수 있습니다:

```python
numbers = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

print(numbers[2:5])    # [2, 3, 4] (인덱스 2부터 4까지)
print(numbers[:3])     # [0, 1, 2] (처음부터 2까지)
print(numbers[3:])     # [3, 4, 5, 6, 7, 8, 9] (3부터 끝까지)
print(numbers[::2])    # [0, 2, 4, 6, 8] (2칸씩)
print(numbers[::-1])   # [9, 8, 7, 6, 5, 4, 3, 2, 1, 0] (역순)
```

## 리스트 메서드

### 요소 추가

```python
fruits = ["사과", "바나나"]

# append(): 끝에 추가
fruits.append("오렌지")
print(fruits)  # ['사과', '바나나', '오렌지']

# insert(): 특정 위치에 추가
fruits.insert(1, "포도")
print(fruits)  # ['사과', '포도', '바나나', '오렌지']

# extend(): 여러 요소 추가
fruits.extend(["수박", "참외"])
print(fruits)  # ['사과', '포도', '바나나', '오렌지', '수박', '참외']
```

### 요소 삭제

```python
fruits = ["사과", "바나나", "오렌지"]

# remove(): 값으로 삭제
fruits.remove("바나나")
print(fruits)  # ['사과', '오렌지']

# pop(): 인덱스로 삭제 (기본값: 마지막 요소)
fruits.pop()
print(fruits)  # ['사과']

# del: 인덱스로 삭제
del fruits[0]
print(fruits)  # []
```

### 기타 메서드

```python
numbers = [3, 1, 4, 1, 5, 9, 2, 6]

# sort(): 정렬 (원본 수정)
numbers.sort()
print(numbers)  # [1, 1, 2, 3, 4, 5, 6, 9]

# reverse(): 역순
numbers.reverse()
print(numbers)  # [9, 6, 5, 4, 3, 2, 1, 1]

# count(): 개수 세기
print(numbers.count(1))  # 2

# index(): 인덱스 찾기
print(numbers.index(5))  # 2

# len(): 길이
print(len(numbers))  # 8
```

## 리스트 컴프리헨션

리스트를 간결하게 생성할 수 있습니다:

```python
# 일반 방법
squares = []
for i in range(10):
    squares.append(i ** 2)

# 리스트 컴프리헨션
squares = [i ** 2 for i in range(10)]

# 조건 추가
even_squares = [i ** 2 for i in range(10) if i % 2 == 0]
```

## 딕셔너리란?

딕셔너리(Dictionary)는 키(Key)와 값(Value)의 쌍으로 데이터를 저장하는 자료구조입니다.

## 딕셔너리 생성

```python
# 빈 딕셔너리
empty_dict = {}

# 키-값 쌍으로 생성
student = {
    "name": "홍길동",
    "age": 25,
    "grade": "A"
}

# dict() 함수 사용
student = dict(name="홍길동", age=25, grade="A")
```

## 딕셔너리 접근

```python
student = {
    "name": "홍길동",
    "age": 25,
    "grade": "A"
}

print(student["name"])        # 홍길동
print(student.get("age"))     # 25
print(student.get("city", "없음"))  # 없음 (기본값)
```

## 딕셔너리 수정

```python
student = {"name": "홍길동", "age": 25}

# 값 수정
student["age"] = 26

# 새 키-값 추가
student["city"] = "서울"

print(student)  # {'name': '홍길동', 'age': 26, 'city': '서울'}
```

## 딕셔너리 메서드

```python
student = {"name": "홍길동", "age": 25, "grade": "A"}

# keys(): 모든 키
print(list(student.keys()))  # ['name', 'age', 'grade']

# values(): 모든 값
print(list(student.values()))  # ['홍길동', 25, 'A']

# items(): 모든 키-값 쌍
print(list(student.items()))  # [('name', '홍길동'), ('age', 25), ('grade', 'A')]

# pop(): 키로 삭제
age = student.pop("age")
print(age)  # 25
print(student)  # {'name': '홍길동', 'grade': 'A'}

# update(): 여러 키-값 추가/수정
student.update({"city": "서울", "phone": "010-XXXX-XXXX"})
```

## 딕셔너리 순회

```python
student = {"name": "홍길동", "age": 25, "grade": "A"}

# 키로 순회
for key in student:
    print(f"{key}: {student[key]}")

# items()로 순회
for key, value in student.items():
    print(f"{key}: {value}")
```

## 실전 예제: 학생 관리 시스템

```python
students = [
    {"name": "홍길동", "age": 20, "score": 85},
    {"name": "김철수", "age": 21, "score": 90},
    {"name": "이영희", "age": 20, "score": 78}
]

# 평균 점수 계산
total = 0
for student in students:
    total += student["score"]

average = total / len(students)
print(f"평균 점수: {average:.2f}")

# 최고점 학생 찾기
best_student = max(students, key=lambda x: x["score"])
print(f"최고점 학생: {best_student['name']} ({best_student['score']}점)")
```

## 실전 예제: 단어 빈도 세기

```python
text = "apple banana apple orange banana apple"
words = text.split()

word_count = {}
for word in words:
    if word in word_count:
        word_count[word] += 1
    else:
        word_count[word] = 1

print(word_count)  # {'apple': 3, 'banana': 2, 'orange': 1}
```

## 중첩 자료구조

리스트와 딕셔너리를 중첩해서 사용할 수 있습니다:

```python
# 리스트 안에 딕셔너리
students = [
    {"name": "홍길동", "scores": [85, 90, 88]},
    {"name": "김철수", "scores": [92, 87, 91]}
]

# 딕셔너리 안에 리스트
classroom = {
    "students": ["홍길동", "김철수", "이영희"],
    "teacher": "선생님",
    "room": 101
}
```

## 다음에 공부할 내용

다음 포스트에서는 문자열 처리에 대해 공부해보겠습니다.

