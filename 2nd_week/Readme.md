# 2주차 배울 목록

## 1. Join
- 두 개 이상의 테이블을 공통된 컬럼을 기준으로 결합시 사용
### 1) Inner JOIN
- 두 테이블에서 일치하는 값을 가진 레코드를 반환

```SQL
SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1.column_name = table2.column_name;

--3개 테이블 JOIN 예시
SELECT Orders.OrderID, Customers.CustomerName, Shippers.ShipperName
FROM ((Orders
INNER JOIN Customers ON Orders.CustomerID = Customers.CustomerID)
INNER JOIN Shippers ON Orders.ShipperID = Shippers.ShipperID);
```
### 2) Outer JOIN
- `LEFT (OUTER) JOIN` : 왼쪽 테이블의 모든 레코드를, 오른쪽 테이블의 일치하는 레코드 반환
- `RIGHT (OUTER) JOIN` : 오른쪽 테이블의 모든 레코드를, 왼쪽 테이블의 일치하는 레코드 반환
- `FULL (OUTER) JOIN` : 왼쪽 또는 오른쪽 테이블의 일치하는 모든 레코드 반환

![image](https://user-images.githubusercontent.com/75970111/137648006-64310929-76c2-4ea6-99ae-6d2c25792457.png)
```SQL
SELECT column_name(s)
FROM table1
LEFT JOIN table2 -- RIGHT JOIN, FULL OUTER JOIN 동일하게 사용 가능
ON table1.column_name = table2.column_name;
```
### 3) Self JOIN
- 자신과 자신을 JOIN
```SQL
SELECT column_name(s)
FROM table1 T1,table1 T2 -- 테이블 별칭 사용 필수
WHERE condition;

--예시
SELECT A.CustomerName AS CustomerName1, B.CustomerName AS CustomerName2, A.City
FROM Customers A, Customers B
WHERE A.CustomerID <> B.CustomerID
AND A.City = B.City
ORDER BY A.City;
```
### 5) Cross JOIN
- 테이블을 아무 조건 없이 연결 (곱집합,두 테이블의 곱만큼 수행됨)
```SQL
SELECT column_name(s)
FROM table1
CROSS JOIN table2;
```
### 6) Natural JOIN
- 테이블간 공통 컬럼이 하나일 때 사용해야하며, JOIN 할 컬럼명을 지정하지 않고도 간단히 표현 가능
- 컬럼명이 동일하더라도,  데이터 타입이 다르면 에러 발생
```SQL
SELECT column_name(s)
FROM table1
NATURAL JOIN table2
(NATURAL JOIN table3)..
WHERE condition;
```

## 2. Group by
- 그룹핑 결과 조회 (집계 함수 없이도 사용 가능)
- 그룹핑에 사용되는 집계 함수 : `COUNT`,`MAX`,`MIN`,`SUM`,`AVG`
```SQL
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
ORDER BY column_name(s);
```

## 3. Having
- `WHERE`절에서 사용할 수 없는 집계함수 > `HAVING`절에서 집계함수로 조건비교
- `GROUP BY`와 함께 사용됨
```SQL
SELECT column_name(s)
FROM table_name
WHERE condition
GROUP BY column_name(s)
HAVING condition
ORDER BY column_name(s);

--예시
SELECT COUNT(CustomerID), Country
FROM Customers
GROUP BY Country
HAVING COUNT(CustomerID) > 5;
```

## 4. 집합연산자와 서브쿼리
### 1) Union
- 두 개 이상 `SELECT` 문을 하나로 합칠 때 사용 > 두 개 이상 쿼리 결과를 하나의 테이블로 조회
- 중복되는 행은 하나만 반환 (중복제외)
- 단, 컬럼의 개수와 데이터타입이 같아야함
```SQL
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
```

### 2) Union All
- 중복되는 행 모두 반환
```SQL
SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;
```
### 3) Intersect
- 테이블간 공통된(교집합) 행만 반환
```SQL
SELECT column_name(s) FROM table1
INTERSECT
SELECT column_name(s) FROM table2;
```
### 4) Except (Minus)
- 테이블간 차집합 행만 반환 (첫번째 쿼리에는 있지만, 두번째 쿼리에는 없는 행 반환)
```SQL
SELECT column_name(s) FROM table1
EXCEPT
SELECT column_name(s) FROM table2;
```
