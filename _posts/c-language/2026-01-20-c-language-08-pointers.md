---
layout: post
title:  "C언어 강의 8편: 포인터 기초"
date:   2026-01-20 10:00:00 +0900
categories: [IT, C언어 강의]
tags: [C언어, 포인터, pointer, 메모리, 주소]
---

# C언어 강의 8편: 포인터 기초

## 포인터란?

포인터(Pointer)는 변수의 메모리 주소를 저장하는 변수입니다. C언어의 가장 강력하면서도 어려운 개념 중 하나입니다.

## 메모리 주소 이해하기

모든 변수는 메모리에 저장되며, 각각 고유한 주소를 가집니다:

```c
int num = 10;
printf("값: %d\n", num);        // 10
printf("주소: %p\n", &num);     // 메모리 주소 (예: 0x7fff5fbff6ac)
```

- `&`: 주소 연산자 (Address Operator)
- 변수 앞에 `&`를 붙이면 그 변수의 주소를 얻을 수 있습니다

## 포인터 선언

```c
데이터타입 *포인터변수명;
```

### 예시

```c
int num = 10;
int *ptr;        // int형 포인터 선언
ptr = &num;      // num의 주소를 ptr에 저장

printf("num의 값: %d\n", num);        // 10
printf("num의 주소: %p\n", &num);     // 주소
printf("ptr이 가리키는 값: %d\n", *ptr);  // 10
printf("ptr에 저장된 주소: %p\n", ptr);  // &num과 동일
```

## 역참조 연산자 (*)

포인터가 가리키는 메모리의 값을 가져오거나 변경할 수 있습니다:

```c
int num = 10;
int *ptr = &num;

*ptr = 20;  // ptr이 가리키는 곳(num)의 값을 20으로 변경
printf("%d\n", num);  // 20
```

## 포인터의 기본 사용법

```c
#include <stdio.h>

int main() {
    int num = 10;
    int *ptr = &num;
    
    printf("=== 포인터 기본 ===\n");
    printf("num의 값: %d\n", num);
    printf("num의 주소: %p\n", &num);
    printf("ptr의 값(주소): %p\n", ptr);
    printf("ptr이 가리키는 값: %d\n", *ptr);
    
    // 포인터를 통한 값 변경
    *ptr = 20;
    printf("\n값 변경 후:\n");
    printf("num의 값: %d\n", num);
    printf("ptr이 가리키는 값: %d\n", *ptr);
    
    return 0;
}
```

## 포인터와 함수

포인터를 사용하면 함수에서 실제 변수의 값을 변경할 수 있습니다:

### 값에 의한 전달 (Call by Value)

```c
void swap(int a, int b) {
    int temp = a;
    a = b;
    b = temp;
    // 함수 내에서만 값이 바뀜
}

int main() {
    int x = 10, y = 20;
    swap(x, y);
    printf("%d %d\n", x, y);  // 10 20 (변하지 않음)
    return 0;
}
```

### 참조에 의한 전달 (Call by Reference)

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

## 포인터와 배열

배열 이름은 첫 번째 요소의 주소를 가리킵니다:

```c
int arr[5] = {1, 2, 3, 4, 5};
int *ptr = arr;  // arr은 &arr[0]과 동일

printf("%d\n", *ptr);        // 1 (arr[0])
printf("%d\n", *(ptr + 1));  // 2 (arr[1])
printf("%d\n", *(ptr + 2));  // 3 (arr[2])

// 배열 접근 방법들
printf("%d\n", arr[0]);      // 1
printf("%d\n", *(arr + 0));  // 1 (동일)
printf("%d\n", ptr[0]);      // 1 (동일)
```

## 포인터 산술 연산

포인터에 정수를 더하거나 빼면 다음/이전 요소를 가리킵니다:

```c
int arr[5] = {10, 20, 30, 40, 50};
int *ptr = arr;

printf("%d\n", *ptr);        // 10
printf("%d\n", *(ptr + 1));  // 20
printf("%d\n", *(ptr + 2));  // 30

ptr++;  // 다음 요소로 이동
printf("%d\n", *ptr);        // 20
```

## NULL 포인터

포인터가 아무것도 가리키지 않을 때 사용:

```c
int *ptr = NULL;

if (ptr == NULL) {
    printf("포인터가 NULL입니다.\n");
}

// NULL 포인터 역참조는 위험!
// *ptr = 10;  // 오류 발생!
```

## 포인터의 포인터 (이중 포인터)

포인터의 주소를 저장하는 포인터:

```c
int num = 10;
int *ptr = &num;
int **pptr = &ptr;

printf("num: %d\n", num);           // 10
printf("*ptr: %d\n", *ptr);         // 10
printf("**pptr: %d\n", **pptr);     // 10
```

## 동적 메모리 할당

포인터를 사용하여 런타임에 메모리를 할당할 수 있습니다:

```c
#include <stdlib.h>

int main() {
    int *ptr;
    int n = 5;
    
    // 동적 메모리 할당
    ptr = (int*)malloc(n * sizeof(int));
    
    if (ptr == NULL) {
        printf("메모리 할당 실패\n");
        return 1;
    }
    
    // 사용
    for (int i = 0; i < n; i++) {
        ptr[i] = i * 10;
    }
    
    // 출력
    for (int i = 0; i < n; i++) {
        printf("%d ", ptr[i]);
    }
    
    // 메모리 해제 (중요!)
    free(ptr);
    ptr = NULL;
    
    return 0;
}
```

## 실전 예제: 배열의 최대값 찾기

```c
#include <stdio.h>

int findMax(int *arr, int size) {
    int max = *arr;  // arr[0]
    for (int i = 1; i < size; i++) {
        if (*(arr + i) > max) {
            max = *(arr + i);
        }
    }
    return max;
}

int main() {
    int numbers[5] = {5, 2, 8, 1, 9};
    int max = findMax(numbers, 5);
    printf("최대값: %d\n", max);
    return 0;
}
```

## 실전 예제: 문자열 길이 구하기

```c
#include <stdio.h>

int stringLength(char *str) {
    int length = 0;
    while (*str != '\0') {
        length++;
        str++;
    }
    return length;
}

int main() {
    char str[] = "Hello";
    printf("길이: %d\n", stringLength(str));
    return 0;
}
```

## 포인터 사용 시 주의사항

1. **초기화**: 포인터를 사용하기 전에 반드시 초기화
2. **NULL 체크**: 역참조 전에 NULL인지 확인
3. **범위 확인**: 배열 범위를 벗어나지 않도록 주의
4. **메모리 해제**: `malloc`으로 할당한 메모리는 반드시 `free`로 해제

## 다음에 공부할 내용

다음 포스트에서는 문자열 처리에 대해 공부해보겠습니다.

