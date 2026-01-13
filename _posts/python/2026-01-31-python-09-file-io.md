---
layout: post
title:  "파이썬 강의 9편: 파일 입출력"
date:   2026-01-31 10:00:00 +0900
categories: [IT, 파이썬 강의]
tags: [파이썬, 파일입출력, 파일처리, file, IO]
description: "파이썬으로 파일을 읽고 쓰는 방법, with 문 사용, CSV/JSON 파일 처리 등 파일 입출력의 모든 기능을 배웁니다."
---

# 파이썬 강의 9편: 파일 입출력

## 파일 입출력이란?

파일 입출력은 파일에서 데이터를 읽거나 파일에 데이터를 쓰는 작업입니다. 파이썬은 파일 처리를 매우 쉽게 할 수 있습니다.

## 파일 열기

### 기본 형식

```python
file = open("파일명", "모드")
# 파일 작업
file.close()
```

### 파일 모드

- `"r"`: 읽기 모드 (기본값)
- `"w"`: 쓰기 모드 (파일이 있으면 덮어쓰기)
- `"a"`: 추가 모드 (파일 끝에 추가)
- `"x"`: 생성 모드 (파일이 없을 때만 생성)
- `"b"`: 바이너리 모드
- `"t"`: 텍스트 모드 (기본값)

## 파일 읽기

### read(): 전체 읽기

```python
file = open("example.txt", "r", encoding="utf-8")
content = file.read()
print(content)
file.close()
```

### readline(): 한 줄 읽기

```python
file = open("example.txt", "r", encoding="utf-8")
line = file.readline()
print(line)
file.close()
```

### readlines(): 모든 줄 읽기

```python
file = open("example.txt", "r", encoding="utf-8")
lines = file.readlines()
for line in lines:
    print(line.strip())  # strip()으로 줄바꿈 제거
file.close()
```

### 파일을 직접 순회

```python
file = open("example.txt", "r", encoding="utf-8")
for line in file:
    print(line.strip())
file.close()
```

## 파일 쓰기

### write(): 문자열 쓰기

```python
file = open("output.txt", "w", encoding="utf-8")
file.write("Hello, World!\n")
file.write("파이썬 파일 입출력")
file.close()
```

### writelines(): 여러 줄 쓰기

```python
lines = ["첫 번째 줄\n", "두 번째 줄\n", "세 번째 줄\n"]
file = open("output.txt", "w", encoding="utf-8")
file.writelines(lines)
file.close()
```

## with 문 사용 (권장)

파일을 자동으로 닫아줍니다:

```python
# 읽기
with open("example.txt", "r", encoding="utf-8") as file:
    content = file.read()
    print(content)
# 파일이 자동으로 닫힘

# 쓰기
with open("output.txt", "w", encoding="utf-8") as file:
    file.write("Hello, World!\n")
```

## 실전 예제: 파일 복사

```python
def copy_file(source, destination):
    with open(source, "r", encoding="utf-8") as src:
        content = src.read()
    
    with open(destination, "w", encoding="utf-8") as dst:
        dst.write(content)
    
    print(f"{source}를 {destination}로 복사했습니다.")

copy_file("source.txt", "copy.txt")
```

## 실전 예제: 파일 통계

```python
def file_statistics(filename):
    with open(filename, "r", encoding="utf-8") as file:
        content = file.read()
        lines = content.split("\n")
    
    stats = {
        "문자 수": len(content),
        "단어 수": len(content.split()),
        "줄 수": len(lines),
        "문장 수": content.count(".") + content.count("!") + content.count("?")
    }
    
    return stats

stats = file_statistics("example.txt")
for key, value in stats.items():
    print(f"{key}: {value}")
```

## CSV 파일 처리

CSV(Comma-Separated Values) 파일을 처리합니다:

```python
import csv

# CSV 파일 읽기
with open("students.csv", "r", encoding="utf-8") as file:
    reader = csv.reader(file)
    for row in reader:
        print(row)

# CSV 파일 쓰기
data = [
    ["이름", "나이", "점수"],
    ["홍길동", "25", "85"],
    ["김철수", "23", "90"]
]

with open("output.csv", "w", encoding="utf-8", newline="") as file:
    writer = csv.writer(file)
    writer.writerows(data)
```

## JSON 파일 처리

JSON(JavaScript Object Notation) 파일을 처리합니다:

```python
import json

# JSON 파일 읽기
with open("data.json", "r", encoding="utf-8") as file:
    data = json.load(file)
    print(data)

# JSON 파일 쓰기
data = {
    "name": "홍길동",
    "age": 25,
    "scores": [85, 90, 88]
}

with open("output.json", "w", encoding="utf-8") as file:
    json.dump(data, file, ensure_ascii=False, indent=2)
```

## 실전 예제: 로그 파일 분석

```python
def analyze_log(log_file):
    error_count = 0
    warning_count = 0
    
    with open(log_file, "r", encoding="utf-8") as file:
        for line in file:
            if "ERROR" in line:
                error_count += 1
            elif "WARNING" in line:
                warning_count += 1
    
    print(f"에러: {error_count}개")
    print(f"경고: {warning_count}개")

analyze_log("app.log")
```

## 실전 예제: 설정 파일 읽기

```python
def read_config(config_file):
    config = {}
    
    with open(config_file, "r", encoding="utf-8") as file:
        for line in file:
            line = line.strip()
            if line and not line.startswith("#"):  # 주석 제외
                key, value = line.split("=")
                config[key.strip()] = value.strip()
    
    return config

config = read_config("config.txt")
print(config)
```

## 파일 존재 확인

```python
import os

filename = "example.txt"

if os.path.exists(filename):
    print(f"{filename} 파일이 존재합니다.")
else:
    print(f"{filename} 파일이 없습니다.")
```

## 디렉토리 작업

```python
import os

# 현재 디렉토리
print(os.getcwd())

# 파일 목록
files = os.listdir(".")
for file in files:
    print(file)

# 디렉토리 생성
os.makedirs("new_folder", exist_ok=True)
```

## 예외 처리

파일 작업 시 오류가 발생할 수 있으므로 예외 처리가 필요합니다:

```python
try:
    with open("example.txt", "r", encoding="utf-8") as file:
        content = file.read()
        print(content)
except FileNotFoundError:
    print("파일을 찾을 수 없습니다.")
except PermissionError:
    print("파일 접근 권한이 없습니다.")
except Exception as e:
    print(f"오류 발생: {e}")
```

## 실전 예제: 학생 정보 관리 시스템

```python
import json

def save_students(students, filename):
    """학생 정보를 JSON 파일로 저장"""
    with open(filename, "w", encoding="utf-8") as file:
        json.dump(students, file, ensure_ascii=False, indent=2)

def load_students(filename):
    """JSON 파일에서 학생 정보 불러오기"""
    try:
        with open(filename, "r", encoding="utf-8") as file:
            return json.load(file)
    except FileNotFoundError:
        return []

# 사용
students = [
    {"name": "홍길동", "age": 25, "score": 85},
    {"name": "김철수", "age": 23, "score": 90}
]

save_students(students, "students.json")
loaded_students = load_students("students.json")
print(loaded_students)
```

## 주의사항

1. **인코딩**: 한글 파일은 `encoding="utf-8"` 사용
2. **파일 닫기**: `with` 문 사용 권장
3. **예외 처리**: 파일이 없을 수 있으므로 처리 필요
4. **경로**: 상대 경로와 절대 경로 구분

## 다음에 공부할 내용

다음 포스트에서는 클래스와 객체에 대해 공부해보겠습니다.

