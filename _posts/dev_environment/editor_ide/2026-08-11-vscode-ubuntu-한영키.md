---
title: "[VSCode] Ubuntu 한/영키 상단 탭 무시하기"
description: 탭 좀 그만 띄워...
date: 2026-08-11 23:24:00 +0900
categories: [개발 환경, 에디터 / IDE]
---

## 출처
 - ChatGPT


## 원인
진짜 간단하다. 한/영키가 Ubuntu에서는 Alt_R로 인식 되는데, Snap으로 설치한 VSCode에서는 일렉트론이 이걸 인식을 제대로 못해서 발생하는 문제다.


## 해결
Snap 설치를 제거하고 *.deb 파일로 재설치하면 깔끔하게 해결된다. <br>
만약 그래도 안된다면 settings.json에 설정을 한번 건드려 줘야한다.
