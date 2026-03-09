---
title: "[busboy] busboy에서 Promise를 사용하는 이유"
description: busboy는 이벤트 기반이라고...
date: 2026-03-08 15:52:47 +0900
categories: [백엔드, 라이브러리 / 패키지]
---

## 경고
예제의 코드는 **이렇게 쓴다를 보여주는 것**이지 **실제 사용은 안되는 것**을 알아두자.
<br><br>



## 출처
- 짤막하게 작성한 설명을 GPT가 요약 한 것을 적절히 수정
<br><br>



## async/await 사용이 안되는 이유
busboy는 **이벤트 기반(EventEmitter)** 구조로 동작한다. 즉, 업로드 과정에서 이벤트가 발생하면 해당 시점에 콜백이 실행되는 방식이다.

```ts
bb.on('file', (name, file, info) => { ... });
bb.on('finish', () => { ... });
bb.on('error', (err) => { ... });
```
{: file='busboy의 대표적 이벤트' }

다만 busboy는 Promise를 반환하지 않는다. 정확히는 <ins>busboy는 이벤트 기반 API만 제공하며, Promise 인터페이스는 제공하지 않는다.</ins> 따라서 다음과 같은 코드는 사용할 수 없다.

```ts
await bb
```

정리하면 busboy는 비동기 처리를 하지만 결과를 반환하는 Promise 기반이 아니라 이벤트를 구독하여 처리하는 **이벤트 기반 API**다. <br>
그래서 업로드 과정에서 발생하는 이벤트를 구독하여 해당 시점에 로직을 처리하는 방식으로 동작한다.
<br><br>



## Promise 사용 이유

### 업로드 완료 시점 대기
busboy를 사용할 때 <ins>Promise를 사용하는 가장 큰 이유</ins>는 **async/await 방식으로 업로드 완료 시점까지 기다리기 위해**서다. <br>
이벤트 기반 API는 Promise를 반환하지 않기 때문에 async/await로 바로 사용할 수 없다. 따라서 이벤트가 끝나는 시점을 기준으로 Promise를 직접 만들어 감싸는 방식을 사용한다.

```
busboy 이벤트 발생
      ↓
Promise 내부에서 이벤트 처리
      ↓
업로드 완료 시 resolve
      ↓
async/await로 기다리기 가능
```
{: file='Promise를 사용한 async/await 방식 구조' }

이렇게 하면 업로드가 끝나는 시점까지 기다리는 Promise를 만들 수 있다.
<br>

### 콜백 지옥 문제
이벤트만 사용하면 다음과 같은 문제가 생길 수 있다.
- 이벤트 안에서 또 다른 이벤트를 처리
- 파일 스트림 완료 이벤트를 각각 관리
- 에러 처리를 이벤트마다 따로 작성

```ts
bb.on('file', (name, file, info) => {
 const ws = createWriteStream(...);

 file.pipe(ws);

 ws.on('finish', () => {
   console.log('file done');
 });
});

bb.on('finish', () => {
 console.log('all done');
});
```
{: file='이벤트만 사용한 예제' }

하지만 이 방식에는 문제가 있다. <ins>여러 파일이 업로드</ins>될 경우 <ins>finish 이벤트는 busboy의 파싱 완료(multipart 파싱)를 의미할 뿐, 각 writeStream의 완료(파일 쓰기)까지 보장하지 않는다.</ins> (실제로 내가 curl로 테스트할 때 겪은 문제이기도 하다.) <br>
무엇보다 에러 처리도 이벤트마다 따로 작성해야 한다. 즉, **콜백 중첩 구조가 생기기 쉽다**.

```
HTTP request stream
        ↓
req.pipe(busboy)
        ↓
busboy가 multipart 파싱
        ↓
file 이벤트 발생
        ↓
file stream → writeStream으로 pipe
        ↓
busboy 파싱 완료 → finish 이벤트
        ↓
writeStream들이 계속 디스크에 쓰는 중
        ↓
각 writeStream finish
```
{: file='파일 업로드 처리 흐름 / 쓸 곳이 마땅히 없어서 여기다 적는다.' }
<br><br>



## Promise + async/await 방식
콜백 지옥 문제 해결과 업로드 완료 시점 대기를 위해 busboy 이벤트를 Promise로 감싸는 패턴을 사용한다. <br>

```ts
await new Promise<void>((resolve, reject) => {
 const tasks: Promise<void>[] = [];

 bb.on('file', (name, file, info) => {
   const task = pipeline(file, createWriteStream(...));
   tasks.push(task);
 });

 bb.on('finish', async () => {
   try {
     await Promise.all(tasks);
     resolve();
   } catch (err) {
     reject(err);
   }
 });

 bb.on('error', reject);

 req.pipe(bb);
});
```
{: file='Promise + async/await 방식' }

이 방식의 장점은 다음과 같다.
- 모든 파일 스트림 완료 후 resolve
- async/await로 업로드 완료까지 기다릴 수 있음
- Promise.all로 여러 파일을 동시에 관리 가능
- try/catch로 에러 처리 통합 가능

즉 이벤트 기반 코드를 async/await 스타일로 깔끔하게 사용할 수 있다.
<br><br>



## req.pipe(bb)
이 코드가 간단하면서도 생각 이상으로 중요해서 따로 적어둔다. <br>
busboy는 <ins>HTTP 요청 스트림을 파싱하는 라이브러리</ins>이기 때문에 `req.pipe(bb)`로 <ins>request 스트림을 연결해야 이벤트가 발생</ins>한다. <br>
이 호출이 없으면 busboy는 데이터를 받지 못해 어떤 이벤트도 발생하지 않는다.



## 정리
busboy 이벤트는 비동기지만 Promise를 반환하지 않기 때문에 await으로 직접 기다릴 수 없다. <br>
그래서 Promise로 래핑하여 async/await으로 사용할 수 있도록 만드는 패턴을 사용한다.
