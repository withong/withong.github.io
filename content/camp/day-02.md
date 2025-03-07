+++
title = "[내일배움캠프] 2일차. 미니 프로젝트 1"
date = "2025-02-18T21:00:00+09:00"
draft = false
topic = ["camp"]
tag = ["내일배움캠프", "TIL"]
ShowToc = true
ShowPostNavLinks = true
+++

> **Chapter 1. 온보딩 주차**  
✅ [미니 프로젝트] 메인 페이지 (HTML/CSS)  
✅ [미니 프로젝트] 팀원 정보 입력창 (HTML/CSS)  
>
>**+**  
☑️ 팀원 등록  
☑️ 팀원 목록 불러오기  
☑️ 팀원 삭제  

---

## 미니 프로젝트
* 부트스트랩 CSS/JS 활용

```html
<!doctype html>
<html lang="en">

<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>title</title>
    <!-- 부트스트랩 CSS -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet"
        integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
  	<!-- 제이쿼리 -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
</head>

<body>
	<!-- 부트스트랩 JS -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/js/bootstrap.bundle.min.js"
        integrity="sha384-MrcW6ZMFYlzcLA8Nl+NtUVF0sA7MsXsP1UyJoMp4YLEuNSfAP+JcXn/tWtIaxVXM" crossorigin="anonymous">
    </script>
</body>

</html>
```


### 메인 페이지

![](https://velog.velcdn.com/images/ezro/post/119fc7f1-75c0-4c9e-bfc2-096e2d5cb79f/image.png)

```html
<!-- main title -->
<div id="title" class="container col-xxl-8 px-4 py-5">
    <div class="row flex-lg-row-reverse align-items-center g-5 py-3">
        <div class="col-10 col-sm-8 col-lg-6">
            <img id="titleImg" src="https://raw.githubusercontent.com/hyun5085/sparta/main/13%EC%A1%B0.jpeg" class="d-block mx-lg-auto img-fluid" alt="Bootstrap Themes" width="700" height="500" loading="lazy">
        </div>
        <div id="team" class="col-lg-6">
            <h1 class="display-5 fw-bold text-body-emphasis lh-1 mb-3">이름 정하기 어렵조</h1>
            <p class="lead">이름 정하기는 어렵조, 하지만 열심히 하조</p>
            <!-- 클릭 시 팀원 정보 입력 모달창 open -->
            <button id="addBtn" type="button" class="btn btn-secondary" data-bs-toggle="modal" data-bs-target="#createMember">등록하기</button>
        </div>
    </div>
</div>

<!-- main member cards -->
<div id="cards" class="row row-cols-1 row-cols-md-5 g-4">
    <div class="col">
        <div class="card h-100">
            <img src="https://www.pngarts.com/files/10/Default-Profile-Picture-Download-PNG-Image.png" class="card-img-top">
            <div class="card-body">
                <h5 class="card-title">홍길동</h5>
                <p class="card-text">23</p>
                <p class="card-text">남</p>
                <p class="card-text">ESFP</p>
            </div>
            <div class="card-footer">
                <small class="text-body-secondary">2025-02-18</small>
                <!-- 삭제 아이콘 -->
                <span class="delete">
                    <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-trash3-fill" viewBox="0 0 16 16">
                        <path d="M11 1.5v1h3.5a.5.5 0 0 1 0 1h-.538l-.853 10.66A2 2 0 0 1 11.115 16h-6.23a2 2 0 0 1-1.994-1.84L2.038 3.5H1.5a.5.5 0 0 1 0-1H5v-1A1.5 1.5 0 0 1 6.5 0h3A1.5 1.5 0 0 1 11 1.5m-5 0v1h4v-1a.5.5 0 0 0-.5-.5h-3a.5.5 0 0 0-.5.5M4.5 5.029l.5 8.5a.5.5 0 1 0 .998-.06l-.5-8.5a.5.5 0 1 0-.998.06m6.53-.528a.5.5 0 0 0-.528.47l-.5 8.5a.5.5 0 0 0 .998.058l.5-8.5a.5.5 0 0 0-.47-.528M8 4.5a.5.5 0 0 0-.5.5v8.5a.5.5 0 0 0 1 0V5a.5.5 0 0 0-.5-.5" />
                    </svg>
                </span>
            </div>
        </div>
    </div>
</div>
```


### 팀원 정보 입력창

![](https://velog.velcdn.com/images/ezro/post/2cafc799-8794-4886-8541-82ff2d56169d/image.png)

```html
<!-- Modal -->
<div class="modal fade" id="createMember" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="exampleModalLabel"></h5>
                <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
            </div>

            <div class="modal-body">
                <div class="form-floating">
                    <textarea class="form-control" placeholder="Leave a comment here" id="modalImg"></textarea>
                    <label for="floatingTextarea">이미지</label>
                </div>

                <div class="form-floating">
                    <textarea class="form-control" placeholder="Leave a comment here" id="modalName"></textarea>
                    <label for="floatingTextarea">이름</label>
                </div>

                <div class="row g-2">
                    <div class="col-md">
                        <div class="form-floating">
                            <select class="form-select" id="modalGender" aria-label="Floating label select example">
                                <option selected>선택</option>
                                <option value="1">남자</option>
                                <option value="2">여자</option>
                            </select>
                            <label for="floatingSelect">성별</label>
                        </div>
                    </div>
                    <div class="col-md">
                        <div class="form-floating">
                            <textarea class="form-control" placeholder="Leave a comment here"
                                id="modalAge"></textarea>
                            <label for="floatingTextarea">나이</label>
                        </div>
                    </div>
                </div>
              
                <div class="form-floating">
                    <textarea class="form-control" placeholder="Leave a comment here" id="modalMbti"></textarea>
                    <label for="floatingTextarea">MBTI</label>
                </div>

                <div class="form-floating">
                    <textarea class="form-control" placeholder="Leave a comment here" id="modalHobby"></textarea>
                    <label for="floatingTextarea">취미</label>
                </div>

                <div class="form-floating">
                    <textarea class="form-control" placeholder="Leave a comment here" id="modalGit"></textarea>
                    <label for="floatingTextarea">git</label>
                </div>

                <div class="form-floating">
                    <textarea class="form-control" placeholder="Leave a comment here" id="modalBlog"></textarea>
                    <label for="floatingTextarea">blog</label>
                </div>

                <div class="form-floating">
                    <textarea class="form-control" placeholder="Leave a comment here" id="modalMsg"></textarea>
                    <label for="floatingTextarea">한마디</label>
                </div>

                <div class="modal-footer">
                    <button type="button" class="btn btn-outline-success">저장하기</button>
                </div>
            </div>
        </div>
    </div>
</div>
```


### 팀원 등록

![](https://velog.velcdn.com/images/ezro/post/03c768a3-ed24-4179-868c-ad05edc8e69c/image.gif)

```js

// 마지막 인덱스를 가져오는 함수
async function getLastIndex() {
    const q = query(collection(db, "members"), orderBy("index", "desc"), limit(1));
    const querySnapshot = await getDocs(q);

    if (!querySnapshot.empty) {
        return querySnapshot.docs[0].data().index; // 가장 마지막 index 값 반환
    } else {
        return 0; // 데이터가 없으면 0 반환
    }
}

function checkInput() {
    // 입력값 가져오기
    let fields = [
        { id: "#modalImg", message: "이미지 주소를 입력하세요." },
        { id: "#modalName", message: "이름을 입력하세요." },
        { id: "#modalGender", message: "성별을 선택하세요." },
        { id: "#modalAge", message: "나이를 입력하세요." },
        { id: "#modalMbti", message: "MBTI를 입력하세요." },
        { id: "#modalHobby", message: "취미를 입력하세요." },
        { id: "#modalGit", message: "GitHub 주소를 입력하세요." },
        { id: "#modalBlog", message: "블로그 주소를 입력하세요." },
        { id: "#modalMsg", message: "한마디를 입력하세요." }
    ];

    // 첫 번째로 비어있는 필드 찾기
    for (let i = 0; i < fields.length; i++) {
        let value = $(fields[i].id).val().trim();
        if (!value || value == "선택") {
            alert(fields[i].message); // 첫 번째로 비어있는 필드의 메시지만 띄움
            $(fields[i].id).focus(); // 해당 입력 필드에 포커스
            return false;
        } else {
            return true;
        }
    }
}

// 오늘 날짜 2025-02-18 형식으로 가져오기
function getTodayDate() {
    let today = new Date();
    let year = today.getFullYear();
    let month = String(today.getMonth() + 1).padStart(2, '0'); // 월 (01~12)
    let day = String(today.getDate()).padStart(2, '0'); // 일 (01~31)

    return `${year}-${month}-${day}`;
}

$(document).ready(function() {
    // 팀원 정보 저장하기
    $("#saveBtn").on("click", async function() {
        const lastIndex = await getLastIndex(); // 마지막 인덱스 가져오기
        const newIndex = lastIndex + 1; // 새로운 인덱스 = 마지막 인덱스 + 1

        let index = newIndex;
        let image = $("#modalImg").val();
        let name = $("#modalName").val();
        let gender = $("#modalGender").val();
        let age = $("#modalAge").val();
        let mbti = $("#modalMbti").val();
        let hobby = $("#modalHobby").val();
        let git = $("#modalGit").val();
        let blog = $("#modalBlog").val();
        let message = $("#modalMsg").val();
        let date = getTodayDate();

        let doc = {
            'index': index,
            'image': image,
            'name': name,
            'gender': gender,
            'age': age,
            'mbti': mbti,
            'hobby': hobby,
            'git': git,
            'blog': blog,
            'message': message,
            'date':date
        };

        if(checkInput()) {
            await addDoc(collection(db, "members"), doc);
            alert("팀원 정보가 저장되었습니다.");
            window.location.reload();    
        }
    });
})
```



### 팀원 목록 불러오기

```js
async function loadMembers() {
    $("#cards").empty();
    let docs = await getDocs(collection(db, "members"));

    docs.forEach((doc) => {
        let row = doc.data();

        let index = row["index"];
        let image = row["image"];
        let name = row["name"];
        let gender = row["gender"];
        let age = row["age"];
        let mbti = row["mbti"];
        let date = row["date"];

        let template = $("#cardTemplate")[0];
        let temp = $(template.content.cloneNode(true));

        temp.find(".image").attr("src", image);
        temp.find(".name").text(name);
        temp.find(".gender").text(gender);
        temp.find(".age").text(age + "살");
        temp.find(".mbti").text(mbti);
        temp.find(".index").text(index);
        temp.find(".date").text(date);

        $("#cards").append(temp);

    });
}

$(document).ready(function() {
    // 팀원 정보 불러오기
    loadMembers();
})
```


### 팀원 삭제

![](https://velog.velcdn.com/images/ezro/post/ad252ca2-5989-4b66-bb2a-d3ad0dd05f95/image.gif)

```js
$(document).ready(function() {
    // 팀원 정보 삭제하기
    $(document).on("click", ".delete", async function () {
        let card = $(this).closest(".col"); // 클릭한 버튼의 카드 찾기
        let index = card.find(".index").text(); // 해당 카드의 인덱스 가져오기
        
        if (!index) {
            alert("오류: 인덱스를 찾을 수 없음");
            return;
        }

        let confirmDelete = confirm("정말 삭제하시겠습니까?");
        if (!confirmDelete) return;
    
        try {
            // Firestore에서 해당 데이터 삭제
            let docs = await getDocs(collection(db, "members"));
    
            docs.forEach(async (doc) => {
                if (doc.data().index == index) { // 인덱스가 일치하는 데이터 찾기
                    await deleteDoc(doc.ref); // Firestore에서 삭제
                    alert("삭제되었습니다.");
                    location.reload(); // 페이지 새로고침
                }
            });
        } catch (error) {
            console.error("삭제 중 오류 발생:", error);
            alert("삭제 실패하였습니다.");
        }
    });  
})

```