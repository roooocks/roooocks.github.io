---
title: React.Fragment
description: 빈 태그의 이름?
date: 2025-09-29 16:19:39 +0900
categories: [프론트엔드, React]
---

## 출처
- 인프런의 **타입스크립트로 배우는 리액트(React.js) : 기초부터 최신 기술까지 완벽하게**
- 제미니 AI


## 설명
진짜 간단한 내용인데 말로 설명하라하면 어버버 거릴것같아서 적어놓는다.
```jsx
export default function App () {
  return (
    <>
      <h1>App Component</h1>
    </>
  );
}
```
{: file='React.Fragment 사용법' }

- 위 코드의 내용을 보면 `<>...</>`처럼 이름이 없는 태그가 있다.
- JSX는 <ins>루트 요소가 2개 이상</ins>이 되면 <ins>에러</ins>가 걸려서 여러 내용을 하나의 루트 요소(div, header, footer 등)로 묶어야 한다.
- 만약 루트 요소를 화면에 렌더링하고 싶지 않다면, 그럴 때 사용하는 기능이 저 이름 없는 태그다. 실제로는 Fragment 컴포넌트라 부른다.
- 위 코드의 방식은 작성 형태를 편하게 한 방식이며 실제로는 React.Fragment(Fragment)로 태그를 사용한다.
- 실제로 써보면 화면에 결과는 나오지만 루트 요소가 렌더링이 안된걸 확인할 수 있다.