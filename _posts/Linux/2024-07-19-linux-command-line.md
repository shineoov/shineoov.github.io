---
layout: post
title: Linux Command Line
category: Linux
tags:
  - Linux
date: 2024-07-19 14:00:00 +0900
last_modified_at: 2024-07-19 15:00:00 +0900
---

## 텍스트 처리

### head
> 문서 내용의 앞부분을 출력

```bash
# Default 10
$ head ${FILE_NAME}

# 앞부분 부터 20 line 출력
$ head -n 20 ${FILE_NAME}

# 마지막 5 line 제외 하고 출력
$ head -n -5 ${FILE_NAME}
```

### tail
>문서 내용의 뒷부분을 출력

```bash
# Default 10
$ tail ${FILE_NAME}

# 뒷부분 20 line 출력
$ tail -n 20 ${FILE_NAME}

# 앞부분 5 line 제외 하고 출력
$ tail -n +5 ${FILE_NAME}

# 추가되는 내용 대기하다가 추가되는 내용이 있으면 출력
$ tail -f ${FILE_NAME}

# 파일이 truncate 되는 경우 re open 하여 follow 함 (logrotate 되는 파일에 유용)
$ tail -F ${FILE_NAME} 
```

### wc
> line / word / byte count 출력

```bash
# line word byte filename  출력
$ wc ${FILE_NAME} 

# wildcard 사용
$ wc *.log 

# 라인 수만 출력
$ wc -l ${FILE_NAME}

# 워드 수만 출력
$ wc -w ${FILE_NAME}

# 바이트 수만 출력
$ wc -c ${FILE_NAME}

# 파일 이름을 제외한 라인 수만 출력 (첫 번째 값)
$ wc -l ${FILE_NAME} | awk '{ print $1}'
```

### nl
> 파일 내용을 라인 넘버와 함께 출력

```bash
$ nl ${FILE_NAME}

# 시작 라인 넘버를 10으로 지정
$ nl -v 10 ${FILE_NAME}

# 라인 넘버 출력시 separator 지정
$ nl -s " : " ${FILE_NAME}

# 모든 라인에 대해 라인 넘버링 (공백 포함)
$ nl -ba ${FILE_NAME}
```
### sort
> 정렬해서 출력

| 옵션                                | 설명                 |
| ---------------------------------- | ------------------- |
| **-k**, **--key=KEYDEF**           | key 에 의한 정렬         |
| **-t**, **--field-seperator**      | 필드 구분자 (기본값은 공백 문자) |
| **-f**, **--ignore-case**          | 대소문자 구별 X           |
| **-g**, **--general-numeric-sort** | 숫자로 정렬 (소수점 처리)     |
| **-n**, **--numeric-sort**         | 숫자로 정렬              |
| **-r**, **--reverse**              | 역방향 정렬              |
| -**u**, **--unique**               | 중복 제거               |
| **--debug**                        | 디버깅 출력              |

```bash
$ cat ${FILE_NAME} | sort

# 정렬 시 숫자로 정렬
$ cat ${FILE_NAME} | sort -n

# 필드 (":") 로 구분 해서 N, M 번째 값으로 정렬
$ cat ${FILE_NAME} | sort -t: -k N
$ cat ${FILE_NAME} | sort -t: -k N,M

# Debug 출력
$ cat ${FILE_NAME} | sort -t: -k N --debug

# 파일 사이즈 정렬
$ ls -al | sort -k N
```
### uniq
> 중복된 내용 제거하고 출력

| 옵션                        | 설명             |
| ------------------------- | -------------- |
| **-d**, **--repeated**    | 중복된 내용만 출력     |
| **-u**, **--unique**      | 중복되지 않은 내용만 출력 |
| **-i**, **--ignore-case** | 대소문자 무시        |

```bash

# 연속적인 중복 텍스트만 제거
$ uniq ${FILE_NAME}

# 모든 중복 텍스트를 제거하기 위해서는 정렬 후 중복 제거
$ sort ${FILE_NAME} | uniq
$ sort ${FILE_NAME} | uniq -i | nl

# 중복된 내용만 출력 
$ uniq -d ${FILE_NAME}

# 중복되지 않은 내용만 출력
$ uniq -u ${FILE_NAME}

# 대소문자 무시
$ uniq -i ${FILE_NAME} 

# grep 으로 검색 후 파일 이름 중복 제거 
$ grep -r "format" * | awk -F: '{print $1}' | uniq

```