+++
title = "[사전캠프] 4일차."
date = "2025-01-16T23:41:13+09:00"
draft = false
topic = ["records"]
tag = ["사전캠프", "SQL"]
+++

> **TASKS**  
✅ SQL 강의 2주차  
✅ Web 강의 1주차  
🟩 Java Part01-04  

---

## SQL


### 1. 집계 함수
* `SUM` : 특정 열의 값들을 모두 더함.
* `AVG` : 특정 열의 값들의 평균을 계산함.
* `COUNT` : 레코드(행)의 개수를 셈.
	- `count(distinct 컬럼명)` : 중복을 제외한 고유한 값들의 개수 반환
* `MIN` : 특정 열에서 가장 작은 값을 반환함.
* `MAX` : 특정 열에서 가장 큰 값을 반환함.


#### 1-1. 주문 금액이 30,000원 이상인 주문 건의 개수 구하기
    1. 어떤 컬럼을 이용할 것인가 → select order_id, price
	2. 어떤 테이블에서 데이터를 뽑을 것인가 → from food_orders 
    3. 어떤 조건을 지정해야 하는가 → where price >= 30000 
    4. 어떤 함수 (수식) 을 이용해야 하는가 → count(order_id) 혹은 count(1)

```sql
select count(order_id) count_of_orders
from food_orders
where price>=30000;
```


#### 1-2. 한국 음식의 주문당 평균 음식 가격 구하기
	1. 어떤 컬럼을 이용할 것인가 → select cuisine_type, price
    2. 어떤 테이블에서 데이터를 뽑을 것인가 → from food_orders
    3. 어떤 조건을 지정해야 하는가 → where cuisint_type=’Korean’
    4. 어떤 함수 (수식)을 이용해야 하는가 → avg(price)

```sql
select avg(price) as average_price
from food_orders
where cuisine_type='Korean';
```


### 2. GROUP BY절
* 데이터를 그룹화하여 집계 함수와 함께 사용.


#### 2-1. 음식점별 주문 금액 최댓값 조회하기
	1. 어떤 컬럼을 이용할 것인가 → select restaurant_name, price
    2. 어떤 테이블에서 데이터를 뽑을 것인가 → from food_orders
    3. 어떤 조건을 지정해야 하는가 → X
    4. 어떤 함수 (수식) 을 이용해야 하는가 → max(price)
    5. 어떤 컬럼 기준으로 계산할 것인가 → group by restaurant_name

```sql
select restaurant_name,
       max(price)
from food_orders
group by restaurant_name;
```


#### 2-2. 결제 타입별 가장 최근 결제일 조회하기
    1. 어떤 컬럼을 이용할 것인가 → select pay_type, date
    2. 어떤 테이블에서 데이터를 뽑을 것인가 → from payments
    3. 어떤 조건을 지정해야 하는가 → X
    4. 어떤 함수 (수식) 을 이용해야 하는가 → max(date)
    5. 어떤 컬럼 기준으로 계산할 것인가 → group by pay_type

```sql
select pay_type "결제타입",
       max(date) "최근 결제일"
from payments
group by pay_type;
```


### 3. ORDER BY절
* 지정한 컬럼을 기준으로 오름차순(ASC) 또는 내림차순(DESC)으로 정렬

```sql
ORDER BY 컬럼명				-- 오름차순
ORDER BY 컬럼명 DESC		-- 내림차순
ORDER BY 컬럼명1, 컬럼명2	-- 컬럼1 기준으로 오름차순 정렬 후, 
							-- 그 결과값 내에서 컬럼2 기준으로 오름차순 정렬
```


#### 3-1. 음식점별 주문 금액 최댓값 조회하기 - 최댓값 기준 내림차순 정렬
    1. 어떤 컬럼을 이용할 것인가 → select restaurant_name, price
    2. 어떤 테이블에서 데이터를 뽑을 것인가 → from food_orders
    3. 어떤 조건을 지정해야 하는가 → X
    4. 어떤 함수 (수식) 을 이용해야 하는가 → max(price)
    5. 어떤 컬럼 기준으로 계산할 것인가 → group by restaurant_name
    6. 어떤 컬럼 기준으로 정렬할 것인가 → order by desc

```sql
select restaurant_name,
       max(price)
from food_orders
group by restaurant_name
order by max(price) desc;
```


### 4. SQL 기본 구조

```sql
SELECT		-- (필수) 반환할 데이터를 선택
FROM		-- (필수) 데이터를 가져올 테이블 지정
WHERE		-- (선택) 특정 조건으로 데이터를 필터링
GROUP BY	-- (선택) 데이터를 그룹화
ORDER BY	-- (선택) 결과를 정렬
```

---

## Web


### HTML/CSS

* HTML 기초 Tag과 CSS 활용해서 간단한 로그인 페이지 만들기
![](https://velog.velcdn.com/images/ezro/post/fd8f47f4-c6b2-450a-a814-8e6c8092fc0b/image.png)


* 부트스트랩 활용해서 나만의 추억 앨범 만들기
![](https://velog.velcdn.com/images/ezro/post/bdeb249f-2067-4e12-a4b9-a07d86769c36/image.png)

![](https://velog.velcdn.com/images/ezro/post/e1ba22ba-0291-4ed7-8af2-f9f0ace782b7/image.png)


* 한 세트로 자주 쓰이는 CSS

```css
	// 이미지를 중앙에 위치시키고 비율 유지하며 가득차도록 설정
	background-image: url('');
	background-position: center;
	background-size: cover;
```

```css
	// 플렉스 박스를 중심으로 컨테이너 안의 자식 요소들을 중앙에 정렬
	display: flex;
	flex-direction: column; <!-- 또는 row -->
	align-items: center;
	justify-content: center;
```