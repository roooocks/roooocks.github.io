---
title: "[Hook] useReducer"
description: useState의 확장판
date: 2025-08-12 15:17:00 +0900
categories: [프론트엔드, React]
---

**리액트를 다루는 기술(개정판)**을 참고했습니다.

## 설명
useState보다 더 다양한 컴포넌트 상황에 따라 다양한 상태를 다른 값으로 업데이트해 주고 싶을 때 사용한다. <br>
여기서 말하는 상태는 useState에서 사용하는 상태를 말한다.
```jsx
// 리듀서 함수의 기본 형태
function reducer(state, action) {
    // 상태값 이리저리 굴리고..
    return { ... }; // 여기서 상태값 반환
}

// action 파라미터 내용
//  - redux 사용하는거 아니면 type은 필수 X
//  - 객체가 아니라 문자, 숫자여도 괜찮다.
{
    type: 'INCREMENT',
    // 타입을 제외하고 상태 변경에 필요한 값 넣기
    value: 0
}

// 리듀서 함수 설정
const [state, dispatch] = useReducer(reducer, { type: 'INCREMENT', value: 0 })

// 리듀서 함수 호출
//  - dispatch('state의 내용')
//  - 액션을 발생시키는 함수로, 발생 시 관련 리듀서 함수가 호출되는 구조
dispatch({ type: 'INCREMENT' });
```
{: file='사용법' }

useState에서는 상태를 [value, setValue] 과 같이 배열 형식으로 받아 사용했다면, reducer에서는 state 파라미터가 value를 담당한다. <br>
reducer 함수는 <ins>현재 상태, 업데이트를 위해 필요한 정보</ins>를 담은 **액션(action) 값을 전달**받아 새로운 상태를 반환한다. <br>
새로운 상태를 만들 때는 **반드시** <ins>**불변성**을 지켜</ins>주어야 한다.


## 사용 목적
설명만 들으면 왜 쓰는지 솔직히 감이 잘 안온다;; <br>
사용 이유는 대표적으로 아래 3가지가 존재한다.
 - 코드 가독성/재사용성 ↑
   - 값이 아니라 상태만 넣으면 되니 컴포넌트는 상태만 신경쓰면 된다. (가독성)
   - 리듀서 함수 자체는 순수 함수라서 여러 곳에서 쓸 경우 파일 하나를 생성해 여러곳에서 공유하면 된다. (재사용성)
 - 렌더링 성능 ↑
   - 리듀서 함수는 컴포넌트 외부에 선언되기 때문에, 컴포넌트가 리렌더링될 때마다 리렌더링되지 않는다.
   - dispatch는 리듀서 함수를 호출만해서 리렌더링이 안된다.
   - 처음 안건데, 위 설명 때문인건지 dispatch는 자식 컴포넌트의 props로 전달할 때 useCallback을 쓰지 않아도 불필요한 리렌더링 방지가 가능하다.
 - 예측이 가능한 상태 관리
   - 함수가 현재 상태, action만을 사용하기 때문에 외부 요인(네트워크 요청, 현재 시간 등)의 영항을 받지 않는다.
   - 위의 이유로 상태 변화 예측과 테스트가 쉽다.

사실 리듀서 함수 개념은 store 개념을 활용하는 Context API, redux를 쓸 때 다시 나온다. 위 설명을 봐도 왜 쓰는지 모르겠다면 작동 방식이라도 잘 알아두는게 좋다.(물론 사용 방식, 목적은 차이가 존재)<br>


## 예제 코드
```jsx
import React, { useReducer } from 'react';

function reducer(state, action) {
  return {
    ...state,
    [action.name]: action.value
  };
}

const Info = () => {
  const [state, dispatch] = useReducer(reducer, { name: '', nickname: '' });
  const { name, nickname } = state;

  const onChange = e => {
    dispatch(e.target);
  };

  return (
    <div>
      <div>
        <input type="text" name='name' value={name} onChange={onChange} />
        <input type="text" name='nickname' value={nickname} onChange={onChange} />
      </div>
      <div>
        <div>
          <b>이름: </b> {name}
        </div>
        <div>
          <b>닉네임: </b> {nickname}
        </div>
      </div>
    </div>
  );
};

export default Info;
```
{: file='useReducer 예제' }
