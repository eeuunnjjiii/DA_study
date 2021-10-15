# 1주차 배울 목록
## 1. select
- 데이터를 조회시 사용
```SQL
SELECT column1, column2, ...
FROM table_name;
```
- 전체 데이터 조회
```SQL
SELECT * FROM table_name;
```
## 2. select distinct
- 중복 값 제외 조회시 사용 (distinct values만 조회)
```SQL
SELECT DISTINCT column1, column2, ...
FROM table_name;
```
## 3. orderby
- 결과 값의 오름차순(default, `ASC`)/내림차순(`DESC`) 정렬시 사용
```SQL
SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC;

## 예시
SELECT * FROM Customers
ORDER BY Country ASC, CustomerName DESC;
```

## 4. where
- 조건에 맞는 레코드 값을 필터링
```SQL
SELECT column1, column2, ...
FROM table_name
WHERE condition;

## 예시
SELECT * FROM Customers
WHERE Country='Mexico';
```
- 연산자 정리

|연산자|설명|
|------|---|
|=|같다|
|>, >=|초과, 이상|
|<, <=|미만, 이하|
|<>, !=|같지 않다|
|BETWEEN|범위 지정|
|LIKE|패턴을 찾음|
|IN|가능한 값 여러개 지정|

## 5. in
- `WHERE`절에서 여러 값 지정, 다중 `OR` 조건의 약자
```SQL
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1, value2, ...);

SELECT column_name(s)
FROM table_name
WHERE column_name IN (SELECT STATEMENT);

## 예시
SELECT * FROM Customers
WHERE Country IN ('Germany', 'France', 'UK');

SELECT * FROM Customers
WHERE Country IN (SELECT Country FROM Suppliers);
```

## 6. between
- 특정 범위의 값을 선택시 사용, 시작 및 끝 값 포함됨
- 범위는 숫자, 문자, 날짜 사용 가능
```SQL
SELECT column_name(s)
FROM table_name
WHERE column_name BETWEEN value1 AND value2;

## 예시
SELECT * FROM Products
WHERE Price BETWEEN 10 AND 20;

SELECT * FROM Products
WHERE Price NOT BETWEEN 10 AND 20; ##10-20이 아닌 값

SELECT * FROM Products
WHERE ProductName BETWEEN 'Carnarvon Tigers' AND 'Mozzarella di Giovanni' ##
ORDER BY ProductName;
```

## 7. like
- `WHERE`절에서 특정 패턴 검색
- 자주 사용되는 두가지
1. `%` : 0,1 또는 여러문자
2. `_` : 하나의 단일문자
```SQL
SELECT column1, column2, ...
FROM table_name
WHERE columnN LIKE pattern;

## 예시
SELECT * FROM Customers
WHERE CustomerName LIKE 'a%' ##a로 시작하는 모든 고객
```
- `%`, `_` 예시

|LIKE 연산자|설명|
|------|---|
|LIKE 'a%'|a로 시작하는 모든 값|
|LIKE '%a'|a로 끝나는 모든 값|
|LIKE '%abc%'|abc가 들어간 값|
|LIKE '_r%'|두번째 자리에 r이 들어간 값|
|LIKE 'a_%'|a로 시작하고 길이가 2자 이상인 값|
|LIKE 'a__%'|a로 시작하고 길이가 3자 이상인 값|
|LIKE 'a%o'|a로 시작하고 o로 끝나는 값|
|NOT LIKE 'a%'|a로 시작하지 않는 모든 값|

## 8.  limit
- 조회할 값의 수를 지정시 사용
```SQL
SELECT column1, column2, ...
FROM table_name
LIMIT number;

## 예시
SELECT * FROM Customers
LIMIT 3;
```

## 9. fetch
- 조회할 값의 수를 지정시 사용
```SQL
SELECT column1, column2, ...
FROM table_name
FETCH NEXT number ROWS ONLY;

## 예시
SELECT * FROM Customers
FETCH NEXT 2 ROWS ONLY;
```

## 10. offset
- 원하는 행의 수만큼 건너뛴 이후 행부터 
```SQL
SELECT column1, column2, ...
FROM table_name
OFFSET number ROWS;

## 예시
SELECT * FROM Customers
OFFSET 2 ROWS; ##세번째 행부터 조회
```

## 11. isnull
- 결측치 확인시 사용
```SQL
SELECT column1, column2, ...
FROM table_name
WHERE columnN IS NULL;

SELECT column1, column2, ...
FROM table_name
WHERE columnN IS NOT NULL;
```
