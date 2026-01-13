---
layout: post
title:  "C언어 강의 9편: 문자열 처리"
date:   2026-01-21 10:00:00 +0900
categories: [IT, C언어 강의]
tags: [C언어, 문자열, string, 문자배열]
---

# C언어 강의 9편: 문자열 처리

## 문자열이란?

C언어에서 문자열은 문자 배열로 표현됩니다. 문자열의 끝은 널 문자(`\0`)로 표시됩니다.

## 문자열 선언

### 방법 1: 문자 배열

```c
char str[20] = "Hello";
// 또는
char str[] = "Hello";  // 크기 자동 설정 (6: 5문자 + 널 문자)
```

### 방법 2: 포인터

```c
char *str = "Hello";
```

**차이점:**
- 배열: 수정 가능
- 포인터: 수정 불가능 (문자열 리터럴)

## 문자열 초기화

```c
char str1[10] = "Hello";
char str2[] = "World";
char str3[10] = {'H', 'e', 'l', 'l', 'o', '\0'};
```

## 문자열 입력/출력

### printf와 scanf 사용

```c
char name[50];

printf("이름을 입력하세요: ");
scanf("%s", name);  // 공백 전까지만 입력됨
printf("안녕하세요, %s님!\n", name);
```

### gets와 puts 사용 (비추천)

```c
char str[100];
gets(str);    // 한 줄 전체 입력 (위험!)
puts(str);    // 출력 후 줄바꿈
```

### fgets 사용 (권장)

```c
char str[100];
printf("문자열 입력: ");
fgets(str, sizeof(str), stdin);  // 안전한 입력
printf("입력한 문자열: %s", str);
```

## 문자열 길이 구하기

### 직접 구현

```c
int stringLength(char *str) {
    int length = 0;
    while (str[length] != '\0') {
        length++;
    }
    return length;
}
```

### strlen 함수 사용

```c
#include <string.h>

char str[] = "Hello";
int len = strlen(str);  // 5
```

## 문자열 복사

### 직접 구현

```c
void stringCopy(char *dest, char *src) {
    int i = 0;
    while (src[i] != '\0') {
        dest[i] = src[i];
        i++;
    }
    dest[i] = '\0';
}
```

### strcpy 함수 사용

```c
#include <string.h>

char src[] = "Hello";
char dest[20];
strcpy(dest, src);
printf("%s\n", dest);  // Hello
```

### strncpy 사용 (안전)

```c
char src[] = "Hello";
char dest[20];
strncpy(dest, src, sizeof(dest) - 1);
dest[sizeof(dest) - 1] = '\0';  // 안전을 위해
```

## 문자열 연결 (Concatenation)

### 직접 구현

```c
void stringConcat(char *dest, char *src) {
    int destLen = strlen(dest);
    int i = 0;
    while (src[i] != '\0') {
        dest[destLen + i] = src[i];
        i++;
    }
    dest[destLen + i] = '\0';
}
```

### strcat 함수 사용

```c
#include <string.h>

char str1[20] = "Hello";
char str2[] = " World";
strcat(str1, str2);
printf("%s\n", str1);  // Hello World
```

### strncat 사용 (안전)

```c
char str1[20] = "Hello";
char str2[] = " World";
strncat(str1, str2, sizeof(str1) - strlen(str1) - 1);
```

## 문자열 비교

### 직접 구현

```c
int stringCompare(char *str1, char *str2) {
    int i = 0;
    while (str1[i] != '\0' && str2[i] != '\0') {
        if (str1[i] != str2[i]) {
            return str1[i] - str2[i];
        }
        i++;
    }
    return str1[i] - str2[i];
}
```

### strcmp 함수 사용

```c
#include <string.h>

char str1[] = "Hello";
char str2[] = "Hello";
char str3[] = "World";

if (strcmp(str1, str2) == 0) {
    printf("같습니다.\n");
}

if (strcmp(str1, str3) < 0) {
    printf("str1이 str3보다 작습니다.\n");
}
```

**반환값:**
- 0: 두 문자열이 같음
- 양수: 첫 번째 문자열이 큼
- 음수: 첫 번째 문자열이 작음

## 문자열 검색

### strchr: 문자 검색

```c
#include <string.h>

char str[] = "Hello World";
char *result = strchr(str, 'o');
if (result != NULL) {
    printf("찾은 위치: %s\n", result);  // o World
    printf("인덱스: %ld\n", result - str);  // 4
}
```

### strstr: 문자열 검색

```c
#include <string.h>

char str[] = "Hello World";
char *result = strstr(str, "World");
if (result != NULL) {
    printf("찾은 위치: %s\n", result);  // World
}
```

## 문자열 토큰화 (Tokenization)

### strtok 함수 사용

```c
#include <string.h>

char str[] = "Hello,World,C,Programming";
char *part = strtok(str, ",");

while (part != NULL) {
    printf("%s\n", part);
    part = strtok(NULL, ",");
}
```

출력:
```
Hello
World
C
Programming
```

## 문자열 변환

### 숫자를 문자열로

```c
#include <stdio.h>

int num = 123;
char str[20];
sprintf(str, "%d", num);
printf("%s\n", str);  // 123
```

### 문자열을 숫자로

```c
#include <stdlib.h>

char str[] = "123";
int num = atoi(str);        // 문자열을 정수로
long lnum = atol(str);      // 문자열을 long으로
double dnum = atof("3.14"); // 문자열을 실수로

printf("%d\n", num);   // 123
printf("%.2f\n", dnum); // 3.14
```

## 실전 예제: 문자열 뒤집기

```c
#include <stdio.h>
#include <string.h>

void reverseString(char *str) {
    int len = strlen(str);
    int start = 0;
    int end = len - 1;
    
    while (start < end) {
        char temp = str[start];
        str[start] = str[end];
        str[end] = temp;
        start++;
        end--;
    }
}

int main() {
    char str[] = "Hello";
    reverseString(str);
    printf("%s\n", str);  // olleH
    return 0;
}
```

## 실전 예제: 단어 개수 세기

```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int countWords(char *str) {
    int count = 0;
    int inWord = 0;
    
    for (int i = 0; str[i] != '\0'; i++) {
        if (isspace(str[i])) {
            inWord = 0;
        } else if (!inWord) {
            inWord = 1;
            count++;
        }
    }
    
    return count;
}

int main() {
    char str[] = "Hello World C Programming";
    printf("단어 개수: %d\n", countWords(str));  // 4
    return 0;
}
```

## 실전 예제: 회문(Palindrome) 확인

```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int isPalindrome(char *str) {
    int len = strlen(str);
    int start = 0;
    int end = len - 1;
    
    while (start < end) {
        // 공백과 대소문자 무시
        while (isspace(str[start])) start++;
        while (isspace(str[end])) end--;
        
        if (tolower(str[start]) != tolower(str[end])) {
            return 0;  // 회문이 아님
        }
        start++;
        end--;
    }
    
    return 1;  // 회문임
}

int main() {
    char str[] = "A man a plan a canal Panama";
    if (isPalindrome(str)) {
        printf("회문입니다.\n");
    } else {
        printf("회문이 아닙니다.\n");
    }
    return 0;
}
```

## 주요 문자열 함수 정리

| 함수 | 설명 | 헤더 |
|------|------|------|
| `strlen()` | 문자열 길이 | `<string.h>` |
| `strcpy()` | 문자열 복사 | `<string.h>` |
| `strcat()` | 문자열 연결 | `<string.h>` |
| `strcmp()` | 문자열 비교 | `<string.h>` |
| `strchr()` | 문자 검색 | `<string.h>` |
| `strstr()` | 문자열 검색 | `<string.h>` |
| `strtok()` | 문자열 토큰화 | `<string.h>` |

## 주의사항

1. **배열 크기**: 충분한 크기를 할당해야 함
2. **널 문자**: 문자열 끝에 `\0` 필요
3. **버퍼 오버플로우**: `strcpy`, `strcat` 사용 시 주의
4. **안전한 함수**: `strncpy`, `strncat` 사용 권장

## 다음에 공부할 내용

다음 포스트에서는 구조체에 대해 공부해보겠습니다.

