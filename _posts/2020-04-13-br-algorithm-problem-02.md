---
layout: post
title:  "프로그래밍 대회에서 배우는 알고리즘 문제해결전략 (1/2) #2"
description: "프로그래밍 대회에서 배우는 알고리즘 문제해결전략 (1/2) #2"
author: RabbitThief
date:   2020-04-13 15:38:00 +0900
tags: algorithm development book 
category: Books
comments: false
---



# 3 코딩과 디버깅에 관하여



### 3.2 좋은 코드를 짜기 위한 원칙

- 간결한 코드를 작성하기 

```c++
bool hasDuplicate(const vector<int>& array) {
  for(int i = 0; i < array.size(); ++i)
    for(int j = 0; j < i; ++j)
      if(array[i] == array[j])
        return true;
  return false;
}
```



```c++
#define FOR(i,n) 	for(int i=0; i<(n); ++i)
bool hasDuplicate(const vector<int>& array) {
	FOR(i,array.size())
    FOR(j,i)   // ++ 실수 방지
    	if(array[i] == array[j])
        return true;
  return false;
}
```



- 적극적으로 코드 재사용하기
- 표준 라이브러리 공부하기
- 항상 같은 형태로 프로그램을 작성하기
- 일관적이고 명료한 명명법 사용하기
- 모든 자료를 정규화해서 저장하기
- 코드와 데이터를 분리하기



### 3.3 자주 하는 실수

- 산술 오버플로
- 배열 범위 밖 원소에 접근
- 일관되지 않은 범위 표현 방식 사용하기
- Off-by-one 오류
- 컴파일러가 잡아주는 못하는 상수 오타
- 스택 오버플로
- 다차원 배열 인덱스 순서 바꿔 스기
- 잘못된 비교 함수 작성
- 최소, 최대 예외 잘못 다루기
- 연산자 우선순위 잘못 쓰기
- 너무 느린 입출력 방식 선택
- 변수 초기화 문제



### 3.4 디버깅과 테스팅

- 작은 입력에 대해 제대로 실행되나 확인하기
- 단정문(assertion)을 쓴다
- 프로그램의 계산 중간 겨로가를 출력한다



### 3.5 변수 범위의 이해

- 산술 오버플로
- 너무 큰 결과
- 너무 큰 중간 값
- 너무 큰 무한대 값
- 오버플로 피해가기
- 자료형의 프로모션



### 3.6 실수 자료형의 이해

- 실수 연산의 어려움
- 실수의 근사 값
- 실수의 이진법 표기
- 부동 소수점 표기
- 실수 비교하기
  - 비교할 실수의 크리들에 비례한 오차 한도를 정한다
  - 상대 오차를 이용한다
  - 대소 비교
  - 정확한 사칙연산
  - 코드의 수치적 안정성 파악하기
- 실수 연산 아예 하지 않기