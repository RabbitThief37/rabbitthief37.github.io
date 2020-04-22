---
layout: post
title:  "프로젝트-빵테온 SQLite3 설치 및 테이블 설계"
description: "토이 프로젝트인 프로젝트-빵테온에서 사용할 DB Table 설계한다"
author: RabbitThief
date: 2020-04-22 16:51:00 +0900
tags: raspberrypi nodejs sqlite3
category: Raspberrypi

comments: false
---	



## USER_INFO

사용자 정보를 등록

| #    | NAME  | TYPE    | DEFAULT | DESCRIPTION               |
| ---- | ----- | ------- | ------- | ------------------------- |
| 1    | IDX   | INTEGER |         | PRIMARY KEY AUTOINCREMENT |
| 2    | NAME  | TEXT    |         | 다현, 병현, 엄마, 아빠    |
| 3    | LEVEL | INTEGER |         | 0-ADMIN, 1-USER           |

```sqlite
/*********** USER_INFO ***********/ 
DROP TABLE IF EXISTS USER_INFO;

CREATE TABLE USER_INFO ( 
	IDX INTEGER PRIMARY KEY AUTOINCREMENT,
	NAME TEXT,
	LEVEL INTEGER );
	
INSERT INTO USER_INFO(NAME,LEVEL) VALUES ('아빠', 0);
INSERT INTO USER_INFO(NAME,LEVEL) VALUES ('엄마', 0);
INSERT INTO USER_INFO(NAME,LEVEL) VALUES ('다현', 1);
INSERT INTO USER_INFO(NAME,LEVEL) VALUES ('병현', 1);
```



## ITEM_INFO

아이들이 해야 할 항목 리스트 

| #    | **NAME**    | **TYPE** | **DEFAULT**       | **DESCRIPTION**           |
| ---- | ----------- | -------- | ----------------- | ------------------------- |
| 1    | IDX         | INTEGER  |                   | PRIMARY KEY AUTOINCREMENT |
| 2    | NAME        | TEXT     |                   | 항목 이름                 |
| 3    | USE_YN      | INTEGER  | 1                 | 0 - 삭제 , 1 - 사용       |
| 4    | ENROLL_DATE | NUMERIC  | CURRENT_TIMESTAMP | 등록한 날짜               |
| 5    | NOTE        | TEXT     | 없음              | 추가 설명                 |

```sqlite
/*********** ITEM_INFO ***********/ 
DROP TABLE IF EXISTS ITEM_INFO;

CREATE TABLE ITEM_INFO ( 
	IDX INTEGER PRIMARY KEY AUTOINCREMENT,
	NAME TEXT,
	USE_YN INTEGER DEFAULT 1,
	ENROLL_DATE DEFAULT CURRENT_TIMESTAMP,
	NOTE TEXT DEFAULT '없음' );

INSERT INTO ITEM_INFO(NAME) VALUES ('줄넘기1000-운동');
INSERT INTO ITEM_INFO(NAME) VALUES ('리틀박스 집중듣기');
INSERT INTO ITEM_INFO(NAME) VALUES ('센(수학)');
INSERT INTO ITEM_INFO(NAME) VALUES ('e학습터/EBS');
INSERT INTO ITEM_INFO(NAME,NOTE) VALUES ('성경읽기','읽은장');
INSERT INTO ITEM_INFO(NAME,NOTE) VALUES ('만점왕(국사과)30','과목명');
INSERT INTO ITEM_INFO(NAME) VALUES ('웅진빅박스');
INSERT INTO ITEM_INFO(NAME) VALUES ('색깔셈');
INSERT INTO ITEM_INFO(NAME) VALUES ('EBS/문제집');
INSERT INTO ITEM_INFO(NAME) VALUES ('책5권 읽기');
INSERT INTO ITEM_INFO(NAME) VALUES ('e학습터');
INSERT INTO ITEM_INFO(NAME,USE_YN) VALUES ('학교특강EBS',0);
```

 

## TODO_ITEMS

항목 리스트 중에서 부모가 항목을 선택해서, 하루 안에 완료해야 하는 것으로 정한 것

| **#** | **NAME**      | **TYPE** | **DEFAULT**       | **DESCRIPTION**            |
| ----- | ------------- | -------- | ----------------- | -------------------------- |
| 1     | IDX           | INTEGER  |                   | PRIMARY KEY  AUTOINCREMENT |
| 2     | USER_INFO_IDX | INTEGER  |                   | USER_INFO.IDX              |
| 3     | ITEM_INFO_IDX | INTEGER  |                   | ITEM_INFO.IDX              |
| 4     | USE_YN        | INTEGER  | 1                 | 0 - 삭제 , 1 - 사용        |
| 5     | ENROLL_DATE   | NUMERIC  | CURRENT_TIMESTAMP | 등록한 날짜                |

```sqlite
/*********** TODO_ITEMS ***********/ 
DROP TABLE IF EXISTS TODO_ITEMS;

CREATE TABLE TODO_ITEMS ( 
	IDX INTEGER PRIMARY KEY AUTOINCREMENT,
	USER_INFO_IDX INTEGER,
	ITEM_INFO_IDX INTEGER,
	USE_YN INTEGER DEFAULT 1,
ENROLL_DATE DEFAULT CURRENT_TIMESTAMP );

INSERT INTO TODO_ITEMS(USER_INFO_IDX, ITEM_INFO_IDX) SELECT A.IDX, B.IDX FROM USER_INFO A, ITEM_INFO B WHERE A.NAME = '다현' AND B.NAME = '줄넘기1000-운동';
INSERT INTO TODO_ITEMS(USER_INFO_IDX, ITEM_INFO_IDX) SELECT A.IDX, B.IDX FROM USER_INFO A, ITEM_INFO B WHERE A.NAME = '다현' AND B.NAME = '리틀박스 집중 듣기';
INSERT INTO TODO_ITEMS(USER_INFO_IDX, ITEM_INFO_IDX) SELECT A.IDX, B.IDX FROM USER_INFO A, ITEM_INFO B WHERE A.NAME = '다현' AND B.NAME = '센(수학)';
INSERT INTO TODO_ITEMS(USER_INFO_IDX, ITEM_INFO_IDX) SELECT A.IDX, B.IDX FROM USER_INFO A, ITEM_INFO B WHERE A.NAME = '다현' AND B.NAME = 'e학습터/EBS';
INSERT INTO TODO_ITEMS(USER_INFO_IDX, ITEM_INFO_IDX) SELECT A.IDX, B.IDX FROM USER_INFO A, ITEM_INFO B WHERE A.NAME = '다현' AND B.NAME = '성경읽기';
INSERT INTO TODO_ITEMS(USER_INFO_IDX, ITEM_INFO_IDX) SELECT A.IDX, B.IDX FROM USER_INFO A, ITEM_INFO B WHERE A.NAME = '다현' AND B.NAME = '만점왕(국사과)30';
INSERT INTO TODO_ITEMS(USER_INFO_IDX, ITEM_INFO_IDX) SELECT A.IDX, B.IDX FROM USER_INFO A, ITEM_INFO B WHERE A.NAME = '다현' AND B.NAME = '웅진빅박스';

INSERT INTO TODO_ITEMS(USER_INFO_IDX, ITEM_INFO_IDX) SELECT A.IDX, B.IDX FROM USER_INFO A, ITEM_INFO B WHERE A.NAME = '병현' AND B.NAME = '줄넘기1000-운동';
INSERT INTO TODO_ITEMS(USER_INFO_IDX, ITEM_INFO_IDX) SELECT A.IDX, B.IDX FROM USER_INFO A, ITEM_INFO B WHERE A.NAME = '병현' AND B.NAME = '리틀박스 집중 듣기';
INSERT INTO TODO_ITEMS(USER_INFO_IDX, ITEM_INFO_IDX) SELECT A.IDX, B.IDX FROM USER_INFO A, ITEM_INFO B WHERE A.NAME = '병현' AND B.NAME = '색깔셈';
INSERT INTO TODO_ITEMS(USER_INFO_IDX, ITEM_INFO_IDX) SELECT A.IDX, B.IDX FROM USER_INFO A, ITEM_INFO B WHERE A.NAME = '병현' AND B.NAME = 'EBS/문제집';
INSERT INTO TODO_ITEMS(USER_INFO_IDX, ITEM_INFO_IDX) SELECT A.IDX, B.IDX FROM USER_INFO A, ITEM_INFO B WHERE A.NAME = '병현' AND B.NAME = '성경읽기';
INSERT INTO TODO_ITEMS(USER_INFO_IDX, ITEM_INFO_IDX) SELECT A.IDX, B.IDX FROM USER_INFO A, ITEM_INFO B WHERE A.NAME = '병현' AND B.NAME = '책5권 읽기';
INSERT INTO TODO_ITEMS(USER_INFO_IDX, ITEM_INFO_IDX) SELECT A.IDX, B.IDX FROM USER_INFO A, ITEM_INFO B WHERE A.NAME = '병현' AND B.NAME = '웅진빅박스';
INSERT INTO TODO_ITEMS(USER_INFO_IDX, ITEM_INFO_IDX) SELECT A.IDX, B.IDX FROM USER_INFO A, ITEM_INFO B WHERE A.NAME = '병현' AND B.NAME = 'e학습터';
INSERT INTO TODO_ITEMS(USER_INFO_IDX, ITEM_INFO_IDX, USE_YN) SELECT A.IDX, B.IDX, 0 FROM USER_INFO A, ITEM_INFO B WHERE A.NAME = '병현' AND B.NAME = '학교특강EBS';
```



## CHECK_TODO_ITEMS

해야 할 항목을 완료했는지를 기록

| **#** | **NAME**         | **TYPE** | **DEFAULT**       | **DESCRIPTION**            |
| ----- | ---------------- | -------- | ----------------- | -------------------------- |
| 1     | IDX              | INTEGER  |                   | PRIMARY KEY  AUTOINCREMENT |
| 2     | TODO_ITEMS_IDX   | INTEGER  |                   | TODO_ITEMS.IDX             |
| 3     | COMPLETE_YN      | INTEGER  | 0                 | 0 - 할일 , 1 - 완료        |
| 4     | COMPLETE_NOTE    | TEXT     |                   | 상세 내용 필요시           |
| 5     | COMPLETE_DATE    | NUMERIC  |                   | 완료한 날짜                |
| 6     | MOMMY_CHECK_YN   | INTEGER  | 0                 | 0 - 미확인 , 1 - 확인      |
| 7     | MOMMY_CHECK_DATE | NUMERIC  |                   | 확인한 날짜                |
| 8     | ENROLL_DATE      | NUMERIC  | CURRENT_TIMESTAMP | 등록한 날짜                |

```sqlite
/*********** CHECK_TODO_ITEMS ***********/
DROP TABLE IF EXISTS CHECK_TODO_ITEMS;

CREATE TABLE CHECK_TODO_ITEMS ( 
	IDX INTEGER PRIMARY KEY AUTOINCREMENT,
	TODO_ITEMS_IDX INTEGER,
	COMPLETE_YN INTEGER DEFAULT 0,
	COMPLETE_NOTE TEXT,
	COMPLETE_DATE NUMERIC,
	MOMMY_CHECK_YN INTEGER DEFAULT 0,
	MOMMY_CHECK_DATE NUMERIC,
	ENROLL_DATE DEFAULT CURRENT_TIMESTAMP );

-- 다현 , 줄넘기 1000
INSERT INTO CHECK_TODO_ITEMS(TODO_ITEMS_IDX,COMPLETE_YN,COMPLETE_DATE,MOMMY_CHECK_YN,MOMMY_CHECK_DATE,ENROLL_DATE) 
VALUES (1,1,datetime('2020-03-30'),1,datetime('2020-03-30'),datetime('2020-03-30'));
INSERT INTO CHECK_TODO_ITEMS(TODO_ITEMS_IDX,COMPLETE_YN,COMPLETE_DATE,MOMMY_CHECK_YN,MOMMY_CHECK_DATE,ENROLL_DATE) 
VALUES (1,1,datetime('2020-03-31'),1,datetime('2020-03-31'),datetime('2020-03-31'));
INSERT INTO CHECK_TODO_ITEMS(TODO_ITEMS_IDX,COMPLETE_YN,COMPLETE_DATE,MOMMY_CHECK_YN,MOMMY_CHECK_DATE,ENROLL_DATE) 
VALUES (1,1,datetime('2020-04-01'),1,datetime('2020-04-01'),datetime('2020-04-01'));
...
...
-- 병현 , e학습터
INSERT INTO CHECK_TODO_ITEMS(TODO_ITEMS_IDX,COMPLETE_YN,COMPLETE_DATE,MOMMY_CHECK_YN,MOMMY_CHECK_DATE,ENROLL_DATE) 
VALUES (15,1,datetime('2020-04-20'),0,datetime('2020-04-20'),datetime('2020-04-20'));
INSERT INTO CHECK_TODO_ITEMS(TODO_ITEMS_IDX,COMPLETE_YN,COMPLETE_DATE,MOMMY_CHECK_YN,MOMMY_CHECK_DATE,ENROLL_DATE) 
VALUES (15,1,datetime('2020-04-21'),0,datetime('2020-04-21'),datetime('2020-04-21'));
```



## MOMMY_POINT

특별한 경우에 엄마의 권한으로 포인트를 추가로 줄 수 있다.  (ex.성경1독 , 심부름)

| **#** | **NAME**      | **TYPE** | **DEFAULT**       | **DESCRIPTION**            |
| ----- | ------------- | -------- | ----------------- | -------------------------- |
| 1     | IDX           | INTEGER  |                   | PRIMARY KEY  AUTOINCREMENT |
| 2     | USER_INFO_IDX | INTEGER  |                   | USER_INFO.IDX              |
| 3     | POINT         | INTEGER  |                   | 포인트                     |
| 4     | DATE_TIME     | NUMERIC  | CURRENT_TIMESTAMP | 등록한 날짜                |
| 5     | REASON        | TEXT     |                   | 포인트 지급 이유           |

```sqlite
/*********** MOMMY_POINT ***********/
DROP TABLE IF EXISTS MOMMY_POINT;

CREATE TABLE MOMMY_POINT ( 
	IDX INTEGER PRIMARY KEY AUTOINCREMENT,
	USER_INFO_IDX INTEGER,
	POINT INTEGER,
	DATE_TIME DEFAULT CURRENT_TIMESTAMP,
	REASON TEXT );

INSERT INTO MOMMY_POINT(USER_INFO_IDX, POINT, DATE_TIME, REASON) VALUES (3,690,datetime('2020-03-30'),'이전달 이월');
INSERT INTO MOMMY_POINT(USER_INFO_IDX, POINT, DATE_TIME, REASON) VALUES (3,200,datetime('2020-04-06'),'사회책걸이');
INSERT INTO MOMMY_POINT(USER_INFO_IDX, POINT, DATE_TIME, REASON) VALUES (3,200,datetime('2020-04-13'),'요한복음 3독');
INSERT INTO MOMMY_POINT(USER_INFO_IDX, POINT, DATE_TIME, REASON) VALUES (4,10,datetime('2020-03-30'),'이전달 이월');
INSERT INTO MOMMY_POINT(USER_INFO_IDX, POINT, DATE_TIME, REASON) VALUES (4,200,datetime('2020-04-01'),'요한복음 1독');
INSERT INTO MOMMY_POINT(USER_INFO_IDX, POINT, DATE_TIME, REASON) VALUES (4,100,datetime('2020-04-06'),'알파북');
```



## COMPUTE_POINT

포인트 결산을 한 결과

- 한 가지 항목을 완료 하면 10 point
- 하루에 해야 할 모든 항목을 완료하면 100 point
- 한 주(5일)동안 모든 항목을 완료하면 1,000 point
- 토, 일, 공휴일에 한 가지 항목을 완료하면 20 point
- 한 달(4주)동안 모든 항목을 완료하면  10,000 point

| **#** | **NAME**      | **TYPE** | **DEFAULT**       | **DESCRIPTION**                |
| ----- | ------------- | -------- | ----------------- | ------------------------------ |
| 1     | IDX           | INTEGER  |                   | PRIMARY KEY  AUTOINCREMENT     |
| 2     | USER_INFO_IDX | INTEGER  |                   | USER_INFO.IDX                  |
| 3     | TYPE          | INTEGER  |                   | 1 - DAY , 2 - WEEK , 3 - MONTH |
| 4     | DATE_TIME     | NUMERIC  | CURRENT_TIMESTAMP | 계산한 날짜                    |
| 5     | TOTAL_POINT   | INTEGER  | 0                 | 총합                           |
| 6     | PAYMENT_YN    | INTEGER  | 0                 | 0 - NOT PAY , 1 - FIX          |

```sqlite
/*********** COMPUTE_POINT ***********/	
DROP TABLE IF EXISTS COMPUTE_POINT;

CREATE TABLE COMPUTE_POINT ( 
	IDX INTEGER PRIMARY KEY AUTOINCREMENT,
	USER_INFO_IDX INTEGER,
	TYPE INTEGER,
	DATE_TIME DEFAULT CURRENT_TIMESTAMP,
	TOTAL_POINT INTEGER,
	PAYMENT_YN INTEGER );
```



## 실행 결과

![2](/Users/kskim/Documents/rabbitthief37.github.io/assets/article_images/2020-04-22/2.png)

약 700 라인의 SQL이라서 시간이 걸릴 줄 알았는데, 1분도 안 걸려서 결과가 처리가 완료되었다.  오~

이제는 node.js로 각종 처리하는 부분을 만들어야 할 차례가 왔네.