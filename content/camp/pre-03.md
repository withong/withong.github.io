+++
title = "[사전캠프] 3일차."
date = "2025-01-15T23:41:13+09:00"
draft = false
topic = ["camp"]
tag = ["사전캠프", "TIL"]
ShowToc = true
ShowPostNavLinks = true
+++

>**TASKS**  
✅ SQL 강의 1주차  
🟩 SQL 걷기반 31-50  

---

## SQL

### 1. SELECT / FROM
* 데이터를 조회할 때 사용하는 가장 기본적인 명령어.
* `SELECT`는 가져올 데이터를, `FROM`은 데이터를 가져올 테이블을 지정함.

##### `food_orders` 테이블에서 모든 데이터를 조회 :
```sql
	SELECT *
    FROM food_orders;
```

##### `customers` 테이블에서 이름(name)과 이메일(email)만 조회 :
```sql
	SELECT name, email
    FROM customers;
```


### 2. WHERE
* 조건에 맞는 데이터만 필터링할 때 사용.

##### `customers` 테이블에서 나이가 21세인 고객만 조회 :
```sql
SELECT *
FROM customers
WHERE age = 21;
```

##### `food_orders` 테이블에서 음식 종류가 한식(Korean)인 주문만 조회 :
```sql
SELECT *
FROM food_orders
WHERE cuisine_type = 'Korean';
```


### 3. 비교 연산자
* `=` : 특정 값과 같은 경우 사용
* `<>` : 특정 값과 다른 경우 사용
* `>` : 특정 값보다 큰 경우 사용
* `<` : 특정 값보다 작은 경우 사용
* `>=` : 특정 값보다 크거나 같은 경우 사용
* `<=` : 특정 값보다 작거나 같은 경우 사용

##### salary가 3000인 직원을 조회 :
```sql
SELECT * 
FROM employees 
WHERE salary = 3000;
```

##### salary가 3000이 아닌 직원을 조회 :
```sql
SELECT * 
FROM employees 
WHERE salary <> 3000;
```

##### 100보다 큰 가격의 상품을 조회 :
```sql
SELECT * 
FROM products 
WHERE price > 100;
```

##### 100보다 작은 가격의 상품을 조회 :
```sql
SELECT * 
FROM products 
WHERE price < 100;
```

##### 나이가 30세 이상인 직원을 조회 :
```sql
SELECT * 
FROM employees 
WHERE age >= 30;
```
##### 나이가 30세 이하인 직원을 조회 :
```sql
SELECT * 
FROM employees 
WHERE age <= 30;
```


### 4. 필터링 함수
* **`BETWEEN`** : 특정 범위 안에 있는 경우 사용
* **`IN`** : 여러 값 중 하나에 해당하는 경우 사용
* **`LIKE`** : 특정 패턴과 일치하는 경우 사용
	- `%` : 0개 이상의 문자를 대체함 → 부분 일치 검색
	- `_` : 단일 문자를 대체함 → 특정 위치의 문자 일치 검색

##### 주문 금액이 20,000원에서 30,000원 사이인 주문 조회 :
```sql
SELECT *
FROM food_orders
WHERE price BETWEEN 20000 AND 30000;
```

##### 나이가 21세, 25세, 30세인 고객 조회 :
```sql
SELECT *
FROM customers
WHERE age IN (21, 25, 30);
```

##### 음식 종류가 한식(Korean) 또는 일식(Japanese)인 주문 조회 :
```sql
SELECT *
FROM food_orders
WHERE cuisine_type IN ('Korean', 'Japanese');
```

##### 이름이 '김'으로 시작하는 고객 조회 :
```sql
SELECT *
FROM customers
WHERE name LIKE '김%';
```

##### 이메일 주소에 'gmail'이 포함된 고객 조회 :
```sql
SELECT *
FROM customers
WHERE email LIKE '%gmail%';
```

##### 이름이 '김'으로 시작하고 두 글자인 고객 조회 :
```sql
SELECT *
FROM customers
WHERE name LIKE '김_';
```


### * 1주차 숙제 : 상품 준비시간이 20~30분 사이인, 한국 음식점의 식당명과 고객번호 조회하기

```sql
SELECT restaurant_name as "식당명", customer_id as "고객번호"
FROM food_orders 
WHERE food_preparation_time BETWEEN 20 AND 30 
AND cuisine_type = 'Korean'
```