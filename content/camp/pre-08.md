+++
title = "[사전캠프] 8일차."
date = "2025-01-22T23:41:13+09:00"
draft = "false"
topic = ["camp"]
tag = ["사전캠프", "TIL"]
+++

>**TASKS**  
✅ Web 강의 5주차  
✅ 달리기반 SQL 4-5  
✅ 달리기반 Java 3  

---

## SQL


### HAVING
`GROUP BY`로 그룹화된 데이터에 조건을 적용할 때 사용
```sql
SELECT 컬럼명, 집계함수(컬럼명)
FROM 테이블명
WHERE 조건 (옵션)
GROUP BY 그룹화할_컬럼명
HAVING 조건;
```

* `WHERE`과 `HAVING`의 차이점
`WHERE` : 그룹화 이전에 조건을 적용, 집계 함수를 사용할 수 없음.
`HAVING` : 그룹화 이후에 조건을 적용, 집계 함수를 사용할 수 있음.


### Lv4. 단골 고객님 찾기


* 고객별로 주문 건수와 총 주문 금액을 조회 (주문을 한 적이 없는 고객도 결과에 포함)

```sql
select c.customername "고객명",
	   count(o.customerid) "주문 건수",
	   coalesce(sum(o.TotalAmount), 0) "총 주문 금액"
from customers c
left join orders o on c.customerid = o.customerid
group by c.customername
```

![](https://velog.velcdn.com/images/ezro/post/fd8b298f-8dd3-407a-a697-8324e5c32ff4/image.png)



* 나라별로 총 주문 금액이 가장 높은 고객과 그 고객의 총 주문 금액을 조회
```sql
select c.country "국가",
	   c.customername "고객명",
	   coalesce(sum(o.TotalAmount), 0) "총 주문 금액"
from customers c
left join orders o on c.customerid = o.customerid
group by c.country, c.customername
having coalesce(sum(o.TotalAmount), 0) = (
	-- 국가 내에서 가장 큰 총 주문 금액을 반환
	select max(sum_amount)
	from
	(
		select c2.customerid,
        	   -- 고객별 총 주문 금액 합계 계산
			   sum(o2.TotalAmount) sum_amount
		from customers c2
		left join orders o2 on c2.customerid = o2.customerid
        -- 외부 쿼리의 현재 국가와 같은 국가에 속한 고객들만 필터링
		where c2.Country = c.Country
		group by c2.customerid
	) b
)
```
![](https://velog.velcdn.com/images/ezro/post/501104a1-edd8-45b2-aca8-38d196bfc850/image.png)


### Lv4. 가장 높은 월급을 받는 직원은?


* 각 직원의 이름, 부서, 월급 그리고 그 직원이 속한 부서에서 가장 높은 월급을 받고 있는 직원의 이름, 월급 조회
```sql
select e1.name,
	   e1.department,
	   e1.salary,
	   e2.name,
	   e2.salary
from employees e1
inner join employees e2 on e1.department = e2.department
where e2.salary = (
	-- 부서별 가장 높은 월급 조회
	select max(e3.salary)
	from employees e3
	where e3.department = e1.department
)
```
![](https://velog.velcdn.com/images/ezro/post/413b2c4d-49ba-4d12-8995-bbe3dda1c780/image.png)


* 평균 월급이 가장 높은 부서와 해당 부서의 평균 월급 조회
```sql
select department,
	   avg(salary) top_avg_salary
from employees
group by department
having top_avg_salary = (
	-- 가장 높은 평균 월급 조회
	select max(avg_salary)
	from
	(
    	-- 부서별 평균 월급 조회
		select avg(salary) avg_salary
		from employees
		group by department
	) q
)
```
![](https://velog.velcdn.com/images/ezro/post/4fbacd9c-91d6-418a-a07d-6f49ff7bb71e/image.png)


### Lv5. 가장 많이 팔린 품목은?


* 각 고객이 구매한 모든 제품의 총 금액을 계산하고, 고객 이름, 총 구매 금액, 주문 수를 출력
```sql
select c.customername,
	   sum(p.price * o.quantity) total_price,
	   count(o.customerid) count_order
from customers c
inner join orders o on c.customerid = o.customerid
inner join products p on o.productid = p.productid
group by c.customername
order by total_price desc
```
![](https://velog.velcdn.com/images/ezro/post/a77d569e-75cb-4450-a1f5-7f838b443e83/image.png)



* 각 제품 카테고리별로 가장 많이 팔린 제품의 이름과 총 판매량을 조회
```sql
select p.category,
	   p.productname,
	   sum(o.quantity) total_quantity
from products p
inner join orders o on p.productid = o.productid
group by p.category, p.productname
having total_quantity = (
	select max(sum_quantity)
	from
	(
		select p2.category,
			   p2.productname,
			   sum(o2.quantity) sum_quantity
		from products p2
		inner join orders o2 on p2.productid = o2.productid
		where p2.category = p.category
		group by p2.category, p2.productname
	) sq
)
```
![](https://velog.velcdn.com/images/ezro/post/a7bd6137-f49a-412c-bd84-c0fd440070c9/image.png)



### Lv5. 예산이 가장 큰 프로젝트는?


* 부서별 가장 높은 월급을 받는 직원들의 이름, 부서, 월급 조회
```sql
select e1.name,
	   e1.department,
	   e1.salary
from employees e1
where e1.salary = (
	select max(e2.salary)
	from employees e2
	where e2.department = e1.department
)
```
![](https://velog.velcdn.com/images/ezro/post/ac98c14a-0599-4490-91a5-725e6d606fd2/image.png)



* 직원이 참여한 프로젝트 중 예산이 10,000 이상인 프로젝트 조회
```sql
select e.name,
	   p.projectname,
	   p.budget
from employees e
inner join employeeprojects ep on e.employeeid = ep.employeeid
inner join projects p on ep.projectid = p.projectid
where p.budget >= 10000
```
![](https://velog.velcdn.com/images/ezro/post/a78de669-138d-4622-97e5-e75933c7a10d/image.png)



## Java


### Lv3. 단어 맞히기 게임
![](https://velog.velcdn.com/images/ezro/post/61fbc668-d45f-4450-a9e2-652e5b3a9ef2/image.png)
```java
import java.util.Random;
import java.util.Scanner;

public class WordGame {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        Random random = new Random();
        String[] words = {
                "apple", "banana", "orange", "grape", "cherry",
                "strawberry", "kiwi", "peach", "plum", "pear",
                "watermelon", "melon", "pineapple", "mango", "apricot",
                "blueberry", "raspberry", "blackberry", "appliance", "travel",
                "music", "exercise", "software", "keyboard", "computer",
                "bicycle", "guitar", "piano", "window", "book"
        };

        boolean power = true;
        boolean playing = true;

        while (power) {
            StringBuffer wordZip = new StringBuffer();
            // 랜덤 단어 생성
            String answer = words[random.nextInt(words.length)].toUpperCase();
            // 글자 수 만큼 _ 담기
            wordZip.append("_".repeat(answer.length()));
            // 게임 기회
            int wrongCount = 9;

            while (playing) {
                if (answer.equals(wordZip.toString())) {
                    showFormatWord(wordZip);
                    System.out.println("정답입니다!");

                    power = false;
                    break;
                }

                if (wrongCount <= 0) {
                    System.out.println("기회를 모두 소진하였습니다.");
                    System.out.println("게임을 종료합니다.");
                    System.out.print("정답: " + answer);

                    power = false;
                    break;
                } else {
                    System.out.println("남은 기회: " + wrongCount + "번");
                }

                showFormatWord(wordZip);
                System.out.print("알파벳을 입력하세요: ");
                String userInput = scanner.nextLine().toUpperCase();

                if (userInput.length() == 1) {
                    char userLetter = userInput.charAt(0);

                    if (!Character.isLetter(userLetter)) {
                        System.out.println("알파벳만 입력 가능합니다.");
                        System.out.println("기회가 1 차감됩니다.");
                        wrongCount--;
                        continue;
                    }

                    if (wordZip.toString().contains(String.valueOf(userLetter))) {
                        System.out.println("이미 입력한 알파벳입니다.");
                        System.out.println("기회가 1 차감됩니다.");
                        wrongCount--;
                        continue;
                    }

                    int answerCount = 0;
                    for (int i = 0; i < answer.length(); i++) {
                        char answerLetter = answer.charAt(i);

                        if (userLetter == answerLetter) {
                            wordZip.replace(i, i + 1, Character.toString(userLetter));
                            answerCount++;
                        }
                    }

                    if (answerCount == 0) {
                        System.out.println("포함된 알파벳이 없습니다.");
                        System.out.println("기회가 1 차감됩니다.");
                        wrongCount--;
                        continue;
                    }

                } else {
                    System.out.println("한 글자만 입력하세요.");
                    System.out.println("기회가 1 차감됩니다.");
                    wrongCount--;
                    continue;
                }
            }
        }
    }

    public static void showFormatWord(StringBuffer zip) {
        String result = "";
        for (int i = 0; i < zip.length(); i++) {
            if (i == 0) {
                result += "[ ";
            }
            result += zip.substring(i, i + 1) + " ";

            if (i == zip.length() - 1) {
                result += "]";
            }
        }
        System.out.println("---------------------");
        System.out.println(result);
        System.out.println("---------------------");
    }

}
```
![](https://velog.velcdn.com/images/ezro/post/e704f265-c734-4835-b0f2-5e1732f4c5b8/image.gif)