---
title: Throttle, Debounce
description: 이벤트 핸들러가 많아 최적화를 해야한다면?
date: 2025-10-02 16:42:00 +0900
categories: [언어, JS/TS]
tags: [기술정리, 비교, 구현]
---

## 출처
- 블로그
  - [디바운스(Debounce)와 스로틀(Throttle) 그리고 차이점](https://webclub.tistory.com/607){: target='_blank' }
  - [쓰로틀링과 디바운싱](https://www.zerocho.com/category/JavaScript/post/59a8e9cb15ac0000182794fa){: target='_blank' }
- 예제
  - 제미니 AI


## Throttle
이벤트가 호출된 후 일정 시간이 지나기 전까지 재호출이 되지 않도록 막는 것 <br>
ex) 스크롤, 버튼 등
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Throttle 예제: 윈도우 스크롤</title>
    <style>
        body { font-family: Arial, sans-serif; padding: 20px; height: 2000px; /* 스크롤을 만들기 위해 높이 증가 */ }
        .header { position: fixed; top: 0; left: 0; right: 0; background: lightblue; padding: 10px; }
    </style>
</head>
<body>
    <div class="header">Throttle 예제: 스크롤 이벤트 (아래로 스크롤하세요)</div>
    <p style="margin-top: 50px;">페이지를 스크롤해보세요. 200ms 간격으로만 Console(콘솔)에 로그가 나타납니다.</p>
    <script src="throttle_scroll.js"></script>
</body>
</html>
```
{: file='Throttle 스크롤 예제 - HTML&CSS' }

```js
// Throttle 함수 정의
function throttle(func, delay) {
    let timeout = null;
    return function(...args) {
        // 이전에 실행 예약된 것이 없으면 (즉, delay 시간만큼 기다렸다면)
        if (!timeout) {
            func.apply(this, args); // 함수를 즉시 실행합니다.
            // 그리고 delay 시간 후 timeout을 해제하는 타이머를 설정합니다.
            timeout = setTimeout(() => {
                timeout = null;
            }, delay);
        }
    };
}

// 실제로 실행될 함수 (스크롤 위치 기반 액션)
function checkScrollPosition() {
    console.log(`[Throttle] 🔵 스크롤 위치 확인: ${window.scrollY}`);
}

// 200ms 스로틀이 적용된 함수 생성
const throttledScroll = throttle(checkScrollPosition, 200);

// scroll 이벤트에 스로틀 함수 연결
window.addEventListener('scroll', throttledScroll);
```
{: file='Throttle 스크롤 예제 - JS' }


## Debounce
같은 이름의 이벤트가 여러번 호출되어도 마지막(혹은 처음) 함수만 호출되도록 하는 것 <br>
ex) 검색어 추천 등
```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>Debounce 예제: 검색 입력</title>
    <style>
        body { font-family: Arial, sans-serif; padding: 20px; }
        #search-input { padding: 10px; width: 300px; }
    </style>
</head>
<body>
    <h1>Debounce 예제: 검색 입력</h1>
    <p>텍스트를 빠르게 입력해보세요. 입력이 멈춘 후 500ms가 지나야만 아래 Console(콘솔)에 'API 호출' 로그가 나타납니다.</p>
    <input type="text" id="search-input" placeholder="여기에 입력하세요">
    <script src="debounce_search.js"></script>
</body>
</html>
```
{: file='Debounce 검색 입력 예제 - HTML&CSS' }

```js
// Debounce 함수 정의
function debounce(func, delay) {
    let timer;
    return function(...args) {
        // 이전에 예약된 함수 실행을 취소합니다.
        clearTimeout(timer); 
        // 새로운 함수 실행을 예약합니다.
        timer = setTimeout(() => {
            func.apply(this, args);
        }, delay);
    };
}

// 실제로 실행될 함수 (API 호출 등)
function fetchData(query) {
    console.log(`[Debounce] 🟢 API 호출: ${query}`);
}

const searchInput = document.getElementById('search-input');

// 500ms 디바운스가 적용된 함수 생성
const debouncedFetch = debounce((event) => {
    fetchData(event.target.value);
}, 500);

// input 이벤트에 디바운스 함수 연결
searchInput.addEventListener('input', debouncedFetch);
```
{: file='Debounce 검색 입력 예제 - JS' }