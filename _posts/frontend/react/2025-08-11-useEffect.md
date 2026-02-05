---
title: "[Hook] useEffect"
description: 렌더링할 때마다 특정 작업을 하고 싶다면?
date: 2025-08-11 16:39:00 +0900
categories: [프론트엔드, React]
tags: [개념정리, 기술정리, React Hook]
---

**리액트를 다루는 기술(개정판)**을 참고했습니다.

## 설명
```jsx
// 1. 컴포넌트가 렌더링될 때마다 "특정 작업" 수행
useEffect(() => {
  console.log('렌더링이 완료되었습니다!');
  console.log({
    name,
    nickname
  });
});

// 2. 컴포넌트가 화면에 맨 처음 렌더링될 때만 "특정 작업" 수행(업데이트될 때는 X)
//  - 두 번째 파라미터를 비어 있는 배열로 두기
useEffect(() => {
  console.log('마운트될 때만 실행됩니다.');
}, []);

// 3. 오히려 특정 값이 변경될 때만 "특정 작업" 수행
//  - 두 번째 파라미터를 특정 값들이 들어간 배열로 두기
useEffect(() => {
  console.log(name);
}, [name]);

// 4. 컴포넌트가 언마운트 or 업데이트되기 직전에 "특정 작업" 수행(cleanup[뒷정리])
//  - 첫번째 파라미터 내용에 return () => { ... }; 넣기
useEffect(() => {
  console.log('effect');
  console.log(name);

  return () => {
    console.log('OUT: cleanup');
    console.log(`이름: ${name}`);
  }
}, [name]);
```
{: file='사용법' }


## 예제 코드
```jsx
import React, { useState, useEffect } from 'react';

const Info = () => {
  const [name, setName] = useState('');
  const [nickname, setNickName] = useState('');
  useEffect(() => {
    console.log('IN: Effect');
    console.log({
      name,
      nickname
    });

    return () => {
      console.log('OUT: cleanup');
      console.log(`이름: ${name}`);
    }
  }, [name]);

  const onChangeName = e => {
    setName(e.target.value);
  };

  const onChangeNickName = (e) => {
    setNickName(e.target.value);
  };

  return (
    <div>
      <div>
        <input type="text" value={name} onChange={onChangeName} />
        <input type="text" value={nickname} onChange={onChangeNickName} />
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
{: file='useEffect 예제' }
