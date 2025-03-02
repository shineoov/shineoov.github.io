---
layout: post
title: MySQL Json Column
category: DB
tags:
  - MySQL
date: 2024-07-04 15:00:00 +0900
last_modified_at: 2024-07-04 15:00:00 +0900
---
> 설명~

## 특징
- **JSON** 형식의 데이터를 손쉽게 저장 및 조회, 관리 할 수 있다.
- 빌트인 함수들을 사용해 **JSON** 데이터 조작 가능
- 저장된 **JSON** 데이터의 일부만 업데이트 가능 (부분 업데이트 기능)
- 저장된 **JSON** 데이터의 특정 키에 대해서 인덱스 생성 가능

## 사용
### 쿼리
```sql

-- CREATE
CREATE TABLE test_json (
	id int NOT NULL AUTO_INCREMENT,
	json_col1 json DEFAULT NULL,
	json_col2 json DEFAULT (json_object()),
	json_col3 json DEFAULT (json_array()),
	PRIMARY KEY (`id`)
)

-- INSERT
INSERT INTO test_json (json_col1) VALUES 
('[1, "ABC", "2023-12-01"]')

INSERT INTO test_json (json_col1) VALUES
(JSON_ARRAY('Text', 12345, NOW()))

INSERT INTO test_json (json_col1) VALUES
(JSON_OBJECT("key1", 1, "key2", "2", "key3", NOW()))



```



## References

