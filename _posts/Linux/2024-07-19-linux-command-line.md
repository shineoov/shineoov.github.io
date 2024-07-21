---
layout: post
title: Linux Command Line
category: Linux
tags:
  - Linux
date: 2024-07-19 16:00:00 +0900
last_modified_at: 2024-07-21 15:39:00 +0900
---

## 텍스트 처리

### head
> 문서 내용의 앞부분을 출력

**Examples**
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

**Examples**
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

**Examples**
```bash
# line word byte filename  출력
$ wc ${FILE_NAME} 
> 40      74    1449    ${FILE_NAME}

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

**Options**
- `-v`: 첫번째 줄 번호 변경
- `-s`: 줄 번호 뒨에 문자열 추가

**Examples**
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

**Options**
- `-k`, `--key=KEYDEF`: key 에 의한 정렬
- `-t`, `--field-seperator`: 필드 구분자 (기본값은 공백 문자)
- `-f`, `--ignore-case`: 대소문자 구별 X
- `-n`, `--numeric-sort`: 숫자로 정렬
- `-g`, `--general-numeric-sort`:숫자로 정렬 (소수점 처리)
- `-r`, `--reverse`: 역방향 정렬
- `-u`, `--unique`: 중복 제거
- `--debug`: 디버깅 출력

**Examples**
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

**Options**
- `-d`, `--repeated`: 중복된 내용만 출력
- `-u`, `--unique`: 중복되지 않은 내용만 출력
- `-i`, `--ignore-case`: 대소문자 무시

**Examples**
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

### cut
> 컬럼 잘라 내기

**Options**
- `-b`, `--bytes=LIST`: byte 선택
- `-c`, `--characters=LIST`:character 선택
- `-f`, `--fields=LIST`: field 선택
- `-d`, `--delimiter=DELIM` : tab 대신 사용할 구분자 지정
- `--complement`: 선택 반전
- `--output-delimiter=STRING`: 출력시 사용할 구분자 지정

**Examples**
```bash

# 주어진 문자열에 "1" 번째 문자열 출력
$ echo "a:b:c:d:e" | cut -b 1
> a

# 주어진 문자열에 "1" 번째 부터 "3" 번째 문자열 출력
$ echo "a:b:c:d:e" | cut -b 1-3
> a:b

# 주어진 문자열에 ":" 로 구분 하고 "1" 번째 "3" 번째 출력 하고 구분자를 " = " 로 출력 (MacOS 에선 안됨)
$ echo "a:b:c:d:e" | cut -d":" -f 1,3 --output-delimiter=" = "
> a = c

# 주어진 문자열에서 "1" 번째 부터 "5" 번째 까지 출력
$ echo "a:b:c:d:e" | cut -c 1-5
> a:b:c

# 주어진 문자열에서 "3"번째 문자열 부터 출력
$ echo "a:b:c:d:e" | cut -c 3-
> b:c:d:e

# 주어진 문자열에서 "3"번째 문자열 까지 출력
$ echo "a:b:c:d:e" | cut -c -3
> a:b
```

### tr
> 문자 내용을 변환하거나 지우기

**Examples**
```bash
# 주어진 문자에서 ":" 문자열을 '?' 로 변환
$ echo "a:b:c" | tr  ':' '?'

# 주어진 문자에서 대문자를 소문자로 변환
$ echo "It's easy" | tr [:upper:] [:lower:]

# 주어진 문자에서 "a" ~ "g" 문자열을 "*" 로 변환 
$ echo "abcdefghijklmn"| tr "a-g" "*"

# "a", "b", "c" 를 제외한 문자열을 '*' 로 치환
$ echo "abcdefg" | tr -c 'abc' '*'

# 주어진 문자열에 ":" 제거
$ echo "a:b:c" | tr -d ":"

# 주어진 문자열에 "a", "b" 제거
$ echo "abc" | tr -d "ab"
```

### sed
> stream editor

**Options**
- `{RANGE}p`: range 내의 라인을 출력
- `{RANGE}d`: range 내의 라인을 삭제
- `/SEARCHPATTERN/p`:  SEARCH PATTERN 과 매치되는 라인을 출력
- `/SEARCHPATTERN/d`:  SEARCH PATTERN 과 매치되는 라인을 삭제
- `s/REGEX/REPLACE/`: REGEX 에 매치되는 부분을 REPLACE 로 교체 (substitute) 

**Examples**
```bash
# "1" ~ "5" 라인 까지만 출력
$ cat ${FILE_NAME} | sed -n '1,5'

# "1" ~ "5" 라인은 삭제
$ cat ${FILE_NAME} | sed '1,5d'

# "1" 문자열과 일치하는 라인만 출력
$ cat ${FILE_NAME} | sed -n '/1/p'

# "1" 문자열과 일치하지 않는 라인만 출력
$ cat ${FILE_NAME} | sed  '/1/d'

# "1" 문자열을 "*" 로 변환하여 출력 (라인당 하나만 변환)
$ cat ${FILE_NAME} | sed  's/1/*/'

# "1" 문자열 모두를 "*" 로 변환하여 출력
$ cat ${FILE_NAME} | sed  's/1/*/g'

# "4" 라는 문자열이 있는 라인 부터 "9" 라인 까지 출력
$ cat ${FILE_NAME} | sed -n '/4/,9p'

# "4" 라는 문자열이 있는 라인 부터 "4" + "3" 라인 까지 출력
$ cat ${FILE_NAME} | sed -n '/4/,+3p'
```

### awk
> 텍스트 처리 script language 

**Options**
- `-F`: field separator 지정

**Built-in Variables**
- `$1`, `$2`, `$3`: Nth Field
- `NR`: 라인 번호 ( number of records )
- `NF`: 필드 개수 ( number of fields )
- `FS`: 필드 구분자  ( field separator (default 'white space') )
- `RS`:  레코드 구분자 ( record separator(default 'new line') )
- `OFS`: 출력 필드 구분자 ( Output field separator )
- `ORS`: 출력 레코드 구분자 ( Output record separator )

**Examples**
```bash
# "2" 번째 필드 출력 ( white space 로 구분 )
$ cat ${FILE_NAME} | awk '{ print $2}'

# ":" 로 필드를 구분하고 "2" 번째 필드 출력
$ cat ${FILE_NAME} | awk -F: '{ print $1}'

# "7" 로 검색하고 검색된 라인에 "1" 번째 필드 출력
$ cat ${FILE_NAME} | awk -F: '/7/{ print $1}'

# 기타 내장 변수 사용
$ cat ${FILE_NAME} | awk -F: '/sd/ { print $3}'
$ cat ${FILE_NAME} | awk -F: '{ print NR, "==>", NF}'
$ cat ${FILE_NAME} | awk -F: '{ print NR "==>" NF}'
```

## 검색 
### find
> 조건에 맞는 파일을 찾아 명령을 수행한다. 

**Options**
- **조건 옵션**
	- `-name` : 이름으로 검색
	- `-regex`: regex 에 매치로 검색
	- `-empty`: 빈 디렉토리 혹은 빈 파일 검색
	- `-size`: 사이즈로 검색 (M, G 표시 가능)
	- `-type`: 파일 타입으로 검색
		- `-type d`: 디렉토리
		- `-type f`: 일반 파일
		- `-type l`: symbolic 링크
	- `-perm`: 퍼미션으로 검색
- **액션 옵션**
	- `-delete`: 파일 삭제
	- `-ls`: ls -dils 명령 수행
	- `-print`: 파일 이름 출력
	- `-printf`: 파일 이름을 포맷에 맞게 출력
	- `-exec`: 주어진 명령 수행
	- `-execdir`: 해당 디렉토리로 이동후 명령 수행
	- `-ok`: 사용자에게 확인 후 `exec`
	- `-okdir`: 사용자에게 확인 후 `execdir`

**Examples**
```bash
# 현재 디렉토리 (".") 에 있는 모든 파일 출력
$ find . 

#  현재 디렉토리에 (`pwd`) 있는 모든 파일들을 출력 (절대 경로로 출력됨)
$ find `pwd`

# 내용이 빈 파일 검색
$ find . -empty

# 디렉토리 검색
$ find . -type d

# 파일 검색
$ find . -type f

# 현재 디렉토리 (".") 에 있는 "*.log" 에 맞는 파일 ("f") 검색
$ find . -name "*.log" -type f

# 현재 디렉토리 (".") 에 있는 "test" 가 들어 가는 파일, 디렉토리 검색
$ find . -name "*test*"

# 현재 디렉토리 (.) 에 있는 모든 파일에서 grep 실행
$ find . | grep "\.log"

# "0777" 퍼미션을 가지고 있는 파일 검색
$ find . -perm 0777

# "0777" 퍼미션을 가지고 있는 파일 수 조회 (wc)
$ find . -perm 0644 | wc -l

# 실행 권한이 있는 파일 정보 출력
$ find . -perm /u+x -ls

# "*.yaml" 에 해당하는 파일들을 "stat" 실행
$ find . -name "*.yaml" -exec stat {} \;
$ find . -name "*.yaml" -ok stat {} \;

# "*.yaml" 에 해당하는 파일들을 "rm" 실행 할지 여부를 물어보고 실행
$ find . -name "*.yaml" -ok rm -f {} \;

# "*.yaml" 에 해당하는 파일들을 "stat" 실행 단 디렉토리 이동후 실행
$ find . -name "*.yaml" -exec stat {} \;
$ find . -name "*.yaml" -okdir stat {} \;
```

### grep
> 파일 내용 중 원하는 내용을 찾는다.  

**Options**
- `-r` : 하위 디렉토리 전체 검색 ( recursive )
- `-i`:  대소문자를 무시하고 검색 ( ignore case )
- `-v`:  일치 하지 않는 결과 검색 ( invert match)
- `-q`: 검색 결과를 출력 하지 않음 ( quiet mode )

**Examples**
```bash
# "datetime" 문자열이 포함된 모든 하위 파일을 검색
$ grep -r "datetime" *

# "datetime" 문자열이 포함된 모든 하위 파일을 단어 단위로 검색
$ grep -r "\<datetime\>" *

# 하위 디렉터리의 모든 파일에서 "function"으로 시작하고 "void"로 끝나는 문자열이 포함된 파일을 검색
$ grep -r "function.*.void" *

# 대소문자를 구분하지 않고 현재 디렉토리의 모든 파일에서 "datetime" 문자열을 검색
$ grep -i "datetime" *

# "datetime" 문자열이 포함되지 않은 결과를 출력
$ grep -v "datetime" *

# "datetime" 문자열이 포함된 파일들을 찾아 중복 없이 정렬된 파일 목록 출력
$ grep -ri "datetime" * | awk -F: '{ print $1 }' | sort -uR

# 현재 디렉토리의 파일들에서 "datetime" 문자열을 출력 없이 검색 
$ grep "datetime" * -q

# grep 명령어의 종료 상태를 확인하여 검색 결과 유무를 판단 
# 0: 검색 결과가 있음, 1: 검색 결과가 없음
$ echo $?
```

### apropos
> man page 이름과 설명을 검색한다

**Options**
- `-s`, `--sections=LIST`, `--section=LIST`: 탐색할 섹션을 colon 으로 구분하여 입력

**Examples**
```bash
# "{keyword}" 를 포함하는 매뉴얼 페이지들의 목록을 출력
$ apropos {keyword}

# "{keyword}"를 포함하는 섹션 "1" 의 매뉴얼 페이지들의 목록을 출력
# 1 - User Commands (사용자 명령어)): 일반 사용자가 실행할 수 있는 명령어입니다.
# 2 - System Calls (시스템 호출)): 커널이 제공하는 함수로, 주로 프로그래밍할 때 사용됩니다.
# 3 - Library Calls (라이브러리 호출): 표준 라이브러리 함수로, 역시 프로그래밍할 때 사용됩니다.
# 5 - File Formats and Conventions (파일 형식 및 규약): 파일 형식, 설정 파일의 형식 등을 설명합니다. 예: `/etc/passwd`
$ apropos {keyword} -s 1
```
### which
> 실행 파일의 위치를 보여준다

**Examples**
```bash
# ls 명령어에 파일 위치 출력
$ which ls

# chmod 명령어에 파일 위치 출력
$ which chmod

# ls, chmod 명령어에 파일 위치 출력
$ which ls chmod
```

