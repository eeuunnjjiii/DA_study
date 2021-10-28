# 3주차 배울 목록
## 서브쿼리(SubQuery)
- 메인쿼리 외 또 하나의 SELECT 구문이 들어간 쿼리
- `SELECT` 절의 서브쿼리 : 스칼라 서브쿼리, 하나의 레코드&하나의 컬럼만 반환 가능
- `FROM` 절의 서브쿼리 : 인라인 뷰, 가상 테이블
- `WHERE` 절의 서브쿼리 : 일반 서브쿼리

```SQL
SELECT column_name(s) --MainQuery
FROM table_name
WHERE column =
  (SELECT column_name --SubQuery
  FROM table_name
  WHERE column = value)
```

## 1. ANY
- 단일 열 값과 다른 값의 범위의 비교 수행
- 결과로 Boolean 값 반환
- 서브 쿼리 중 하나라도 조건을 충족하는 경우 TRUE 반환
- ANY 범위의 값 중 하나에 대해 작업이 참인 경우 조건이 참임을 의미

```SQL
SELECT column_name(s)
FROM table_name
WHERE column_name operator ANY -- operator은 비교 연산자여야 함
  (SELECT column_name
  FROM table_name
  WHERE condition);
```

## 2. ALL
- 결과로 Boolean 값 반환
- 서브 쿼리 모두 조건을 충적하는 경우 TRUE 반환
- `SELECT`,`WHERE`,`HAVING`절과 함께 사용

```SQL
-- ALL Syntax with SELECT
SELECT ALL column_name(s)
FROM table_name
WHERE condition;

-- ALL Syntax with WHERE or HAVING
SELECT column_name(s)
FROM table_name
WHERE column_name operator ALL -- operator은 비교 연산자여야 함
  (SELECT column_name
  FROM table_name
  WHERE condition);
```

## 3. EXISTS
- 하위 쿼리에서 레코드의 존재를 테스트 하는데 사용
- 하나 이사의 레코드를 반환하면 TRUE 반환
- `NOT EXISTS`는 반환된 행이 하나도 없어야 TRUE 반환

```SQL
SELECT column_name(s)
FROM table_name
WHERE EXISTS
(SELECT column_name FROM table_name WHERE condition);
```
## 4. Grouping Set
- 여러 그룹핑 쿼리를 `UNION ALL`한 것과 같은 결과 반환

```SQL
SELECT column_name(s)
FROM table_name
GROUP BY GROUPING SETS (column_names) -- (GROUP BY column1) UNION ALL (GROUP BY column2);0

GROUPING SETS ((column_names), (column_names), (column_names)) -- GROUP BY 쿼리

--빈 괄호 추가시 합계 표현 가능
SELECT column_name(s)
FROM table_name
GROUP BY GROUPING SETS ((column_names), ());
```

## 5. Roll up
- `GROUP BY` 집계 결과 반환
- 컬러수와 순서(오른쪽에서 왼쪽 수으로)에 따라 레벨별로 집계 결과 반환
- 참고 : https://thebook.io/006696/part01/ch05/03/01/
```SQL
SELECT column_name(s)
FROM table_name
GROUP BY ROLLUP(column_name(s));
-- GROUP BY column_name ROLLUP(column_name(s)); 도 사용 가능
```
## 6. Cube
- 명시한 표현식 개수에 따라 가능한 모든 조합별로 집계한 결과 반환
- 집계 총 개수는 2^컬럼수
- 참고 : https://thebook.io/006696/part01/ch05/03/02/
```SQL
SELECT column_name(s)
FROM table_name
GROUP BY CUBE(column_name(s));
-- 예를 들어 컬럼1, 컬럼2가 있다면 전체 집계, 컬럼1 집계, 컬럼2 집계, 컬럼1&컬럼2 집계 리턴
```

<img width="630" alt="스크린샷 2021-10-27 오전 8 44 06" src="https://user-images.githubusercontent.com/75970111/138976492-3d981ff7-f9b9-42e4-af97-8944fbd962e0.png">

## 7. WINDOW FUNCTION
- 행과 행 간의 관계 정의 목적
- 순위, 합계, 평균, 행 위치 등 조작 가능
- `GROUP BY`와 병행해서 사용 못함
- 참고 : https://velog.io/@yewon-july/Window-Function

```SQL
SELECT WINDOW_FUNCTION(args)
       OVER ([PARTITION BY column_name(s)]
            [ORDER BY WIDOWING절](ROWS | RANGE BETWEEN))
FROM table_name

-- PARTITION BY : 전체 집합을 기준에 의해 소그룹으로 나눔
-- ORDER BY : 어떤 항목에 대해 정렬
-- WINDOWING : 행의 기준 범위를 정함, ROWS는 물리적 결과 행의 수, RANGE는 논리적 값에 의한 범위
```
<img width="580" alt="스크린샷 2021-10-28 오후 12 19 09" src="https://user-images.githubusercontent.com/75970111/139180535-015dd03d-b70e-4edf-9e50-e0b022e6fe83.png">

## 8. PARTITION BY 
- 결과 데이터의 각 행마다 집계결과를 함께 반환
- 반면, 집계함수는 그룹별 1개의 행 반환
- 참고 : https://velog.io/@mindddi/SQL-%EB%B6%84%EC%84%9D%ED%95%A8%EC%88%98, https://gent.tistory.com/442
```SQL
SELECT ANALYTIC_FUNCTION(args) --분석함수명(입력인자)
       OVER ( --분석함수임을 나타내는 키워드
            [PARTITION BY column_name(s)] --계산 대상 그룹 지정. Group by 역할. 이 구문 없으면 전체 데이터에 적용
            [ORDER BY column_name(s)] -- 정렬
            [WINDOWING절 (ROWS | RANGE BETWEEN)] --분석함수의 계산 대상 범위 지정
            )
FROM table_name;
```
<img width="492" alt="스크린샷 2021-10-28 오후 12 10 16" src="https://user-images.githubusercontent.com/75970111/139179662-b498dfdd-a401-40a0-9f7a-5f7305312ed7.png">

### 1) RANK
- 순위 계산, 각 행마다 순위를 매겨주는 함수
- `PARTITION` 내에서 `ORDER BY`절에 명시된대로 정렬된 후의 순위를 의미
- 1부터 시작, 동일한 값은 동일한 순위, 동일한 순위의 수만큼 다음 순위는 건너뜀
```SQL
SELECT column_name(s)
RANK() OVER (ORDER BY column_name)
FROM table_name;
```

### 2) DENSE_RANK
- `RANK`와 달리 동일 순위 다음 순위는 동일 순위의 수와 상관없이 1 증가된 값 반환

```SQL
SELECT column_name(s)
DENSE_RANK() OVER (ORDER BY column_name)
FROM table_name;
```

### 3) ROW_NUMBER
- 각 partition 내 값들을 `ORDER BY` 정렬 후 순서대로 순번 부여

```SQL
SELECT column_name(s)
ROW_NUMBER() OVER (ORDER BY column1, column2)
FROM table_name;
```
### 4) RATIO_TO_REPORT
- 해당 구간에서 차지하는 비율 반환
```SQL
SELECT column_name(s)
RATIO_TO_REPORT() OVER (ORDER BY column1, column2)
FROM table_name;
```
### 5) LAG/LEAD
- LAG : 이전 행의 값 리턴
- LEAD : 다음 행의 값 리턴
- 참고 : https://gent.tistory.com/339
```SQL
LAG (expr [,offset] [,default]) OVER ([partition_by_clause] order_by_clause)
-- LEAD도 동일
-- expr : 대상 컬럼명
-- offset : 값을 가져올 행의 위치, 기본값=1, 생략가능
-- default : 값이 없을 경우 기본값, 생략가능
-- partition_by_clause : 그룹 컬럼명, 생략가능
-- order_by_clause : 정렬 컬럼명, 필수

SELECT column_name(s)
LAG(column_name) OVER (ORDER BY column1, column2)
FROM table_name;

SELECT column_name(s)
LEAD(column_name, 2, 100) OVER (ORDER BY column1, column2) -- 2번째 이후 값 표시, 없을 경우 기본값 100 표시
FROM table_name;
```

### 6) FIRST_VALUE / LAST_VALUE
- FIRST_VALUE : 윈도우 정렬값 중 첫번째 값 리턴
- LAST_VALUE : 윈도우 정렬값 중 마지막 값 리턴

## 9. AVG 함수
- 선택된 그룹 값의 평균을 반환
- NULL 값은 0으로 계산
```SQL
SELECT AVG(column_name)
FROM table_name
WHERE condition;
```

## 10. WITH
- 쿼리 구문 반복 사용 가능
- 참고 : https://royzero.tistory.com/50

```SQL
WITH tmp_table_name AS (SELECT ..) --서브쿼리문
SELECT column_names
FROM tmp_table_name;

-- 2개 이상의 임시 테이블
WITH tmp_table_name1 AS (SELECT ..), --서브쿼리문1
WITH tmp_table_name2 AS (SELECT ..) --서브쿼리문2
SELECT column_names
FROM tmp_table_name1, tmp_table_name2;
```
## 11. Transaction(트랜잭션)
- 데이터베이스 처리 작업이 모두 완료되거나 모두 취소되게 하는 작업의 단위
- `COMMIT` : 데이터베이스에 영구 반영
- `ROLLBACK` : 명령어 수행 모두 취소, 바로 전 트랜잭션 시작 시점으로 돌아감
