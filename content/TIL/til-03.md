+++
title = "미니 프로젝트 2"
date = "2025-02-19T21:00:00+09:00"
draft = false
topic = ["TIL"]
tag = ["내일배움캠프", "과제", "팀_소개_페이지"]
+++

> **+**  
☑️ 팀원 상세 페이지 화면  
☑️ 팀원 상세 정보 조회  
☑️ 팀원 방명록 목록 조회  
☑️ 팀원 방명록 등록  
☑️ 팀원 방명록 삭제  
☑️ 팀원 방명록 수정  

---

## 미니 프로젝트


### 팀원 상세 페이지 화면

![](https://velog.velcdn.com/images/ezro/post/227dc23c-15ff-4a84-a975-280136059816/image.png)


### 팀원 상세 정보 조회

메인 페이지 `loadMembers()` 수정 - data 세팅 코드 추가
![](https://velog.velcdn.com/images/ezro/post/35e0b44d-c5d4-49da-ac77-484e6815788f/image.png)

팀원 카드 클릭 시 데이터 매칭
![](https://velog.velcdn.com/images/ezro/post/02e8f255-efec-48c9-a13b-7776d0238aef/image.png)


### 팀원 방명록 목록 조회

![](https://velog.velcdn.com/images/ezro/post/e7f5077d-6a7f-4e07-846e-60f9f5bca0fb/image.png)

![](https://velog.velcdn.com/images/ezro/post/b209f915-0ee4-4738-b741-b6af497e6c43/image.png)

오류남... firebase에서 복합 쿼리(where + orderBy)를 사용하려면 색인(index)을 생성해야 한다고 한다

![](https://velog.velcdn.com/images/ezro/post/c7f06d75-c44c-4f8f-bcce-af26e7174052/image.png)

memberIndex: 오름차순 / date: 내림차순으로 색인 생성해주니 정상 조회됨


### 팀원 방명록 등록

방명록 db에 해당 팀원의 index 추가해서 팀원 상세 페이지별 방명록 구현

![](https://velog.velcdn.com/images/ezro/post/410bbe97-6f23-4815-adcc-7b96cddc74ac/image.gif)

![](https://velog.velcdn.com/images/ezro/post/37fdab15-bb51-4443-a3f0-fa41cb097a96/image.png)

![](https://velog.velcdn.com/images/ezro/post/3f9e69f3-b7ac-4d0e-90f5-2342aaeb9e9e/image.png)


### 팀원 방명록 삭제

방명록 등록 시 입력한 비밀번호 일치해야 삭제 가능
팀원 삭제 시 해당 팀원의 방명록도 삭제

![](https://velog.velcdn.com/images/ezro/post/62095588-e942-4957-9c43-4148871abe9d/image.gif)

![](https://velog.velcdn.com/images/ezro/post/721c2614-fa62-4106-8551-eb17b30b3e16/image.png)

![](https://velog.velcdn.com/images/ezro/post/013df90d-2d96-40c2-978d-ce02f1f5c0a8/image.png)



### 팀원 방명록 수정

방명록 등록 시 입력한 비밀번호 일치하면 수정 가능

![](https://velog.velcdn.com/images/ezro/post/6746dcdb-9928-41d4-be3e-74af66b8d109/image.gif)

![](https://velog.velcdn.com/images/ezro/post/a4ff9576-f8a3-4208-b43d-05a1727ecdf1/image.png)

![](https://velog.velcdn.com/images/ezro/post/3e638e9d-e061-45e7-b05c-6877906c258a/image.png)

![](https://velog.velcdn.com/images/ezro/post/7788bc9a-bad0-442b-ac70-baf631782654/image.png)
