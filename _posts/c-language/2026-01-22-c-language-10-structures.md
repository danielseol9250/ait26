---
layout: post
title:  "C언어 강의 10편: 구조체"
date:   2026-01-22 10:00:00 +0900
categories: [IT, C언어 강의]
tags: [C언어, 구조체, struct, 사용자정의타입]
---

# C언어 강의 10편: 구조체

## 구조체란?

구조체(Structure)는 서로 다른 타입의 데이터를 하나의 그룹으로 묶어서 관리할 수 있게 해주는 사용자 정의 데이터 타입입니다.

## 구조체 선언

### 기본 형식

```c
struct 구조체명 {
    데이터타입 멤버1;
    데이터타입 멤버2;
    // ...
};
```

### 예시: 학생 정보 구조체

```c
struct Student {
    char name[50];
    int age;
    float score;
};
```

## 구조체 변수 선언

```c
// 방법 1: 구조체 선언 후 변수 선언
struct Student {
    char name[50];
    int age;
    float score;
};

struct Student student1;

// 방법 2: 선언과 동시에 변수 선언
struct Student {
    char name[50];
    int age;
    float score;
} student1, student2;

// 방법 3: typedef 사용 (권장)
typedef struct {
    char name[50];
    int age;
    float score;
} Student;

Student student1;  // struct 키워드 없이 사용 가능
```

## 구조체 멤버 접근

점(`.`) 연산자를 사용하여 멤버에 접근합니다:

```c
struct Student {
    char name[50];
    int age;
    float score;
};

struct Student student1;

// 값 할당
strcpy(student1.name, "홍길동");
student1.age = 20;
student1.score = 85.5;

// 값 출력
printf("이름: %s\n", student1.name);
printf("나이: %d\n", student1.age);
printf("점수: %.1f\n", student1.score);
```

## 구조체 초기화

### 방법 1: 선언과 동시에 초기화

```c
struct Student student1 = {
    "홍길동",
    20,
    85.5
};
```

### 방법 2: 지정 초기화 (C99)

```c
struct Student student1 = {
    .name = "홍길동",
    .age = 20,
    .score = 85.5
};
```

## 구조체 배열

여러 개의 구조체를 배열로 관리할 수 있습니다:

```c
struct Student students[5];

// 입력
for (int i = 0; i < 5; i++) {
    printf("학생 %d 정보 입력:\n", i + 1);
    printf("이름: ");
    scanf("%s", students[i].name);
    printf("나이: ");
    scanf("%d", &students[i].age);
    printf("점수: ");
    scanf("%f", &students[i].score);
}

// 출력
for (int i = 0; i < 5; i++) {
    printf("학생 %d: %s, %d세, %.1f점\n", 
           i + 1, students[i].name, students[i].age, students[i].score);
}
```

## 구조체 포인터

구조체를 가리키는 포인터를 사용할 수 있습니다:

```c
struct Student student1 = {"홍길동", 20, 85.5};
struct Student *ptr = &student1;

// 포인터를 통한 멤버 접근
printf("%s\n", (*ptr).name);  // 방법 1
printf("%s\n", ptr->name);    // 방법 2 (화살표 연산자, 권장)
printf("%d\n", ptr->age);
printf("%.1f\n", ptr->score);
```

**화살표 연산자(`->`)**: 포인터를 통해 구조체 멤버에 접근할 때 사용

## 구조체를 함수에 전달

### 값에 의한 전달

```c
void printStudent(struct Student s) {
    printf("이름: %s\n", s.name);
    printf("나이: %d\n", s.age);
    printf("점수: %.1f\n", s.score);
}

int main() {
    struct Student student1 = {"홍길동", 20, 85.5};
    printStudent(student1);
    return 0;
}
```

### 참조에 의한 전달 (포인터)

```c
void printStudent(struct Student *s) {
    printf("이름: %s\n", s->name);
    printf("나이: %d\n", s->age);
    printf("점수: %.1f\n", s->score);
}

void modifyStudent(struct Student *s) {
    s->age = 21;  // 값 변경 가능
    s->score = 90.0;
}

int main() {
    struct Student student1 = {"홍길동", 20, 85.5};
    printStudent(&student1);
    modifyStudent(&student1);
    printStudent(&student1);
    return 0;
}
```

## 중첩 구조체

구조체 안에 다른 구조체를 포함할 수 있습니다:

```c
struct Date {
    int year;
    int month;
    int day;
};

struct Student {
    char name[50];
    struct Date birthDate;
    float score;
};

int main() {
    struct Student student1 = {
        "홍길동",
        {2000, 1, 15},
        85.5
    };
    
    printf("생년월일: %d년 %d월 %d일\n", 
           student1.birthDate.year,
           student1.birthDate.month,
           student1.birthDate.day);
    
    return 0;
}
```

## 구조체 크기와 메모리 정렬

구조체의 크기는 멤버들의 크기 합과 다를 수 있습니다 (메모리 정렬 때문):

```c
struct Example {
    char a;    // 1바이트
    int b;     // 4바이트
    char c;    // 1바이트
};

printf("크기: %zu\n", sizeof(struct Example));  // 12바이트 (정렬됨)
```

## 실전 예제: 학생 관리 시스템

```c
#include <stdio.h>
#include <string.h>

typedef struct {
    char name[50];
    int age;
    float score;
} Student;

void inputStudent(Student *s) {
    printf("이름: ");
    scanf("%s", s->name);
    printf("나이: ");
    scanf("%d", &s->age);
    printf("점수: ");
    scanf("%f", &s->score);
}

void printStudent(Student *s) {
    printf("이름: %s, 나이: %d, 점수: %.1f\n", 
           s->name, s->age, s->score);
}

float calculateAverage(Student students[], int count) {
    float sum = 0;
    for (int i = 0; i < count; i++) {
        sum += students[i].score;
    }
    return sum / count;
}

int main() {
    Student students[5];
    
    // 학생 정보 입력
    printf("5명의 학생 정보를 입력하세요:\n");
    for (int i = 0; i < 5; i++) {
        printf("\n학생 %d:\n", i + 1);
        inputStudent(&students[i]);
    }
    
    // 학생 정보 출력
    printf("\n=== 학생 정보 ===\n");
    for (int i = 0; i < 5; i++) {
        printStudent(&students[i]);
    }
    
    // 평균 계산
    float avg = calculateAverage(students, 5);
    printf("\n평균 점수: %.2f\n", avg);
    
    return 0;
}
```

## 실전 예제: 도서 관리 시스템

```c
#include <stdio.h>
#include <string.h>

typedef struct {
    char title[100];
    char author[50];
    int year;
    float price;
} Book;

void addBook(Book *book, char *title, char *author, int year, float price) {
    strcpy(book->title, title);
    strcpy(book->author, author);
    book->year = year;
    book->price = price;
}

void printBook(Book *book) {
    printf("제목: %s\n", book->title);
    printf("저자: %s\n", book->author);
    printf("출판년도: %d\n", book->year);
    printf("가격: %.2f원\n", book->price);
    printf("---\n");
}

int main() {
    Book library[3];
    
    addBook(&library[0], "C언어 프로그래밍", "홍길동", 2020, 25000);
    addBook(&library[1], "자료구조", "김철수", 2021, 30000);
    addBook(&library[2], "알고리즘", "이영희", 2022, 35000);
    
    printf("=== 도서 목록 ===\n");
    for (int i = 0; i < 3; i++) {
        printBook(&library[i]);
    }
    
    return 0;
}
```

## 구조체와 함수 포인터

구조체에 함수 포인터를 포함할 수 있습니다:

```c
typedef struct {
    int x, y;
    void (*print)(int, int);
} Point;

void printPoint(int x, int y) {
    printf("(%d, %d)\n", x, y);
}

int main() {
    Point p = {10, 20, printPoint};
    p.print(p.x, p.y);  // (10, 20)
    return 0;
}
```

## 구조체의 장점

1. **데이터 그룹화**: 관련된 데이터를 하나로 묶어 관리
2. **코드 가독성**: 의미 있는 단위로 데이터 관리
3. **재사용성**: 구조체를 타입처럼 사용 가능
4. **확장성**: 필요에 따라 멤버 추가 가능

## 다음 단계

이제 C언어의 기본 개념들을 모두 배웠습니다! 다음에는:
- 파일 입출력
- 동적 메모리 할당 심화
- 고급 포인터 기법
- 프로젝트 실습

등을 통해 실력을 더 향상시킬 수 있습니다.

## 마무리

C언어 공부를 이렇게 정리해봤어요! 이제 기본기를 바탕으로 다양한 프로젝트를 만들어보려고 해요. 꾸준한 연습이 실력 향상의 열쇠라고 생각해요.

