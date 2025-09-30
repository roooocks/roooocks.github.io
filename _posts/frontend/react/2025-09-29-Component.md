---
title: Component
description: 블록들을 만들어보자!
date: 2025-09-29 15:42:48 +0900
categories: [프론트엔드, React]
tags: [프론트엔드, React]
---

## 출처
- 인프런의 **타입스크립트로 배우는 리액트(React.js) : 기초부터 최신 기술까지 완벽하게**
- 제미니 AI


## 정의
```jsx
export default function App () {
  return (
    <>
      <h1>App Component</h1>
    </>
  );
}
```
{: file='App 컴포넌트 기본 코드' }
- 위와 같은 형태의 코드가 작성된 파일을 React JS에서는 **컴포넌트**라고 한다.
  - 예를 들어 App.tsx가 있다면 이를 App 컴포넌트라고 부른다.
- UI를 구성하는 독립적이고 <ins>재사용 가능한 작은 단위</ins>를 의미한다.
- 리액트는 이런 컴포넌트들을 가져와 결합하여 하나의 완성된 화면을 만들어가는 방식으로 동작한다.


## 종류
- 옛날에는 클래스형/함수형 컴포넌트 2가지로 나뉘어져있었다는데, 지금은 함수형만 쓴다고 한다. (옛날 책으로 공부했을 때 클래스형에 있는 라이프 사이클이 헷갈렸는데 매우매우 좋은 부분)
- 다만 함수형은 또 4가지 방식으로 표현이 가능하다.

```jsx
// 함수 선언문
export default function App () {
  return (
    <>
      <h1>App Component</h1>
    </>
  );
}

// 함수 표현식
const App = function App() {
  return ( ... );
};

export default App;

// 함수 표현식 + 화살표 함수
const App = () => {
  return ( ... );
};

export default App;

// 함수 표현식 + 화살표 함수 + 리턴 삭제
const App = () => (<> ... </>);

export default App;
```
{: file='함수형의 종류' }
본인 취향 혹은 회사에서 자주 사용하는걸 채택하면 된다.


## 가능한 조건
함수형 기준으로 설명했을 때 아래와 같다.
  1. null 반환 (undefined는 안된다고 한다)
  2. JSX 요소 반환 (빈 내용을 반환하는건 안된다)


## 예시
사실 예시라하기도 에매한데, 그냥 이런 느낌으로 컴포넌트를 사용하는구나! 라고 생각하자
- ex) App 컴포넌트
  - 헤더 컴포넌트
  - Home 컴포넌트
    - nav (사이드바) 컴포넌트
    - article 컴포넌트
  - Footer 컴포넌트
