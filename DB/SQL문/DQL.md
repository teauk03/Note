# DQL (Data Query Language)
* DB에서 데이터를 조회 할 때 사용하는 SQL 구문 중에 하나
* **`SELECT`** 문법 사용

<br></br>

# SELECT
* DQL의 대표적인 문법
* 여러가지 옵션을 추가하고 조합하여 원하는 데이터를 조회(검색)할 수 있음


**SELECT 절의 형식**
```sql
SELECT 컬럼이름
FROM 테이블이름
WHERE 조건식
GROUP BY 컬럼이름
HAVING 조건식
ORDER BY 컬럼이름
LIMIT 숫자
```

<br></br>

## 1. 기본 조회 
```sql
-- 특정 컬럼만 조회 
SELECT 컬럼이름,컬럼이름 FROM 테이블이름;

-- 모든 컬럼 조회
SELECT * FROM 테이블이름;
```

<br></br>

## 2. 조건 조회 
* 기본 조회에 `WHERE` 을 붙여 조건에 맞는 데이터를 필터링 하여 조회할 수 있음
```sql
SELECT * FROM 테이블명 WHERE 조건;
```
<br></br>

### 2.1. 비교 연산자 사용 
* `=, >, <, >=, <=, !=` 연산자를 사용해 조건에 맞는 데이터를 필터링 해 조회할 수 있음
```sql
-- 나이가 30인 사용자 조회
SELECT * FROM user WHERE age = 30;

-- 나이가 20보다 큰 사용자 조회
SELECT * FROM user WHERE age > 20;

-- 나이가 50 이하인 사용자 조회
SELECT * FROM user WHERE age <= 50;

-- 이름이 'John'이 아닌 사용자 조회
SELECT * FROM user WHERE name != 'John';
```
<br></br>

### 2.2. 논리 연산자 사용
* `AND, OR, NOT` 연산자를 사용해 조건에 맞는 데이터를 필터링 해 조회할 수 있음
```sql
-- 나이가 25 이상이고, 서울에 사는 사용자 조회
SELECT * FROM user WHERE age > 25 AND city = 'Seoul';
-- 나이가 25 이상이거나 서울에 사는 사용자 조회
SELECT * FROM user WHERE age > 25 OR city = 'Seoul';
```

<br></br>

### 2.3. 범위 조건
* `BETWEEN` 연산자를 사용해 조건에 맞는 데이터를 필터링 해 조회할 수 있음
    * `BETWEEN`: 특정 값이 어떤 범위 내에 포함되는지 확인할 때 사용

```sql
-- 나이가 20 이상 30 이하인 데이터들만 조회
-- age >= 20 AND age <= 30; 와 같음
SELECT * FROM user WHERE age BETWEEN 20 AND 30;
```

<br></br>

### 2.4. 특정 값 목록 검사
* `IN`연산자를 사용해 조건에 맞는 데이터를 필터링 해 조회할 수 있음
    * `IN`: 어떤 컬럼의 값이 지정한 목록 안에 포함되는지 확인할 때 사용
    * `OR`연산자를 여러 번 나열하는 것보다 가독성 있고 간단하게 표현할 수 있음
```sql
-- user 테이블에서 city 값이 'Seoul', 'Busan', 'Incheon' 중 하나인 데이터 조회
-- city = 'Seoul' OR city = 'Busan' OR city = 'Incheon'; 와 같음
SELECT * FROM users WHERE city IN ('Seoul', 'Busan', 'Incheon');
```

<br></br>

### 2.5. 문자열 검사
* `LIKE`연산자를 사용해 조건에 맞는 데이터를 필터링 해 조회할 수 있음
    * `LIKE`: 특정 패턴과 일치하는 데이터를 찾을 때 사용(와일드 카드 %,_)

<br></br>

**`LIKE`연산자의 와일드 카드**

| 와일드카드 | 의미 | 예 | 설명 |
|-----------|------|------|------|
| `%` | 아무 글자나 개수 상관없이 포함 | `'A%'` | 'A'로 시작하는 모든 값 |
| `%` | | `'%B'` | 'B'로 끝나는 모든 값 |
| `%` | | `'%hello%'` | 'hello'를 포함하는 모든 값 |
| `_` | 한 글자만 매칭 | `'A_'` | 'A' 다음에 한 글자가 있는 값 (예: `AB`, `AC`) |
| `_` | | `'A__'` | 'A' 다음에 두 글자가 있는 값 (예: `ABC`, `ABD`) |

```sql
-- name이 'J'로 시작하는 모든 사용자 조회
SELECT * FROM use
WHERE name LIKE 'J%';
```

<br></br>

## 3. 정렬
* `ORDER BY` 키워드를 사용하여 데이터를 정렬함
* `ORDER BY`: 결과 값의 값이나 개수에 대해서 영향을 주지 않고 출력되는 순서를 조절함
    * **오름차순 정렬(기본 값):** `ASC`
    * **내림차순 정렬:** `DESC`

```sql
-- ASC(오름차순)사용시 생략 가능 
SELECT * FROM 테이블명 ORDER BY 컬럼이름 [ASC | DESC];
-- 오름차순 
SELECT * FROM user ORDER BY age ASC;
-- 내림차순
SELECT * FROM user ORDER BY age DESC;

```

<br></br>

## 4. 결과 개수 제한
* `LIMIT` 키워드를 사용하여 검색할 데이터의 개수를 지정할 수 있음
```sql
SELECT * FROM 테이블명 LIMIT 개수;
-- 상위 10개 행만 가져옴
SELECT * FROM user LIMIT 10;
-- 나이가 어리게 정렬 된 후 상위 10개 행만 가져옴
SELECT * FROM user ORDER BY age LIMIT 10; 
-- offset fcn (나이가 어리게 정렬 된 후 상위 3개의 행을 건너 뛰고 5개의 행을 가져옴)
SELECT * FROM user ORDER BY age LIMIT 3,5;
```

<br></br>

## 5. 중복 제거
* `DISTINCT` 키워드를 사용하여 중복된 값을 제거하고 고유한 값만 반환
* 컬럼 이름의 중복된 값을 제러하고 유일한 값들만 반환 시킴
```sql
SELECT DISTINCT 컬럼명 FROM 테이블명;
-- user 테이블에서 중복이 없게 city 목록 가져옴
SELECT DISTINCT city FROM user;

```

<br></br>

## 6. 그룹

### 그룹화 (GROUP BY)
* `GROUP BY` 키워드를 사용하여 같은 값을 가진 행들을 그룹화 할 수 있음
* `집계 함수(Aggregate Fnc)`와 같이 사용할 때가 많음 
 
**집계 함수**
| **함수**        | **설명**               |
|-----------------|------------------------|
| `COUNT(*)`      | 행 개수를 셈           |
| `SUM(column)`   | 합계를 구함            |
| `AVG(column)`   | 평균 값을 계산         |
| `MAX(column)`   | 최대값을 반환          |
| `MIN(column)`   | 최소값을 반환          |

```sql
SELECT 컬럼이름, 집계함수(컬럼이름) FROM 테이블이름 GROUP BY 컬럼이름;
```

### 그룹화 필터링 (HAVING)
* `GROUP BY` 된 데이터에 조건을 걸때 사용함 (GROUP BY 후에만 가능)
* `GROUP BY`의 `WHERE`이라고 생각하면 됨
* 집계 함수와 같이 사용함

**HAVING VS WHERE**
|**비교항목** | **WHERE**        | **HAVING**               |
|-----------------|-----------------|------------------------|
|**사용위치**| `GROUP BY` 전에 사용    | `GROUP BY` 전에 후에 사용           |
|**집계 함수 사용 여부**| 불가   | 가능           |
|**조건 적용 대상**| 각각의 행   | 그룹별 결과         |

```sql
-- HAVING 기본 문법
SELECT 컬럼이름, 집계함수
FROM 테이블이름
GROUP BY 컬럼이름
HAVING 조건문;

-- 도시별 사용자 수를 특정 -> 사용자 수가 2명 이상인 도시만 출력
SELECT city, COUNT(*) 
FROM user
GROUP BY city
HAVING COUNT(*) >= 2;
```