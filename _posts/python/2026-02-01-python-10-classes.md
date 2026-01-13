---
layout: post
title:  "파이썬 강의 10편: 클래스와 객체"
date:   2026-02-01 10:00:00 +0900
categories: [IT, 파이썬 강의]
tags: [파이썬, 클래스, 객체, OOP, 객체지향]
description: "파이썬의 클래스와 객체, 생성자, 메서드, 상속 등 객체지향 프로그래밍의 기초를 배웁니다."
---

# 파이썬 강의 10편: 클래스와 객체

## 클래스와 객체란?

클래스(Class)는 객체를 만들기 위한 설계도이고, 객체(Object)는 클래스로부터 생성된 인스턴스입니다. 객체지향 프로그래밍의 핵심 개념입니다.

## 클래스 정의

### 기본 형식

```python
class 클래스명:
    def __init__(self):
        # 초기화 메서드
        pass
    
    def 메서드명(self):
        # 메서드 정의
        pass
```

### 간단한 예시

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def introduce(self):
        print(f"안녕하세요, 저는 {self.name}이고 {self.age}세입니다.")

# 객체 생성
person1 = Person("홍길동", 25)
person1.introduce()  # 안녕하세요, 저는 홍길동이고 25세입니다.
```

## 생성자 (__init__)

객체가 생성될 때 자동으로 호출되는 메서드입니다:

```python
class Student:
    def __init__(self, name, age, grade):
        self.name = name
        self.age = age
        self.grade = grade
        print(f"{name} 학생이 생성되었습니다.")

student = Student("홍길동", 20, "A")
```

## 인스턴스 변수와 메서드

```python
class Dog:
    def __init__(self, name, breed):
        self.name = name      # 인스턴스 변수
        self.breed = breed    # 인스턴스 변수
    
    def bark(self):           # 인스턴스 메서드
        print(f"{self.name}가 멍멍 짖습니다!")
    
    def get_info(self):       # 인스턴스 메서드
        return f"{self.name}는 {self.breed}입니다."

dog1 = Dog("뽀삐", "골든리트리버")
dog1.bark()              # 뽀삐가 멍멍 짖습니다!
print(dog1.get_info())   # 뽀삐는 골든리트리버입니다.
```

## 클래스 변수

모든 인스턴스가 공유하는 변수입니다:

```python
class Student:
    school = "서울대학교"  # 클래스 변수
    
    def __init__(self, name):
        self.name = name      # 인스턴스 변수

student1 = Student("홍길동")
student2 = Student("김철수")

print(student1.school)  # 서울대학교
print(student2.school)  # 서울대학교

# 클래스 변수 변경
Student.school = "연세대학교"
print(student1.school)  # 연세대학교
```

## 실전 예제: 은행 계좌 클래스

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self.balance = balance
    
    def deposit(self, amount):
        """입금"""
        if amount > 0:
            self.balance += amount
            print(f"{amount}원 입금되었습니다. 잔액: {self.balance}원")
        else:
            print("입금 금액은 0보다 커야 합니다.")
    
    def withdraw(self, amount):
        """출금"""
        if amount > 0:
            if amount <= self.balance:
                self.balance -= amount
                print(f"{amount}원 출금되었습니다. 잔액: {self.balance}원")
            else:
                print("잔액이 부족합니다.")
        else:
            print("출금 금액은 0보다 커야 합니다.")
    
    def get_balance(self):
        """잔액 조회"""
        return self.balance

# 사용
account = BankAccount("홍길동", 10000)
account.deposit(5000)
account.withdraw(3000)
print(f"현재 잔액: {account.get_balance()}원")
```

## 실전 예제: 학생 관리 클래스

```python
class Student:
    def __init__(self, name, student_id):
        self.name = name
        self.student_id = student_id
        self.scores = []
    
    def add_score(self, score):
        """점수 추가"""
        if 0 <= score <= 100:
            self.scores.append(score)
        else:
            print("점수는 0-100 사이여야 합니다.")
    
    def get_average(self):
        """평균 점수 계산"""
        if len(self.scores) == 0:
            return 0
        return sum(self.scores) / len(self.scores)
    
    def get_grade(self):
        """등급 반환"""
        average = self.get_average()
        if average >= 90:
            return "A"
        elif average >= 80:
            return "B"
        elif average >= 70:
            return "C"
        elif average >= 60:
            return "D"
        else:
            return "F"
    
    def __str__(self):
        """문자열 표현"""
        return f"{self.name} (학번: {self.student_id}, 평균: {self.get_average():.2f}, 등급: {self.get_grade()})"

# 사용
student = Student("홍길동", "2024001")
student.add_score(85)
student.add_score(90)
student.add_score(78)

print(student)
```

## 상속 (Inheritance)

기존 클래스를 확장하여 새로운 클래스를 만들 수 있습니다:

```python
class Animal:
    def __init__(self, name):
        self.name = name
    
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        return f"{self.name}가 멍멍 짖습니다!"

class Cat(Animal):
    def speak(self):
        return f"{self.name}가 야옹 웁니다!"

dog = Dog("뽀삐")
cat = Cat("나비")

print(dog.speak())  # 뽀삐가 멍멍 짖습니다!
print(cat.speak())  # 나비가 야옹 웁니다!
```

## 특수 메서드 (Magic Methods)

### __str__: 문자열 표현

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def __str__(self):
        return f"Person(name={self.name}, age={self.age})"

person = Person("홍길동", 25)
print(person)  # Person(name=홍길동, age=25)
```

### __len__: 길이 반환

```python
class ShoppingCart:
    def __init__(self):
        self.items = []
    
    def add_item(self, item):
        self.items.append(item)
    
    def __len__(self):
        return len(self.items)

cart = ShoppingCart()
cart.add_item("사과")
cart.add_item("바나나")
print(len(cart))  # 2
```

## 캡슐화 (Encapsulation)

파이썬은 접근 제어자가 없지만, 관례적으로 사용합니다:

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self._balance = balance      # 보호된 변수 (관례)
        self.__account_number = 12345  # 비공개 변수 (이름 변경됨)
    
    def get_balance(self):
        return self._balance
    
    def set_balance(self, amount):
        if amount >= 0:
            self._balance = amount

account = BankAccount("홍길동", 1000)
print(account.get_balance())  # 1000
```

## 실전 예제: 도서 관리 시스템

```python
class Book:
    def __init__(self, title, author, isbn):
        self.title = title
        self.author = author
        self.isbn = isbn
        self.is_borrowed = False
    
    def borrow(self):
        if not self.is_borrowed:
            self.is_borrowed = True
            return True
        return False
    
    def return_book(self):
        if self.is_borrowed:
            self.is_borrowed = False
            return True
        return False
    
    def __str__(self):
        status = "대출 중" if self.is_borrowed else "대출 가능"
        return f"{self.title} - {self.author} ({status})"

class Library:
    def __init__(self):
        self.books = []
    
    def add_book(self, book):
        self.books.append(book)
    
    def find_book(self, title):
        for book in self.books:
            if book.title == title:
                return book
        return None
    
    def list_available_books(self):
        return [book for book in self.books if not book.is_borrowed]

# 사용
library = Library()
library.add_book(Book("파이썬 프로그래밍", "홍길동", "123-456"))
library.add_book(Book("자료구조", "김철수", "789-012"))

book = library.find_book("파이썬 프로그래밍")
if book:
    book.borrow()
    print(book)

available = library.list_available_books()
print(f"대출 가능한 책: {len(available)}권")
```

## 다음 단계

이제 파이썬의 기본 개념들을 모두 배웠습니다! 다음에는:
- 모듈과 패키지
- 예외 처리
- 데코레이터
- 제너레이터
- 프로젝트 실습

등을 통해 실력을 더 향상시킬 수 있습니다.

## 마무리

파이썬 공부를 이렇게 정리해봤어요! 이제 기본기를 바탕으로 다양한 프로젝트를 만들어보려고 해요. 파이썬의 풍부한 라이브러리를 활용하면 웹 개발, 데이터 분석, 인공지능 등 다양한 분야에서 활용할 수 있을 것 같아요.

