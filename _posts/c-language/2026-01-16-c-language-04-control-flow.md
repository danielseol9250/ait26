---
layout: post
title:  "C언어 강의 4편: 제어문 (if, switch)"
date:   2026-01-16 10:00:00 +0900
categories: [IT, C언어 강의]
tags: [C언어, 제어문, if문, switch문, 조건문]
---

# C언어 강의 4편: 제어문 (if, switch)

## 제어문이란?

제어문은 프로그램의 실행 흐름을 제어하는 문장입니다. 조건에 따라 다른 코드를 실행하거나, 특정 코드를 반복 실행할 수 있습니다.

## if 문

조건이 참일 때 코드를 실행합니다:

### 기본 형식

```c
if (조건) {
    // 조건이 참일 때 실행할 코드
}
```

### 예시

```c
int age = 20;

if (age >= 18) {
    printf("성인입니다.\n");
}
```

## if-else 문

조건이 거짓일 때도 코드를 실행합니다:

```c
int score = 85;

if (score >= 60) {
    printf("합격입니다!\n");
} else {
    printf("불합격입니다.\n");
}
```

## if-else if-else 문

여러 조건을 확인합니다:

```c
int score = 85;

if (score >= 90) {
    printf("A등급\n");
} else if (score >= 80) {
    printf("B등급\n");
} else if (score >= 70) {
    printf("C등급\n");
} else {
    printf("F등급\n");
}
```

## 중첩 if 문

if 문 안에 또 다른 if 문을 사용할 수 있습니다:

```c
int age = 25;
bool hasLicense = true;

if (age >= 18) {
    if (hasLicense) {
        printf("운전 가능합니다.\n");
    } else {
        printf("면허가 필요합니다.\n");
    }
} else {
    printf("미성년자입니다.\n");
}
```

## 삼항 연산자

간단한 조건문을 한 줄로 작성할 수 있습니다:

```c
int a = 10, b = 5;
int max = (a > b) ? a : b;  // a가 크면 a, 아니면 b
printf("큰 값: %d\n", max);
```

## switch 문

여러 경우 중 하나를 선택할 때 사용합니다:

### 기본 형식

```c
switch (변수) {
    case 값1:
        // 값1일 때 실행
        break;
    case 값2:
        // 값2일 때 실행
        break;
    default:
        // 위의 경우가 아닐 때 실행
        break;
}
```

### 예시

```c
int day = 3;

switch (day) {
    case 1:
        printf("월요일\n");
        break;
    case 2:
        printf("화요일\n");
        break;
    case 3:
        printf("수요일\n");
        break;
    case 4:
        printf("목요일\n");
        break;
    case 5:
        printf("금요일\n");
        break;
    default:
        printf("주말\n");
        break;
}
```

**주의사항:**
- `break`를 빼먹으면 다음 case도 실행됩니다 (fall-through)
- `default`는 선택사항입니다

## switch 문의 fall-through 활용

의도적으로 break를 생략할 수 있습니다:

```c
int month = 2;

switch (month) {
    case 12:
    case 1:
    case 2:
        printf("겨울\n");
        break;
    case 3:
    case 4:
    case 5:
        printf("봄\n");
        break;
    case 6:
    case 7:
    case 8:
        printf("여름\n");
        break;
    case 9:
    case 10:
    case 11:
        printf("가을\n");
        break;
}
```

## 실전 예제: 성적 계산기

```c
#include <stdio.h>

int main() {
    int score;
    printf("점수를 입력하세요: ");
    scanf("%d", &score);
    
    if (score < 0 || score > 100) {
        printf("잘못된 점수입니다.\n");
    } else if (score >= 90) {
        printf("A등급입니다.\n");
    } else if (score >= 80) {
        printf("B등급입니다.\n");
    } else if (score >= 70) {
        printf("C등급입니다.\n");
    } else if (score >= 60) {
        printf("D등급입니다.\n");
    } else {
        printf("F등급입니다.\n");
    }
    
    return 0;
}
```

## 실전 예제: 계산기

```c
#include <stdio.h>

int main() {
    char operator;
    double num1, num2, result;
    
    printf("연산자를 입력하세요 (+, -, *, /): ");
    scanf(" %c", &operator);
    
    printf("두 숫자를 입력하세요: ");
    scanf("%lf %lf", &num1, &num2);
    
    switch (operator) {
        case '+':
            result = num1 + num2;
            break;
        case '-':
            result = num1 - num2;
            break;
        case '*':
            result = num1 * num2;
            break;
        case '/':
            if (num2 != 0) {
                result = num1 / num2;
            } else {
                printf("0으로 나눌 수 없습니다.\n");
                return 1;
            }
            break;
        default:
            printf("잘못된 연산자입니다.\n");
            return 1;
    }
    
    printf("%.2f %c %.2f = %.2f\n", num1, operator, num2, result);
    return 0;
}
```

## if vs switch 언제 사용할까?

- **if 문**: 범위 검사, 복잡한 조건, 부울 표현식
- **switch 문**: 정확한 값 비교, 여러 경우 중 선택

## 다음에 공부할 내용

다음 포스트에서는 반복문(for, while)에 대해 공부해보겠습니다.

