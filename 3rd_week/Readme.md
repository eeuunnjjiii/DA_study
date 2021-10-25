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
## 5. Roll up
## 6. Cube
## 7. 분석 함수
## 8. AVG 함수
- 선택된 그룹 값의 평균을 반환
- NULL 값은 0으로 계산
```SQL
SELECT AVG(column_name)
FROM table_name
WHERE condition;
```
## 9. Row Number, Rank, Dense_Rank
## 10. First_Value, Last_Value
## 11. Lag, Lead
## 12. 조건연산자, with문, 트랜잭션
## 13. window function
## 14. CTE
## 15. Partition by
