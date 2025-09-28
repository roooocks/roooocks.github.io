---
title: "[Tailwind] @apply"
description: Directives(지시어) - 재사용 가능한 CSS 컴포넌트를 Tailwind로 만들기
date: 2025-09-28 16:31:22 +0900
categories: [프론트엔드, CSS]
tags: [프론트엔드, CSS]
---

## 출처
- 인프런의 **타입스크립트로 배우는 리액트(React.js) : 기초부터 최신 기술까지 완벽하게**
- 제미니 AI
- AI에게 질문할 기반이 된 블로그
  - [tailwindCSS @apply으로 스타일 추출할 때 주의사항](https://velog.io/@rmaomina/tailwindcss-apply-reusing-styles){: target='_blank '}


## 사용 이유
- HTML에서 매번 직접 클래스 나열하기가 복잡하고 길어질 때 사용
- CSS 파일에서 Tailwind에서 사용하는 <ins>유틸리티 클래스 조합을 **새로운 클래스로 묶어 재사용**</ins>하기 위해 사용


## 사용법
진짜 간단하다.

```css
// 이렇게 @apply를 사용해 tailwind를 작성해주고
.btns {
  @apply bg-green-500 text-white py-3 px-5 border-none cursor-pointer rounded transition-colors duration-300 hover:bg-green-600;
}
```
{: file='@apply 예제' }

```jsx
{/* 원하는 태그에 클래스 이름을 넣어주면 바로 적용된다. */}
<button className="btns">
  Click Me
</button>
```
{: file='@apply로 적용시킨 tailwind 사용법' }


## 주의사항

### 문제점
- 사용법의 예제처럼 아무것도 없이 그냥 사용하면 유틸리티 클래스와 컴포넌트 클래스의 분리가 어려워진다.
- 분리가 어려워지면 **CSS 번들 크기의 효율이 떨어지고(CSS 코드가 여러 컴포넌트에 중복 생성)** 스타일이 복잡해진다.

### 이유
```css
@layer components {
  .btn { @apply bg-blue-500 text-white; }
  .card-btn { @apply bg-blue-50 text-sm; }
}
```
{: file='문제에 대한 예제' }

문제점에서 설명한 **CSS 번들 크기의 효율 저하**에 대한 예제가 바로 위의 코드다. <br>
코드에는 <ins>bg-blue-500이라는 동일한 속성</ins>이 **두 곳에서 중복으로 포함**된다는 것을 보여주는데, 예제가 간단해 보이지만 크고 복잡하거나 여러 사람이 같이 진행하는 프로젝트라면 CSS에서 저러한 중복이 한두개가 아니게 될 수 있다. <br>
**중복**을 중점으로 이야기 하고 있는데, <ins>더 이상 사용하지 않는 CSS 코드</ins> 역시 **CSS 번들 크기의 효율 저하**에 포함된다.

### 해결책
**1번** @layer components { } 사용하기
```css
/* 아래 예제처럼 @layer components 지시어와 같이 사용하는게 권장된다. */
@layer components { .btn { @apply ... } }
```
{: file='이렇게 쓰자' }

이유는 **PrugeCSS(Tree-shaking)** 등의 최적화 과정을 통해 <ins>사용하지 않는 유틸리티 클래스는 최종 번들에서 제거</ins>가 되기 때문이다. <br>
`components` 뿐만 아니라 `utilities`도 포함된다. (base는 용도가 달라 포함 X)

**2번** 사용 규칙 정하기 <br>
블로그 내용을 정리하면서 느낀건데, 이러나 저러나 사용하면 번들 크기는 결국 커진다는 것이다... <br>
솔직히 근본적인 문제인 것 같아서 본인이 규칙을 잘 세우고 거기에 맞게 사용하는게 좋을 듯 싶다...
  1. 너무 남발하지 않기
  2. 버튼, 폼과 같은 <ins>엄청 자주 사용되는 곳</ins>이면서 <ins>완성된 디자인을 써야할 때</ins> 사용하기
  3. 만약 재사용이 가능한데 작은 단위라면 @layer utilities를 사용해 정의하고, 이걸 HTML에서 여러 유틸리티 클래스와 함께 조합해 사용하기
  4. 안써도 될 것 같다면 그냥 HTML에 때려 박자
