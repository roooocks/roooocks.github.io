---
title: "[Hook] useCallback"
description: useMemo가 값이면 이건 함수다!
date: 2025-08-13 10:53:00 +0900
categories: [프론트엔드, React]
tags: [프론트엔드, React, React Hook]
---

**리액트를 다루는 기술(개정판)**을 참고했습니다.

## 설명
useMemo처럼 메모제이션을 사용하는 렌더링 성능 최적화를 위한 함수다. <br>
차이점이라면 **무엇**을 메모제이션(재사용)하냐이다.
 - useMemo : 값
 - useCallback : 함수 자체
<br>

```jsx
const onChange = useCallback(e => { setNumber(e.target.value); }, []);
```
{: file='사용법' }
1. 렌더링하는 과정에서 특정 값이 바뀌었을 때만 함수를 생성
2. 만약 값이 안바뀌었다면 이전에 생성했던 함수를 사용

useMemo와 장점이 같은데, 그래서인지 단점 역시 같다. <br>
<ins>추가적인 메모리를 사용 + 의존성 배열 비교로 인한 오버헤드 발생</ins>으로 인해 너무 많이 쓰면 **성능 저하가 발생**한다. <br>
때문에 컴포넌트가 리렌더링될 때마다 <ins>복잡하거나 시간이 많이 소요되는 함수 생성을 반복되는 것을 막을때만 사용</ins>하는 것이 좋다.


## 예제 코드
```jsx
import React, { useState, useMemo, useCallback } from 'react';

const getAverage = (numbers) => {
  console.log('평균값 계산 중..');

  if (numbers.length === 0)
    return 0;

  const sum = numbers.reduce((a, b) => a + b);
  return sum / numbers.length;
};

const Average = () => {
  // variable
  const [list, setList] = useState([]);
  const [number, setNumber] = useState('');

  // callback
  // 1. 배열이 빈 경우 : 컴포넌트가 처음 렌더링될 때만 함수 생성
  const onChange = useCallback(e => {
    setNumber(e.target.value);
  }, []);

  // 2. 배열이 찬 경우 : 배열의 값들 중 하나라도 바뀌었을 때만 함수 생성
  const onInsert = useCallback(e => {
    const nextList = list.concat(parseInt(number));
    setList(nextList);
    setNumber('');
  }, [list, number]);

  // memo
  const avg = useMemo(() => getAverage(list), [list]);

  return (
    <div>
      <input type="number" value={number} onChange={onChange} />
      <button onClick={onInsert}>등록</button>
      <ul>
        {
          list.map((value, index) => (
            <li key={index}>{value}</li>
          ))
        }
      </ul>
      <div>
        <b>평균값:</b> {avg}
      </div>
    </div>
  );
};

export default Average;
```
{: file='useCallback 예제' }