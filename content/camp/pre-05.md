+++
title = "[사전캠프] 5일차."
date = "2025-01-17T23:41:13+09:00"
draft = "false"
topic = ["camp"]
tag = ["사전캠프", "TIL"]
ShowToc = "true"
+++

> **TASKS**  
✅ SQL 강의 3주차  
✅ Web 강의 2주차  
✅ Java 강의 Part 03.  
>
> **+**  
☑️ 퀘스트 걷기반 Java Part 3  
☑️ 퀘스트 걷기반 SQL 45번  

---

## SQL


### 1. 문자열 함수
* `REPLACE` : 문자열의 특정 부분을 다른 문자열로 대체.
```sql
	replace(바꿀 컬럼, 현재 값, 바꿀 값)
```

* `SUBSTR` 또는 `SUBSTRING` : 문자열의 일부를 추출.
```sql
	substr(조회할 컬럼, 시작 위치, 글자 수)
```

* `CONCAT` : 문자열을 연결.
```sql
	concat(붙이고 싶은 값1, 붙이고 싶은 값2, 붙이고 싶은 값3, ...)
```


#### 1-1. 서울 지역의 음식 타입별 평균 음식 주문 금액 구하기
	1. 어떤 테이블에서 데이터를 뽑을 것인가 → from food_orders 
	2. 어떤 컬럼을 이용할 것인가 → price, cuisine_type, addr
	3. 어떤 조건을 지정해야 하는가 → where addr like ‘%서울%’
	4. 어떤 함수 (수식) 을 이용해야 하는가 → avg(price), substring(addr, 1, 2)
```sql
select substring(addr, 1, 2) "시도", -- addr 첫 번째 글자에서 두 글자를 출력
       cuisine_type "음식 종류",
       avg(price) "평균 금액"
from food_orders
where addr like '%서울%'
group by 1, 2	-- 컬럼 순서로 입력 가능 
				-- 1 = substring(addr, 1, 2) "시도"
				-- 2 = cuisine_type "음식 종류"
```


#### 1-2. ‘[지역(시도)] 음식점이름 (음식종류)’ 컬럼을 만들고, 총 주문 건수 구하기
	1. 어떤 테이블에서 데이터를 뽑을 것인가 → from food_orders 
	2. 어떤 컬럼을 이용할 것인가 → addr, restaurant_name, cuisine_type, order_id
	3. 어떤 조건을 지정해야 하는가 → X
	4. 어떤 함수 (수식) 을 이용해야 하는가 → count(1), substring(addr, 1, 2), concat(’[’, 뽑은시도, ‘] ‘, restaurant_name, ‘ (’, cuisine_type, ‘)’)
```sql
select concat('[', substring(addr, 1, 2), '] ', restaurant_name, ' (', cuisine_type, ')') "바뀐이름", -- [서울] Hangawi (Korean)
       count(1) "주문건수"
from food_orders
group by 1
```


### 2. 조건문
* `IF` : 특정 조건이 참인지 여부를 확인하고 참일 경우 특정 값을 반환.
```sql
	if(조건, 조건을 충족할 때, 조건을 충족하지 못할 때)
```

* `CASE` : 다양한 조건을 정의하고 조건에 따라 값을 반환.
```sql
	case when 조건1 then 값(수식)1
	 	 when 조건2 then 값(수식)2
		 else 값(수식)3
	end
```


#### 2-1. 10세 이상, 30세 미만의 고객의 나이와 성별로 그룹 나누기
	1. 어떤 테이블에서 데이터를 뽑을 것인가 → from customers
	2. 어떤 컬럼을 이용할 것인가 → name, age, gender
	3. 어떤 조건을 지정해야 하는가 → where age between 10 and 29
	4. 어떤 함수 (수식) 을 이용해야 하는가 → case when … end
```sql
select name, 
	   age, 
       gender,
	   case when (age between 10 and 19) and gender='male' then "10대 남자"
			when (age between 10 and 19) and gender='female' then "10대 여자"
            when (age between 20 and 29) and gender='male' then "20대 남자"
            when (age between 20 and 29) and gender='female' then "20대 여자"
       end "그룹" 
from customers
where age between 10 and 29
```


#### 2-1. 지역과 배달 시간을 기반으로 배달 수수료 구하기
	1. 어떤 테이블에서 데이터를 뽑을 것인가 → from food_orders 
	2. 어떤 컬럼을 이용할 것인가 → restaurant_name, order_id, addr, derlivery_time, price
	3. 어떤 조건을 지정해야 하는가 → X
	4. 어떤 함수 (수식) 을 이용해야 하는가 → case when, if
```sql
select restaurant_name,
       order_id,
       delivery_time,
       price,
       addr,
       case when delivery_time>25 and delivery_time<=30 then price*0.05*(if(addr like '%서울%', 1.1, 1))
            when delivery_time>30 then price*1.1*(if(addr like '%서울%', 1.1, 1))
            else 0 end "수수료"
from food_orders
```


### 3주차 숙제
* 다음의 조건으로 배달 시간이 늦었는지 판단하는 값을 만들어주세요. 
	- 주중 : 25분 이상
	- 주말 : 30분 이상
```sql
select day_of_the_week,
       delivery_time,
       case when day_of_the_week='Weekday' and delivery_time>=25 then '지연'
            when day_of_the_week='Weekend' and delivery_time>=30 then '지연'
            else '정시' end "지연 여부"
from food_orders
```

---

## JavaScript


### 1. 딕셔너리 : 객체 (Object)
* 키-값 쌍(key-value pairs)으로 데이터를 저장하는 데이터 구조.
```js
	let dictionary = {
		key1: "value1",
		key2: "value2",
		key3: 123,
	};
```

* 접근
```js
	console.log(dictionary["key1"])
	console.log(dictionary["key2"])
	console.log(dictionary["key3"])
```

* 결과
```
	value1
	value2
	123
```


### 2. 리스트 : 배열 (Array)
* 순서가 있는 데이터의 리스트로, 인덱스로 요소를 접근.
```js
	let list = ["item1", "item2", 456];
```

* 접근
```js
	console.log(list[0]);
	console.log(list[1]);
	console.log(list[2]);
```

* 결과
```
	item1
	item2
	456
```


### 3. 리스트 안의 딕서녀리 : 배열과 객체의 조합
* 배열의 순서 유지: 배열의 인덱스를 통해 각 객체에 접근.  
* 객체의 키-값 접근: 배열의 요소인 객체에서 키를 사용해 값 출력.
```js
	let people = [
		{"name": "김서영", "age": 23},
		{"name": "이현아", "age": 33},
		{"name": "최영환", "age": 18},
		{"name": "빅서연", "age": 19},
		{"name": "김지용", "age": 21},
		{"name": "나예지", "age": 30}
    ]
```

* 접근
```js
	console.log(people[0]);
	console.log(people[2]["name"]);
	console.log(people[3]["age"]);
```

* 결과
```
	{name: '김서영', age: 23}
    최영환
    19
```


### 4. 배열 함수 forEach
* 배열의 각 요소에 대해 주어진 콜백 함수를 한 번씩 실행.
* 반환값은 없으며, 주로 배열 요소를 순회하며 특정 작업을 수행할 때 사용.
```js
	array.forEach(element => {});
```

* 실행
```js
	people.forEach(person => {
    	console.log(person["name"], person["age"]);
    });
```

* 결과
```js
	김서영 23
	이현아 33
	최영환 18
	빅서연 19
	김지용 21
	나예지 30
```


### 5. 조건문 if-else
* 조건에 따라 서로 다른 코드를 실행하는 데 사용.
* 특정 조건을 만족하는 경우에만 코드를 실행할 수 있음.
```js
	if (조건) {
		// 조건이 true일 때 실행되는 코드
	} else {
		// 조건이 false일 때 실행되는 코드
	}
```

* 실행
```js
	let age = 18;

	if (age < 20) {
		console.log("미성년자입니다.")
    } else {
    	console.log("성인입니다.")
    }
```

* 결과
```js
	미성년자입니다.
```


### 5. 활용하기
* 20살 미만일 경우 "[이름]님은 미성년자입니다."  
20살 이상일 경우 "[이름]님은 성인입니다." 출력
```js
	people.forEach(person => {
		if (person["age"] < 20) {
			console.log(person["name"] + "님은 미성년자입니다.")
		} else {
			console.log(person["name"] + "님은 성인입니다.")
		}
	});

```

* 결과
```
	김서영님은 성인입니다.
	이현아님은 성인입니다.
	최영환님은 미성년자입니다.
	빅서연님은 미성년자입니다.
	김지용님은 성인입니다.
	나예지님은 성인입니다.
```


### 6. function
* 특정 작업을 수행하는 코드를 정의하고 필요할 때 호출해서 사용.
```js
	function hello() {			// hello 라는 이름의 함수를 생성
    	alert("안녕하세요!");	// 안녕하세요! 를 출력하는 알림창
    }
```

* 호출
```html
	<!-- 인사하기 버튼 클릭 시 hello() 함수 호출 -->
	<button onclick="hello()">인사하기</button> 
```

* 결과
![](https://velog.velcdn.com/images/ezro/post/7e15e1f1-596a-4e12-a4e2-8ed1dfb6e801/image.png)

---

## jQuery
* JavaScript 코드를 간단하게 작성하도록 돕는 경량 JavaScript 라이브러리
```html
  <div id="htmlId">Hello, ID!</div>
  <div class="htmlClass">Hello, Class!</div>
```

* JavaScript로 호출
```js
	let getId = document.getElementById("htmlId");
	let getId2 = document.querySelector("#htmlId");

	let getClass = document.getElementsByClassName("htmlClass");
	let getClass2 = document.querySelectorAll(".htmlClass");
```

* jQuery로 호출
```js
	let getId = $("#htmlId");
	let getClass = $(".htmlClass");
```

---

## Java


### 반복문 연습하기 Part 1
1부터 100까지의 숫자 출력하기.
`for` 또는 `while` 반복문을 사용하여 1부터 100까지의 숫자를 출력하세요.


* `for`문
```java
	for (int i = 1; i <= 100; i++) {
		System.out.println("i = " + i);
	}
```

* `while`문
```java
	int i = 1;

	while (i <= 100) {
		System.out.println("i = " + i);
		i++;
	}

```


### 반복문 연습하기 Part 2
1부터 100까지의 짝수만 출력하기.
반복문을 사용하여 1부터 100까지의 숫자 중 짝수만 출력하세요.


* `for`문
```java
	for (int i = 1; i <= 100; i++) {
		if (i % 2 == 0) {
		System.out.println("i = " + i);
	}
```

* `while`문
```java
	int i = 1;
    
	while(i <= 100) {
		if (i % 2 == 0) {
			System.out.println("i = " + i);
		}
		i++;
	}
```


### 반복문 연습하기 Part 3
구구단 출력하기.
2단부터 9단까지의 구구단을 출력하세요.
```java
	for (int i = 2; i <= 9; i++) {
		System.out.println("--- " + i + "단 ---");
		for(int j = 1; j <= 9; j++) {
			System.out.println(i + " * " + j + " = " + i * j);
		}
	}
```