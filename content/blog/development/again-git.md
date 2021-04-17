---
title: '다시 되새기는 Git'
date: 2021-03-01 16:21:13
category: 'development'
draft: false
---

\* 익숙함에 속아서 쓰는것만 쓰는 Git 을 다시정리해보자.

### Git WorkFlow

---

Git 에는 3가지의 작업 환경이 있다.<br />

1. Working directory : 우리가 프로젝트를 작업하고 수정하는 곳

2. Staging area : 어느정도 작업하다 버전히스토리에 저장할 준비가 되어있는 파일을 옮겨 놓는 곳

3. Git directory : 버전의 히스토리를 가지고 있는 곳

#### CMD

---

1. `git --version` > 깃 버전 확인

2. `git config --list` > 깃 설정 확인

3. `git config --global user.name "sangHan"` > 깃 사용자 이름 설정<br />
   `git config user.name` > 깃 사용자 이름 확인

4. `git config --global user.email "nmc2711@naver.com"` > 깃 사용자 이메일 설정<br />
   `git config user.email` > 깃 사용자 이메일 확인

5. windows : `git config --global core.autocrlf true`<br />
   mac : `git config --global core.autocrlf input`<br />
   => 깃을 통한 소스공유시 환경에 따른 줄바꿈 처리 통일을 위함

6. `git config --h` > 깃 명령어 모음 보기

7. `git init` > 깃 초기 설정 및 초기화 <br />
   `rm -rf .git` > 숨김 상태의 파일인 git 파일 제거

8. `git status` > 깃 상태 확인

9. `git add filename` > untracked 된 파일을 tracked 하게 바꿈

10. `git rm --cached filename` > tracked된 상황을 untracked 하게 바꿈

11. `echo finenameorfoldername > .gitignore` > tracked 할수없게 working directory에만 유지

12. `git diff` > working directory 변경 사항 확인
    `git diff --staged` > staged에 올라온 기점 부터 변경 사항 확인

13. `git commit -m "커밋내용"` > 버전을 만들고 stage있는 변경사항을 git repository로 옮겨준다

14. `git log` > 변경 헤쉬코드 및 메세지 확인

15. `git remote` > 현 폴더의 원격 레파지토리를 확인

16. `git push 브랜치명(ex origin main)` > 커밋된 파일들을 remote 서버에 올린다.

17. `git fetch` > origin branch 에 현재 workspace의 동기현황을 알려준다.

18. `git pull 브랜치명(ex origin main)` > 동기화된 커밋 내용을 받는다.

19. `git checkout -b 브랜치명` > 브랜치를 만들고 이동
