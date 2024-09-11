---
title: Virtual DOM
description: DOM의 업데이트가 잦으면 성능 저하가 발생하는 문제를 해결하는 방법이자 react의 특징
date: 2024-09-10 14:43:00 +0900
categories: [프론트엔드, 용어]
tags: [프론트엔드, 용어]
---

**리액트를 다루는 기술(개정판)**을 참고했습니다.

## Virtual DOM이란??
- 실제 DOM에 접근하여 조작하는 대신, 이를 <ins>추상화한 JS 객체를 구성하여 사용</ins>
- 요약하면 실제 DOM의 사본 느낌이다.
<br>


## 동작 방식
1. 데이터를 업데이트하면 전체 UI를 Virtual DOM에 리렌더링
2. 이전 Virtual DOM에 있던 내용과 현재 내용을 비교
3. 바뀐 부분만 실제 DOM에 적용

![Desktop View](https://lh3.googleusercontent.com/pw/AP1GczP_R8bwo0EA2RtjFB0OZt12RwhJ0Huwje025mTQL_ZtnlDemqj6Liq81AP9qRhu4CBrUx3daHfCU4SttG-JlvftrSDNLBqZO_ay-dUVUOqNOHCn1z8=w2400)