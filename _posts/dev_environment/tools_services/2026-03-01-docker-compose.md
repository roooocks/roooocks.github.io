---
title: "[Docker] docker compose란?"
description: 뭐야 쿠버네티스가 도커가 아니라고???
date: 2026-03-01 15:26:21 +0900
categories: [개발 환경, 도구 & 서비스]
---

## 출처
 - [[코드팩토리] [초급] NestJS REST API 백엔드 완전 정복 마스터 클래스 - NestJS Core](https://www.inflearn.com/course/nestjs-%EB%B0%B1%EC%97%94%EB%93%9C-%EC%99%84%EC%A0%84%EC%A0%95%EB%B3%B5-%EB%A7%88%EC%8A%A4%ED%84%B0-%ED%81%B4%EB%9E%98%EC%8A%A4-1)
 - ChatGPT랑 티키타카
<br><br>



## 정의
docker 설치 시 기본으로 딸려오는 기능이다. <br>
프로젝트를 위해 여러 컨테이너를 만들었다고 치자. (NestJS, MySQL, NginX...) 편해서 쓰던 컨테이너지만 이게 3개가 되고 4개가되고 점점 늘어날수록 관리하기 어려워질 것이다. <br>
이걸 해결하기 위해 사용하는 <ins>다중 컨테이너 관리를 위한 툴</ins> 중 하나이다.



## 여러 도구들과 Kubernetes
다중 컨테이너 관리를 위해 툴은 Kubernetes, Docker Compose, Docker Swarm 등이 있다. 이 중 Kubernetes가 가장 중요하다. (유명하다. 도커를 전혀 안써본 나도 쿠버네티스를 따로 들어봣을 정도다. 처음 들었을 땐 도커를 재친 툴인줄 알았다.) <br>
지금 당장에는 Kubernetes를 <ins>아파트 단지 전체를 관리하는 관리사무소</ins> 느낌으로만 이해해도 된다. 나중에 이걸 사용할 날이 온다면 따로 게시글을 작성해서 정리하겠다.



## Kubernetes와의 차이점
하지만 우리가 쓸 기술은 docker compose다. <br>
이 기술은 여러개의 기기, 여러개의 하드웨어에서 작동을 시킨다는 가정을 하고 만든 기술이 아니다. Kubernetes와 비슷하게 컨테이너를 묶기는 하는데, <ins>아파트 한 세대 안의 여러 방을 관리한다는 느낌</ins>이 더 쌔다(컨테이너가 몇 개던 그걸 한 번에 묶어 관리할 수 있게 해줌). <br>
때문에 로컬 환경에서 개발할 때 자주 쓰인다고 한다.



## 단점
단점이 있는데, 바로 docker compose 만의 문법이 또 따로 존재한다는 것이다. 아예 새로운건가 했는데 강의를 들어보니 yaml이라 한다. <br>
![Desktop View](https://lh3.googleusercontent.com/pw/AP1GczOyo3mWhEfKcCwaQvtAqehjjJCGPBB6xV01LzPLU-51NH0qA1dGRunlaxpz2gUWXwaWIxjimm_Amfd7x0n0ki0HvQ99MIffl5zeLjSWPWvPZv5zuI0=w2400)
_강의 내용 중 들어간 사진_

docker file 기반으로 작성하던가 docker file 없이 작성하던가 할 수 있다고 한다.