---
title: Linux Ubuntu 에서 git + github 연동
description: 윈도우는 git bash만 쓰면 됫는데...
date: 2026-02-13 11:58:03 +0900
categories: [개발 환경, Git & Github]
---

## 출처
- [공식 레퍼런스](https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Git-%EC%B5%9C%EC%B4%88-%EC%84%A4%EC%A0%95)
- [Coding_Beginner_JH - 우분투에서 Git 설정 및 GitHub 연동 방법: 초보자 가이드](https://cbjh-4.tistory.com/216)


## 순서
1. git 설치 (sudo apt install git, git --version)
2. 사용자 정보 설정
	- git config --global user.name "name"
	- git config --global user.email name@example.com
	- global 빼면 local 적용
3. git config --global --list로 설정 확인
	- 일일이 확인할거면 `git config user.name` 느낌으로 검색
4. ssh 설정
	- 키 생성
		- `ssh-keygen -t rsa -b 4096 -C "email@example.com" -o` : 보편화된 알고리즘
		- `ssh-keygen -t ed25519 -C "email@example.com"` : 최신 알고리즘
		- 비번 생성 가능하면 비번도 생성(Passphrase)
		- id_알고리즘이름(Private Key), id_알고리즘이름.pub(Public Key)이 생성되면 성공
	- 공개 키 내용 복사
		- `cat .ssh/id_name.pub | pbcopy` (pbcopy 안해도 됨)
		- 명령어 사용시 나오는 내용을 메일 포함해서 전부 복사한다.
	- 복사한 공개 키 Github 등록
		- 계정 프로필 > 설정 > SSH and GPG keys > New SSH key
		- Title(나는 컴퓨터 종류로 했다.), 공개 키(복붙) 입력 > Add SSH key
	- 연동 테스트
		- ssh -T git@github.com
			- Are you sure you want to continue connecting (yes/no) 시 yes
			- 성공하면 Hi name!어쩌고라고 나온다.
		- Passphrase 설정 시 입력하라는 메시지 나온다.
	- 이후에는?
		- 이제 git clone, git push 할 때 마다 비번 안적어도 된다. (최초 한번은 필요)