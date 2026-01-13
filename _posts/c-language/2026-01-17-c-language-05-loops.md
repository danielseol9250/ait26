---
layout: post
title:  "C언어 강의 5편: 반복문 (for, while)"
date:   2026-01-17 10:00:00 +0900
categories: [IT, C언어 강의]
tags: [C언어, 반복문, for문, while문, 반복]
---

# C언어 강의 5편: 반복문 (for, while)

## 반복문이란?

반복문은 특정 코드를 여러 번 실행할 수 있게 해주는 제어문입니다. 같은 작업을 반복해야 할 때 유용합니다.

## for 문

정해진 횟수만큼 반복할 때 사용합니다:

### 기본 형식

```c
for (초기화; 조건; 증감) {
    // 반복 실행할 코드
}
```

### 예시

```c
// 1부터 10까지 출력
for (int i = 1; i <= 10; i++) {
    printf("%d ", i);
}
// 출력: 1 2 3 4 5 6 7 8 9 10
```

### 동작 과정

1. **초기화**: `int i = 1` 실행 (한 번만)
2. **조건 확인**: `i <= 10` 확인
3. **코드 실행**: 조건이 참이면 중괄호 안 코드 실행
4. **증감**: `i++` 실행
5. **2번으로 돌아가서 반복**

### 다양한 사용법

```c
// 역순 출력
for (int i = 10; i >= 1; i--) {
    printf("%d ", i);
}

// 2씩 증가
for (int i = 0; i <= 10; i += 2) {
    printf("%d ", i);
}

// 중첩 for 문 (구구단)
for (int i = 1; i <= 9; i++) {
    for (int j = 1; j <= 9; j++) {
        printf("%d x %d = %d\n", i, j, i * j);
    }
}
```

## while 문

조건이 참인 동안 반복합니다:

### 기본 형식

```c
while (조건) {
    // 반복 실행할 코드
}
```

### 예시

```c
int i = 1;
while (i <= 10) {
    printf("%d ", i);
    i++;
}
```

### for vs while

```c
// for 문
for (int i = 1; i <= 10; i++) {
    printf("%d ", i);
}

// while 문으로 동일하게
int i = 1;
while (i <= 10) {
    printf("%d ", i);
    i++;
}
```

## do-while 문

최소 한 번은 실행하고, 그 후 조건을 확인합니다:

```c
int i = 1;
do {
    printf("%d ", i);
    i++;
} while (i <= 10);
```

**차이점:**
- `while`: 조건을 먼저 확인
- `do-while`: 코드를 먼저 실행한 후 조건 확인

## 반복문 제어

### break 문

반복문을 즉시 종료합니다:

```c
for (int i = 1; i <= 10; i++) {
    if (i == 5) {
        break;  // i가 5가 되면 반복문 종료
    }
    printf("%d ", i);
}
// 출력: 1 2 3 4
```

### continue 문

현재 반복을 건너뛰고 다음 반복으로 넘어갑니다:

```c
for (int i = 1; i <= 10; i++) {
    if (i % 2 == 0) {
        continue;  // 짝수면 건너뛰기
    }
    printf("%d ", i);
}
// 출력: 1 3 5 7 9 (홀수만 출력)
```

## 무한 루프

조건이 항상 참인 반복문입니다:

```c
// for 문으로 무한 루프
for (;;) {
    // 코드
    if (조건) break;  // 종료 조건
}

// while 문으로 무한 루프
while (1) {
    // 코드
    if (조건) break;  // 종료 조건
}
```

## 실전 예제: 팩토리얼 계산

```c
#include <stdio.h>

int main() {
    int n;
    long long factorial = 1;
    
    printf("숫자를 입력하세요: ");
    scanf("%d", &n);
    
    if (n < 0) {
        printf("음수는 팩토리얼을 계산할 수 없습니다.\n");
        return 1;
    }
    
    for (int i = 1; i <= n; i++) {
        factorial *= i;
    }
    
    printf("%d! = %lld\n", n, factorial);
    return 0;
}
```

## 실전 예제: 소수 찾기

```c
#include <stdio.h>
#include <stdbool.h>

int main() {
    int n;
    printf("숫자를 입력하세요: ");
    scanf("%d", &n);
    
    bool isPrime = true;
    
    if (n < 2) {
        isPrime = false;
    } else {
        for (int i = 2; i * i <= n; i++) {
            if (n % i == 0) {
                isPrime = false;
                break;
            }
        }
    }
    
    if (isPrime) {
        printf("%d는 소수입니다.\n", n);
    } else {
        printf("%d는 소수가 아닙니다.\n", n);
    }
    
    return 0;
}
```

## 실전 예제: 별 찍기

```c
#include <stdio.h>

int main() {
    int n;
    printf("줄 수를 입력하세요: ");
    scanf("%d", &n);
    
    // 직각삼각형
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= i; j++) {
            printf("*");
        }
        printf("\n");
    }
    
    printf("\n");
    
    // 역직각삼각형
    for (int i = n; i >= 1; i--) {
        for (int j = 1; j <= i; j++) {
            printf("*");
        }
        printf("\n");
    }
    
    return 0;
}
```

## 실전 예제: 숫자 맞추기 게임

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
    srand(time(NULL));
    int answer = rand() % 100 + 1;  // 1~100 사이의 랜덤 숫자
    int guess;
    int attempts = 0;
    
    printf("1부터 100 사이의 숫자를 맞춰보세요!\n");
    
    while (1) {
        printf("숫자를 입력하세요: ");
        scanf("%d", &guess);
        attempts++;
        
        if (guess == answer) {
            printf("정답입니다! %d번 만에 맞췄습니다.\n", attempts);
            break;
        } else if (guess < answer) {
            printf("더 큰 숫자입니다.\n");
        } else {
            printf("더 작은 숫자입니다.\n");
        }
    }
    
    return 0;
}
```

## 반복문 선택 가이드

- **for**: 반복 횟수가 정해져 있을 때
- **while**: 조건에 따라 반복할 때
- **do-while**: 최소 한 번은 실행해야 할 때

## 다음에 공부할 내용

다음 포스트에서는 함수에 대해 공부해보겠습니다.

