---
title: Styled Components
description: 컴포넌트에 옷 입히기
date: 2025-09-29 14:20:40 +0900
categories: [프론트엔드, React]
---

## 출처
- 인프런의 **타입스크립트로 배우는 리액트(React.js) : 기초부터 최신 기술까지 완벽하게**
- 제미니 AI


## 설치법
간단하다! `npm install styled-components` 를 하면 된다.


## 사용법
- 상단에 `import styled from "styled-components";` 작성
- styles를 끌어다가 쓰면 된다.


## 예제 보기
[공식 사이트](https://styled-components.com){: target='_blank' }에서 확인


## 문법

### 기초
```jsx
// 메인이 되는 컴포넌트의 외부에서 스타일을 정의해야 한다.
// 이렇게 정의한 것도 컴포넌트가 된다.
const 컴포넌트이름 = styled.[tagName]`스타일 속성`;
...
<컴포넌트이름>내용(문자나 숫자가 될 수도, 다른 JSX/TSX 컴포넌트가 될 수도 있다.)</컴포넌트이름>
```
{: file='기본 문법' }

### props
- 살짝 중요한 내용이 있다. 바로 **$**가 그 주인공이다.
- **React 컴포넌트에 전달된 props**는 <ins>HTML 속성으로 렌더링</ins>될 수 있는데, 이러면 <ins>HTML 속성과 충돌</ins>이나 <ins>존재하지 않는 속성</ins>이라는 이유로 원치 않은 상황이 발생할 수 있다.
  - ex) `<button $active> => <button active>`
- 그렇기에 **$**는 HTML 속성 충돌 방지 + 컴포넌트 props와 DOM 속성의 분리를 위해서라도 쓰는게 좋다.
- 만약 컴포넌트에 전달되지 않는다면 $를 붙이지 않아도 괜찮다.

```jsx
const Title = styled.h1<{ $color: string }>`
  color: ${(props) => props.$color};
  text-decoration: underline;
`; // props
const Wrapper = styld.section`
  padding: 2rem;
  border: 1px solid red;
`;

// 이러면 꼭 red같은 이름 말고 #ff0000같은 값을 넣어도 된다.
return(
  <>
    <Wrapper>
      <Title $color="red">Hello, World!</Title>
    </Wrapper>
  </>
);
```
{: file='props 사용법' }

### 확장
```jsx
// 이게 말이 확장이지, 실상은 기존의 스타일 컴포넌트를 재사용해서 거기에 추가로 속성을 더 넣는거다.
// 만들어진 완성품들 중 몇개는 색상을 바꾸고 싶을 때가 있을텐데 그럴 때 사용하면 괜찮을거다.
const Title = styled.h1<{ $color: string }>`
  color: ${(props) => props.$color}; 
  text-decoration: underline;
`;
const BigTitle = styled(Title)`
  font-size: 50px;
`; // 확장(props도 딸려온다.)
const Wrapper = styld.section`
  padding: 2rem;
  border: 1px solid red;
`;
const BlueBorderWrapper = styled(Wrapper)`
  border-color: blue;
`; // 확장
...
return(
  <>
    <BlueBorderWrapper>
      <Title $color="red">Hello, World!</Title>
      <BigTitle $color="blue">HELLO, WORLD!</BigTitle>
    </BlueBorderWrapper>
  </>
);
```
{: file='만들어둔 스타일 컴포넌트를 다시 한번 스타일 설정' }

### as, keyframe
```jsx
import styled, { keyframes } from "styled-components";
...
// keyframes를 쓰면 애니메이션 효과를 줄 수 있다.
const fadeIn = keyframes`from { opacity: 0; } to { opacity: 1; }`;

// as를 사용하면 기존에 쓰던 스타일 컴포넌트의 이름을 바꿔서 사용할 수 있다.
const Title = styled.h1<{ $color: string }>`
  color: ${(props) => 
            props.$color;
            animation: ${fadeIn} 2s ease-in;}; 
  text-decoration: underline;
`;
const BigTitle = styled(Title)`
  font-size: 50px;
`; // 확장(props도 딸려온다.)
const Wrapper = styld.section`
  padding: 2rem; 
  border: 1px solid red;
`;
const BlueBorderWrapper = styled(Wrapper)`
  border-color: blue;
`; // 확장
...
return(
  <>
    <BlueBorderWrapper>
      {/* 이러면 태그가 h1에서 p로 바뀐다. 참고로 태그의 효과도 같이 적용되니 주의! */}
      {/* 이런걸 어따 써먹어? 싶기도 한데, UI는 버튼으로 보여지지만 a태그 효과를 주고 싶을 때 쓰면 좋을듯? */}
      <Title $color="red" as="p">Hello, World!</Title>
      <BigTitle $color="blue">HELLO, WORLD!</BigTitle>
    </BlueBorderWrapper>
  </>
);
```
{: file='keyframe, as 예제' }

### css 헬퍼, 믹스인
```jsx
// css 헬퍼 함수는 자주 사용하는 css 코드 블럭을 별도의 변수에 담을 수 있다.
// 느낌이 오겠지만 조건에 따라 스타일이 달라지는 곳에서 유용하게 써먹을 수 있다.

import styled, { keyframes, css } from "styled-components";
...
const boxShadowMixin = css`box-shadow: 0px 10px 10px rgba(0, 0, 0, 0.5);`; // css 헬퍼
const fadeIn = keyframes`from { opacity: 0; } to { opacity: 1; }`;

const Title = styled.h1<{ $color: string }>`
  color: ${(props) => props.$color; animation: {fadeIn} 2s ease-in;}; 
  text-decoration: underline;
`;
const BigTitle = styled(Title)`
  font-size: 50px;
`;
const Wrapper = styld.section`
  padding: 2rem;
  border: 1px solid red;
`;
const BlueBorderWrapper = styled(Wrapper)<{ $shadow: boolean }>`
  border-color: blue;
  ${props => props.$shadow && boxShadowMixin}
`;
...
return(
  <>
    {/* $shadow가 있냐 없냐로 값이 적용된다. */}
    <BlueBorderWrapper $shadow>
      <Title $color="red" as="p">Hello, World!</Title>
      <BigTitle $color="blue">HELLO, WORLD!</BigTitle>
    </BlueBorderWrapper>
  </>
);
```
{: file='CSS 헬퍼와 믹스인 예제' }

### 전역
```jsx
// 전역에 일관된 스타일을 적용하기 위해 테마 기능을 제공한다.
// 그러기 위해 main이 되는 파일에 테마를 적용시킨다. (스타일 컴포넌트에 적용 가능해짐)
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import { ThemeProvider } from 'styled-components';

import App from './App.tsx'

const theme = {
  colors: {
    primary: 'red',
    secondary: 'gray',
  },
  fontSize: {
    normal: '16px',
    large: '20px',
  },
};
createRoot(document.getElementById('root')!).render(
  <StrictMode>
    {/* 이걸 써야 모든 스타일 컴포넌트에서 사용 가능 */}
    {/* 쓸 때는 ${props => props.theme.colors.primary} 방식으로 쓰면 된다. */}
    <ThemeProvider theme={theme}>
      <App />
    </ThemeProvider>
  </StrictMode>,
)
```
{: file='다크모드 지원할거면 이거 괜찮을지도?' }
