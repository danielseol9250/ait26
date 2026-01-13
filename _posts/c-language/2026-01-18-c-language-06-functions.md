---
layout: post
title:  "C언어 강의 6편: 함수"
date:   2026-01-18 10:00:00 +0900
categories: [IT, C언어 강의]
tags: [C언어, 함수, function, 모듈화, 재사용]
---

# C언어 강의 6편: 함수

## 함수란?

함수(Function)는 특정 작업을 수행하는 코드 블록입니다. 코드를 재사용하고 모듈화할 수 있게 해줍니다.

## 함수의 장점

1. **코드 재사용**: 같은 코드를 여러 번 작성하지 않아도 됨
2. **모듈화**: 프로그램을 작은 단위로 나눌 수 있음
3. **가독성**: 코드가 더 읽기 쉬워짐
4. **유지보수**: 수정이 필요할 때 한 곳만 수정하면 됨

## 함수 선언 및 정의

### 기본 형식

```c
반환타입 함수명(매개변수) {
    // 함수 본문
    return 값;
}
```

### 예시: 간단한 함수

```c
#include <stdio.h>

// 함수 선언 (프로토타입)
int add(int a, int b);

int main() {
    int result = add(5, 3);
    printf("결과: %d\n", result);
    return 0;
}

// 함수 정의
int add(int a, int b) {
    return a + b;
}
```

## 반환값이 없는 함수

```c
void printHello() {
    printf("Hello, World!\n");
}

int main() {
    printHello();
    return 0;
}
```

## 매개변수가 없는 함수

```c
int getNumber() {
    return 42;
}

int main() {
    int num = getNumber();
    printf("숫자: %d\n", num);
    return 0;
}
```

## 다양한 함수 예제

### 최대값 구하기

```c
int max(int a, int b) {
    if (a > b) {
        return a;
    } else {
        return b;
    }
}

// 또는 삼항 연산자 사용
int max(int a, int b) {
    return (a > b) ? a : b;
}
```

### 팩토리얼 계산

```c
long long factorial(int n) {
    if (n <= 1) {
        return 1;
    }
    return n * factorial(n - 1);  // 재귀 함수
}
```

### 소수 판별

```c
#include <stdbool.h>
#include <math.h>

bool isPrime(int n) {
    if (n < 2) {
        return false;
    }
    
    for (int i = 2; i * i <= n; i++) {
        if (n % i == 0) {
            return false;
        }
    }
    
    return true;
}
```

## 함수 프로토타입

함수를 사용하기 전에 선언해야 합니다:

```c
// 프로토타입 선언
int add(int a, int b);
void printMessage();

int main() {
    int result = add(5, 3);
    printMessage();
    return 0;
}

// 함수 정의
int add(int a, int b) {
    return a + b;
}

void printMessage() {
    printf("함수 호출 완료!\n");
}
```

## 지역 변수와 전역 변수

### 지역 변수

함수 내부에서 선언된 변수로, 함수가 끝나면 사라집니다:

```c
void function() {
    int localVar = 10;  // 지역 변수
    printf("%d\n", localVar);
}

int main() {
    // printf("%d\n", localVar);  // 오류! 접근 불가
    function();
    return 0;
}
```

### 전역 변수

함수 밖에서 선언된 변수로, 모든 함수에서 접근 가능합니다:

```c
int globalVar = 100;  // 전역 변수

void function() {
    printf("%d\n", globalVar);  // 접근 가능
}

int main() {
    printf("%d\n", globalVar);  // 접근 가능
    return 0;
}
```

## 값에 의한 전달 (Call by Value)

C언어는 기본적으로 값에 의한 전달을 사용합니다:

```c
void swap(int a, int b) {
    int temp = a;
    a = b;
    b = temp;
    // 이 함수 내에서만 값이 바뀜
}

int main() {
    int x = 10, y = 20;
    swap(x, y);
    printf("%d %d\n", x, y);  // 10 20 (변하지 않음)
    return 0;
}
```

## 참조에 의한 전달 (Call by Reference)

포인터를 사용하여 실제 변수의 값을 변경할 수 있습니다:

```c
void swap(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}

int main() {
    int x = 10, y = 20;
    swap(&x, &y);
    printf("%d %d\n", x, y);  // 20 10 (값이 바뀜)
    return 0;
}
```

## 재귀 함수

함수가 자기 자신을 호출하는 함수입니다:

```c
// 피보나치 수열
int fibonacci(int n) {
    if (n <= 1) {
        return n;
    }
    return fibonacci(n - 1) + fibonacci(n - 2);
}

// 하노이 탑
void hanoi(int n, char from, char to, char aux) {
    if (n == 1) {
        printf("원반 1을 %c에서 %c로 이동\n", from, to);
        return;
    }
    hanoi(n - 1, from, aux, to);
    printf("원반 %d를 %c에서 %c로 이동\n", n, from, to);
    hanoi(n - 1, aux, to, from);
}
```

## 실전 예제: 계산기 프로그램

```c
#include <stdio.h>

double add(double a, double b) {
    return a + b;
}

double subtract(double a, double b) {
    return a - b;
}

double multiply(double a, double b) {
    return a * b;
}

double divide(double a, double b) {
    if (b != 0) {
        return a / b;
    } else {
        printf("0으로 나눌 수 없습니다.\n");
        return 0;
    }
}

int main() {
    char operator;
    double num1, num2, result;
    
    printf("연산자를 입력하세요 (+, -, *, /): ");
    scanf(" %c", &operator);
    
    printf("두 숫자를 입력하세요: ");
    scanf("%lf %lf", &num1, &num2);
    
    switch (operator) {
        case '+':
            result = add(num1, num2);
            break;
        case '-':
            result = subtract(num1, num2);
            break;
        case '*':
            result = multiply(num1, num2);
            break;
        case '/':
            result = divide(num1, num2);
            break;
        default:
            printf("잘못된 연산자입니다.\n");
            return 1;
    }
    
    printf("%.2f %c %.2f = %.2f\n", num1, operator, num2, result);
    return 0;
}
```

## 함수 작성 시 주의사항

1. **명확한 이름**: 함수가 하는 일을 이름으로 알 수 있어야 함
2. **단일 책임**: 하나의 함수는 하나의 작업만 수행
3. **적절한 크기**: 함수가 너무 길면 작은 함수로 나누기
4. **문서화**: 복잡한 함수는 주석으로 설명

## 다음에 공부할 내용

다음 포스트에서는 배열에 대해 공부해보겠습니다.

