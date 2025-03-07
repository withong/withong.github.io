+++
title = "[내일배움캠프] 1일차. Git"
date = "2025-02-17T21:00:00+09:00"
draft = "false"
topic = ["camp"]
tag = ["내일배움캠프", "TIL"]
ShowToc = true
+++

> **Chapter 1. 온보딩 주차**  
✅ [특강] Git  
✅ 미니 프로젝트 S.A  

---

## Git

### 1. 기본 명령어

| 명령어 | 기능 |
|--------|------|
| `git init` | 현재 폴더를 Git 저장소로 초기화 |
| `git status` | 현재 저장소의 상태 확인 (추적되지 않은 파일, 변경 사항 등) |
| `git add <파일명>` | 특정 파일을 스테이징 영역에 추가 |
| `git add .` | 변경된 모든 파일을 스테이징 영역에 추가 |
| `git commit -m "메시지"` | 스테이징된 파일을 저장소에 커밋 |
| `git log` | 커밋 내역 확인 |
| `git log --oneline` | 한 줄 요약된 커밋 내역 확인 |
| `git config --global user.name "이름"` | Git 전역 사용자 이름 설정 |
| `git config --global user.email "이메일"` | Git 전역 이메일 설정 |
| `git config --list` | 설정된 Git 사용자 정보 확인 |


### 2. 브랜치 관련 명령어

| 명령어 | 기능 |
|--------|------|
| `git branch` | 현재 존재하는 브랜치 목록 확인 |
| `git branch <브랜치명>` | 새로운 브랜치 생성 |
| `git checkout <브랜치명>` | 해당 브랜치로 이동 |
| `git checkout -b <브랜치명>` | 브랜치 생성 후 바로 이동 |
| `git merge <브랜치명>` | 현재 브랜치에 다른 브랜치를 병합 |
| `git branch -d <브랜치명>` | 특정 브랜치 삭제 |
| `git branch -D <브랜치명>`	| 브랜치 강제 삭제 |
| `git branch -M main` | 브랜치 이름 변경 (기본 브랜치를 `main`으로 변경) |



### 3. 원격 저장소(GitHub) 관련 명령어

| 명령어 | 기능 |
|--------|------|
| `git remote add origin <저장소 URL>` | 원격 저장소 추가 (GitHub와 연결) |
| `git remote -v` | 원격 저장소 목록 확인 |
| `git push origin main` | 현재 브랜치를 원격 저장소의 `main` 브랜치에 업로드 |
| `git push -u origin main` | 기본 업스트림 브랜치 설정 후 푸시 |
|`git push --force`|	강제 푸시 (주의 필요)|
| `git pull origin main` | 원격 저장소의 최신 변경 사항 가져오기 |
|`git remote remove origin`|원격 저장소 연결 해제|
| `git clone <저장소 URL>` | 원격 저장소를 로컬로 복제 |



### 4. 파일 관리 및 변경 사항 되돌리기

| 명령어 | 기능 |
|--------|------|
| `git reset --soft HEAD~1` | 마지막 커밋 취소 (파일 변경 사항 유지) |
| `git reset --hard HEAD~1` | 마지막 커밋 취소 (변경 사항도 삭제) |
| `git checkout -- <파일명>` | 특정 파일의 변경 사항 취소 |
| `git stash` | 변경 사항을 임시 저장 |
| `git stash pop` | 임시 저장한 변경 사항 적용 |
| `git revert <커밋ID>` | 특정 커밋을 취소하는 새로운 커밋 생성 |



### 5. 협업 및 충돌 해결

| 명령어 | 기능 |
|--------|------|
| `git fetch origin` | 원격 저장소의 변경 사항 가져오기 (병합 X) |
| `git pull origin main` | 원격 저장소에서 최신 코드 가져오기 (병합 O) |
| `git merge <브랜치명>` | 브랜치를 병합 |
|`git mergetool`|	충돌 해결 도구 실행|
| `git rebase <브랜치명>` | 다른 브랜치 위에 현재 브랜치를 재배치 |
| `git reset --hard origin/main`	|원격 저장소 기준으로 강제 리셋|
| `git cherry-pick <커밋ID>` | 특정 커밋만 현재 브랜치에 적용 |
| `git diff` | 변경 사항 비교 |
| `git diff --staged` | 스테이징된 변경 사항 비교 |
| `git log --graph --all --decorate --oneline` | 브랜치와 커밋 히스토리를 그래프로 확인 |



### 6. 설정 및 기타 유용한 명령어

| 명령어 | 기능 |
|--------|------|
| `git config --global core.editor "vim"` | 기본 텍스트 편집기 설정 |
| `git config --global core.autocrlf true` | 자동 줄바꿈 설정 (Windows) |
| `git gc` | Git 저장소 정리 및 최적화 |
| `git fsck` | 저장소의 무결성 검사 |
| `git blame <파일명>` | 특정 파일에서 누가 어떤 줄을 변경했는지 확인 |


### 7. 협업을 위한 기본 Workflow

#### 1) GitHub에서 프로젝트 생성 후 클론
```bash
git clone <저장소 URL>
cd 프로젝트 폴더
```

#### 2) 새로운 브랜치에서 개발 진행
```bash
git checkout -b feature-branch
```

#### 3) 변경 사항 저장
```bash
git add .
git commit -m "기능 추가"
```

#### 4) GitHub에 업로드
```bash
git push origin feature-branch
```

#### 5) 메인 브랜치와 병합
```bash
git checkout main
git merge feature-branch
```

#### 6) 최신 코드 유지
```bash
git pull origin main
```

---

## 미니 프로젝트 S.A  
📅 기간: 2025.02.17 - 2025.02.21  
📝 프로젝트: 팀 & 팀원 소개 웹 페이지  
🎨 역할: 메인 페이지 및 팀원 정보 입력창 (HTML/CSS)  