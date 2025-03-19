+++
title = "[내일배움캠프] 미니 프로젝트 3"
date = "2025-02-20T21:00:00+09:00"
draft = false
topic = ["camp"]
tag = ["내일배움캠프", "TIL"]
+++

>✅ 코드 병합  
✅ 기능 수정

---

## 미니 프로젝트

### 기능 수정

* **기존 기능 문제**: 수정 박스 생성 후 수정 아이콘 클릭 시 수정 박스 중복 생성
* **문제 해결**: 수정 아이콘 클릭 시 수정 박스가 존재하는지 체크 후 있으면 수정 취소, 없으면 수정 박스 생성으로 변경

![](https://velog.velcdn.com/images/ezro/post/481f5dcb-a15a-43b4-8d65-f6fc6f2247c1/image.gif)

![](https://velog.velcdn.com/images/ezro/post/d456bd35-4c03-4d82-aebd-05ac3f3858cb/image.png)

```js
let list = $(this).closest("li");
```
현재 이벤트가 발생한 요소에서 가장 가까운 `li` 찾기 == **수정하려는 방명록**

```js
let updateLi = list.next('li');
```
`list`의 다음 `li` 요소를 찾아 변수에 저장 == **수정 박스**

```js
let checkUpdateBox = list.next('li').find('#guestbook-update-div').length;
```
`id="guestbook-update-div"` 개수 찾기 == **수정 박스 존재 여부 확인**