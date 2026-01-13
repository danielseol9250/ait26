---
layout: post
title:  "파이썬 강의 8편: 문자열 처리"
date:   2026-01-30 10:00:00 +0900
categories: [IT, 파이썬 강의]
tags: [파이썬, 문자열, string, 텍스트처리]
description: "파이썬의 문자열 메서드, 포맷팅, 슬라이싱, 정규표현식 기초 등 문자열 처리의 모든 기능을 배웁니다."
---

# 파이썬 강의 8편: 문자열 처리

## 문자열이란?

문자열(String)은 문자들의 시퀀스입니다. 파이썬에서 문자열은 매우 강력하고 유연하게 처리할 수 있습니다.

## 문자열 생성

```python
# 작은따옴표
str1 = 'Hello'

# 큰따옴표
str2 = "World"

# 삼중따옴표 (여러 줄)
str3 = """여러 줄
문자열입니다"""

# 이스케이프 문자
str4 = "안녕하세요\n줄바꿈입니다"
```

## 문자열 접근

인덱스로 문자에 접근할 수 있습니다:

```python
text = "Python"

print(text[0])   # P (첫 번째 문자)
print(text[-1])  # n (마지막 문자)
print(text[1:4]) # yth (슬라이싱)
```

## 문자열 연산

```python
# 연결
greeting = "안녕" + "하세요"
print(greeting)  # 안녕하세요

# 반복
print("Hello" * 3)  # HelloHelloHello

# 길이
text = "Python"
print(len(text))  # 6
```

## 문자열 메서드

### 대소문자 변환

```python
text = "Hello World"

print(text.upper())    # HELLO WORLD
print(text.lower())    # hello world
print(text.capitalize())  # Hello world
print(text.title())    # Hello World
```

### 공백 제거

```python
text = "  Python  "

print(text.strip())    # Python (양쪽 공백 제거)
print(text.lstrip())   # Python  (왼쪽 공백 제거)
print(text.rstrip())   #   Python (오른쪽 공백 제거)
```

### 문자열 찾기

```python
text = "Hello World"

print(text.find("World"))    # 6 (인덱스 반환)
print(text.find("Python"))   # -1 (없으면 -1)
print(text.index("World"))   # 6
print("World" in text)       # True
print(text.count("l"))       # 3 (개수)
```

### 문자열 분할과 결합

```python
# split(): 문자열 분할
text = "사과,바나나,오렌지"
fruits = text.split(",")
print(fruits)  # ['사과', '바나나', '오렌지']

# join(): 문자열 결합
fruits = ["사과", "바나나", "오렌지"]
text = ", ".join(fruits)
print(text)  # 사과, 바나나, 오렌지
```

### 문자열 교체

```python
text = "Hello World"

new_text = text.replace("World", "Python")
print(new_text)  # Hello Python
```

### 문자열 검사

```python
text = "Hello123"

print(text.isdigit())    # False (모두 숫자인지)
print(text.isalpha())    # False (모두 문자인지)
print(text.isalnum())    # True (문자 또는 숫자인지)
print(text.startswith("Hello"))  # True
print(text.endswith("123"))      # True
```

## 문자열 포맷팅

### f-string (권장, Python 3.6+)

```python
name = "홍길동"
age = 25

text = f"이름: {name}, 나이: {age}세"
print(text)  # 이름: 홍길동, 나이: 25세

# 표현식 사용
text = f"내년 나이: {age + 1}세"
print(text)  # 내년 나이: 26세

# 포맷 지정
pi = 3.14159
print(f"원주율: {pi:.2f}")  # 원주율: 3.14
```

### format() 메서드

```python
name = "홍길동"
age = 25

text = "이름: {}, 나이: {}세".format(name, age)
print(text)  # 이름: 홍길동, 나이: 25세

# 인덱스 사용
text = "이름: {0}, 나이: {1}세, 이름: {0}".format(name, age)

# 키워드 사용
text = "이름: {name}, 나이: {age}세".format(name=name, age=age)
```

### % 포맷팅 (구식)

```python
name = "홍길동"
age = 25

text = "이름: %s, 나이: %d세" % (name, age)
print(text)  # 이름: 홍길동, 나이: 25세
```

## 실전 예제: 문자열 뒤집기

```python
text = "Python"

# 방법 1: 슬라이싱
reversed_text = text[::-1]
print(reversed_text)  # nohtyP

# 방법 2: reversed()와 join()
reversed_text = "".join(reversed(text))
print(reversed_text)  # nohtyP
```

## 실전 예제: 회문(Palindrome) 확인

```python
def is_palindrome(text):
    text = text.lower().replace(" ", "")
    return text == text[::-1]

print(is_palindrome("level"))      # True
print(is_palindrome("hello"))     # False
print(is_palindrome("A man a plan a canal Panama"))  # True
```

## 실전 예제: 단어 개수 세기

```python
text = "Hello World Python Programming"

words = text.split()
print(f"단어 개수: {len(words)}")  # 4

# 각 단어의 길이
for word in words:
    print(f"{word}: {len(word)}글자")
```

## 실전 예제: 이메일 검증

```python
def is_valid_email(email):
    if "@" not in email:
        return False
    
    parts = email.split("@")
    if len(parts) != 2:
        return False
    
    username, domain = parts
    if len(username) == 0 or len(domain) == 0:
        return False
    
    if "." not in domain:
        return False
    
    return True

print(is_valid_email("test@example.com"))  # True
print(is_valid_email("invalid.email"))     # False
```

## 실전 예제: 텍스트 통계

```python
def text_statistics(text):
    stats = {
        "문자 수": len(text),
        "단어 수": len(text.split()),
        "줄 수": text.count("\n") + 1,
        "대문자": sum(1 for c in text if c.isupper()),
        "소문자": sum(1 for c in text if c.islower()),
        "숫자": sum(1 for c in text if c.isdigit())
    }
    return stats

text = """Hello World
Python Programming
123 Numbers"""

stats = text_statistics(text)
for key, value in stats.items():
    print(f"{key}: {value}")
```

## 정규표현식 기초 (re 모듈)

```python
import re

text = "연락처: 010-XXXX-XXXX"

# 전화번호 패턴 찾기 (예시)
pattern = r"\d{3}-[X\d]{4}-[X\d]{4}"
match = re.search(pattern, text)
if match:
    print(f"전화번호 패턴: {match.group()}")  # 010-XXXX-XXXX

# 이메일 찾기
text = "이메일: test@example.com"
pattern = r"\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b"
match = re.search(pattern, text)
if match:
    print(f"이메일: {match.group()}")  # test@example.com
```

## 문자열 인코딩/디코딩

```python
# 문자열을 바이트로
text = "안녕하세요"
encoded = text.encode("utf-8")
print(encoded)  # b'\xec\x95\x88\xeb\x85\x95...'

# 바이트를 문자열로
decoded = encoded.decode("utf-8")
print(decoded)  # 안녕하세요
```

## 다음에 공부할 내용

다음 포스트에서는 파일 입출력에 대해 공부해보겠습니다.

