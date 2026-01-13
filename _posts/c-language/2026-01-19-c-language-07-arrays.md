---
layout: post
title:  "C언어 강의 7편: 배열"
date:   2026-01-19 10:00:00 +0900
categories: [IT, C언어 강의]
tags: [C언어, 배열, array, 자료구조]
---

# C언어 강의 7편: 배열

## 배열이란?

배열(Array)은 같은 타입의 데이터를 연속된 메모리 공간에 저장하는 자료구조입니다. 여러 개의 변수를 하나의 이름으로 관리할 수 있습니다.

## 배열 선언

### 기본 형식

```c
데이터타입 배열명[크기];
```

### 예시

```c
int numbers[5];           // 5개의 정수를 저장하는 배열
float scores[10];         // 10개의 실수를 저장하는 배열
char name[20];           // 20개의 문자를 저장하는 배열
```

## 배열 초기화

### 방법 1: 선언과 동시에 초기화

```c
int numbers[5] = {1, 2, 3, 4, 5};
```

### 방법 2: 크기 생략 (자동 크기)

```c
int numbers[] = {1, 2, 3, 4, 5};  // 크기가 자동으로 5로 설정됨
```

### 방법 3: 부분 초기화

```c
int numbers[5] = {1, 2};  // 나머지는 0으로 초기화됨
// 결과: {1, 2, 0, 0, 0}
```

### 방법 4: 모두 0으로 초기화

```c
int numbers[5] = {0};  // 모든 요소가 0으로 초기화
```

## 배열 접근

배열의 각 요소는 인덱스(0부터 시작)로 접근합니다:

```c
int numbers[5] = {10, 20, 30, 40, 50};

printf("%d\n", numbers[0]);  // 10 (첫 번째 요소)
printf("%d\n", numbers[2]);  // 30 (세 번째 요소)
printf("%d\n", numbers[4]);  // 50 (마지막 요소)

// 값 변경
numbers[0] = 100;
printf("%d\n", numbers[0]);  // 100
```

**주의사항:**
- 인덱스는 0부터 시작합니다
- 배열 범위를 벗어나면 오류가 발생할 수 있습니다

## 배열 순회

### for 문 사용

```c
int numbers[5] = {1, 2, 3, 4, 5};

// 배열 출력
for (int i = 0; i < 5; i++) {
    printf("%d ", numbers[i]);
}
printf("\n");

// 배열에 값 입력
for (int i = 0; i < 5; i++) {
    printf("숫자 입력: ");
    scanf("%d", &numbers[i]);
}
```

## 배열의 크기

```c
int numbers[5];
int size = sizeof(numbers) / sizeof(numbers[0]);  // 배열 크기 계산
printf("배열 크기: %d\n", size);  // 5
```

## 다차원 배열

### 2차원 배열

```c
int matrix[3][4];  // 3행 4열 배열

// 초기화
int matrix[3][4] = {
    {1, 2, 3, 4},
    {5, 6, 7, 8},
    {9, 10, 11, 12}
};

// 접근
printf("%d\n", matrix[0][0]);  // 1
printf("%d\n", matrix[1][2]);  // 7
```

### 2차원 배열 순회

```c
int matrix[3][4] = {
    {1, 2, 3, 4},
    {5, 6, 7, 8},
    {9, 10, 11, 12}
};

for (int i = 0; i < 3; i++) {
    for (int j = 0; j < 4; j++) {
        printf("%d ", matrix[i][j]);
    }
    printf("\n");
}
```

## 배열을 함수에 전달

배열을 함수에 전달할 때는 포인터로 전달됩니다:

```c
// 배열의 합 구하기
int sum(int arr[], int size) {
    int total = 0;
    for (int i = 0; i < size; i++) {
        total += arr[i];
    }
    return total;
}

int main() {
    int numbers[5] = {1, 2, 3, 4, 5};
    int result = sum(numbers, 5);
    printf("합: %d\n", result);
    return 0;
}
```

## 배열의 최대값/최소값 찾기

```c
int findMax(int arr[], int size) {
    int max = arr[0];
    for (int i = 1; i < size; i++) {
        if (arr[i] > max) {
            max = arr[i];
        }
    }
    return max;
}

int findMin(int arr[], int size) {
    int min = arr[0];
    for (int i = 1; i < size; i++) {
        if (arr[i] < min) {
            min = arr[i];
        }
    }
    return min;
}
```

## 배열 정렬 (버블 정렬)

```c
void bubbleSort(int arr[], int size) {
    for (int i = 0; i < size - 1; i++) {
        for (int j = 0; j < size - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                // 값 교환
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
}

int main() {
    int numbers[5] = {5, 2, 8, 1, 9};
    bubbleSort(numbers, 5);
    
    for (int i = 0; i < 5; i++) {
        printf("%d ", numbers[i]);
    }
    // 출력: 1 2 5 8 9
    return 0;
}
```

## 배열 검색 (선형 검색)

```c
int linearSearch(int arr[], int size, int target) {
    for (int i = 0; i < size; i++) {
        if (arr[i] == target) {
            return i;  // 찾은 위치 반환
        }
    }
    return -1;  // 찾지 못함
}
```

## 실전 예제: 학생 성적 관리

```c
#include <stdio.h>

int main() {
    int scores[5];
    int sum = 0;
    float average;
    
    // 성적 입력
    printf("5명의 성적을 입력하세요:\n");
    for (int i = 0; i < 5; i++) {
        printf("학생 %d: ", i + 1);
        scanf("%d", &scores[i]);
        sum += scores[i];
    }
    
    // 평균 계산
    average = (float)sum / 5;
    
    // 결과 출력
    printf("\n=== 성적 결과 ===\n");
    for (int i = 0; i < 5; i++) {
        printf("학생 %d: %d점\n", i + 1, scores[i]);
    }
    printf("평균: %.2f점\n", average);
    
    return 0;
}
```

## 실전 예제: 행렬 곱셈

{% raw %}
```c
#include <stdio.h>

void multiplyMatrices(int a[][3], int b[][3], int result[][3], int rows, int cols) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            result[i][j] = 0;
            for (int k = 0; k < cols; k++) {
                result[i][j] += a[i][k] * b[k][j];
            }
        }
    }
}

int main() {
    int matrix1[3][3] = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
    int matrix2[3][3] = {{9, 8, 7}, {6, 5, 4}, {3, 2, 1}};
    int result[3][3];
    
    multiplyMatrices(matrix1, matrix2, result, 3, 3);
    
    // 결과 출력
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            printf("%d ", result[i][j]);
        }
        printf("\n");
    }
    
    return 0;
}
```
{% endraw %}

## 배열의 한계

1. **고정 크기**: 배열 크기를 런타임에 변경할 수 없음
2. **메모리 낭비**: 필요한 것보다 크게 선언하면 메모리 낭비
3. **삽입/삭제 어려움**: 중간에 요소를 삽입하거나 삭제하기 어려움

## 다음에 공부할 내용

다음 포스트에서는 포인터 기초에 대해 공부해보겠습니다.

