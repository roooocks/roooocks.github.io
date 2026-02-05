---
title: "[Tailwind] 레이어(base, components, utilities)"
description: 분명 클래스 기반 유틸리티 우선 방식이라고 했는데...?
date: 2025-09-28 16:00:16 +0900
categories: [프론트엔드, CSS]
tags: [개념정리, 비교, tailwind css]
---

## 출처
- 인프런의 **타입스크립트로 배우는 리액트(React.js) : 기초부터 최신 기술까지 완벽하게**
- 제미니 AI


## 개념
CSS의 우선순위, 구조를 관리하는데 필수적인 요소


## 우선순위
- 개념에 우선순위가 나와서 순서가 어떻게 되는지 적어두겠다.
- base < components < utilities
   
   
## 종류와 용도

### base (@tailwind base)
- 개념
  - 프로젝트의 전역적인 기본 스타일 정의
  - 노멀라이즈된 기본 HTML 요소에 대한 스타일(h1, p, a 등)이 레이어에 포함
  - Tailwind의 기본 노멀라이즈 스타일도 레이어에 포함
- 사용처
  - 프로젝트 전반에 걸쳐 적용되는 기본 스타일을 재정의 또는 추가할 때 사용
  - ex) 프로젝트 모든 곳에서 p 태그에서 사용하고 싶은 특정 폰트 스타일을 적용하기
- 사용법
```css
@layer base {
    h1 { @apply text-red-950; } /* tailwind 유틸리티 클래스 적용 */
    h2 { color: red; } /* 스타일 직접 정의 */
}
```
{: file='tailwind merge + clsx 예제' }

### components (@tailwind components)
- 개념
  - 재사용 가능한 커스텀 컴포넌트 스타일 정의
- 사용처
  - 버튼, 카드, 폼 등등 과 같이 <ins>자주 사용</ins>되는 <ins>완성된 UI 컴포넌트</ins>의 스타일을 정의할 때 사용
  - ex) .btn 클래스를 생성 후 그 안에 여러 Tailwind 유틸리티 클래스를 @apply로 적용하기
- 사용법
```css
/* @layer components로 묶어주면 일반 CSS와의 충돌(우선순위)을 방지할 수 있다. */
@layer components { .btn { @apply ... } }
```

### utilities (@tailwind utilities)
- 개념
  - Tailwind의 핵심인 **재사용 가능한 단일 목적의 유틸리티 클래스**가 여기에 속한다.
  - ex) flex, p-4, text-lg, text-red-950 같은 원자적인 스타일 속성을 가진 클래스
- 사용처
  - 원칙상(Utility First) HTML 태그에 직접 유틸리티 클래스를 적용하여 스타일을 입힐 때 사용
  - 혹은 box-shadow 속성처럼 <ins>단 하나의 속성으로 여러곳에서 일관되게 사용할 수 있는 스타일</ins>을 @layer utilities에 정의하여 새로운 유틸리티 클래스로 사용
  - 해당 레이어는 CSS에서 매우 높은 우선순위이기 때문에 <ins>다른 레이어의 스타일을 덮어쓰는게 가능</ins>
- 사용법
  - 유틸리티 클래스
    - base 사용법을 보면 "text-red-950"이라는 유틸리티 클래스를 볼 수 있다.
    - 그러한 클래스들을 그대로 사용하면 된다. (HTML의 class에 넣을수도, base 방식처럼 넣을수도 있음)
  - 사용자 커스텀
```css
@layer utilities {
      .calc-btn {
        @apply shadow-lg hover:shadow-xl transition-shadow duration-300;
      }
}
```
