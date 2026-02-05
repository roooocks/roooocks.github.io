---
title: spread 연산자
description: 배열, 객체의 전체 혹은 일부에 해당하는 사본 만들기
date: 2024-10-21 15:53:00 +0900
categories: [언어, JS/TS]
tags: [기술정리, 문법]
---

**여러 자료**들을 참고했습니다.
  1. **[w3schools](https://www.w3schools.com/react/react_es6_spread.asp)**를 참고했습니다.
  2. **[MDN](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)**을 참고했습니다.

## 설명
- w3schools의 spread 연산자 설명을 보면 <ins>기존 배열 혹은 객체의 전체나 일부를 다른 배열이나 객체로 빠르게 복사할 수 있게 해준다</ins> 설명되어 있다.
- 설명만 들으면 사용 방식이 헷가리므로 아래 예제를 보면서 기억해두자.
<br>


## 예제

### 배열1
```js
const one = [1, 2, 3];
const two = [4, 5, 6];
const combined = [...numbersOne, ...numbersTwo];

console.log(combined); // [1, 2, 3, 4, 5, 6]
```

### 배열2
```js
const numbers = [1, 2, 3, 4, 5, 6];
const [one, two, ...rest] = numbers;

console.log(one); // 1
console.log(two); // 2
console.log(rest); // [3, 4, 5, 6]
```

### 객체
```js
const myVehicle = {
  brand: 'Ford',
  model: 'Mustang',
  color: 'red'
}

const updateMyVehicle = {
  type: 'car',
  year: 2021, 
  color: 'yellow'
}

const myUpdatedVehicleOne = {...myVehicle, ...updateMyVehicle}
console.log(myUpdatedVehicleOne); // { brand: 'Ford', model: 'Mustang', color: 'yellow', type: 'car', year: '2021' } => 색상이 빨강에서 노랑으로 교체

const myUpdatedVehicleTwo = {...myVehicle, ...updateMyVehicle, year: 1999}
console.log(myUpdateVechicleTwo); // One과 비교했을 때 색상이 빨강->노랑, 년도가 2021->1999로 변경
```