---
title: "[Node.js] pipe vs pipeline"
description: Node.js 스트림에서 pipe와 pipeline의 차이
date: 2026-03-08 18:48:16 +0900
categories: [백엔드, 라이브러리 / 패키지]
---

## 경고
예제의 코드는 **이렇게 쓴다를 보여주는 것**이지 **실제 사용은 안되는 것**을 알아두자.
<br><br>



## 출처
- 짤막하게 작성한 설명을 GPT, Gemini가 요약 한 것을 적절히 수정
<br><br>



## pipe()

### 정의
**Readable 스트림을 Writable 스트림으로 연결하는 <ins>Node.js 기본 함수</ins>**다. <br>
데이터를 메모리에 <ins>모으지 않고</ins> **실시간으로 스트림을 흘려보낸다.** (정확히는 청크 단위 만큼은 메모리로 간다. 쌓이지 않을 뿐이다.)
<br>

### 특징
- 사용법: `readable.pipe(writable)`
- 반환값: 연결된 Writable Stream(createWriteStream, busboy라면 예시 코드의 bb)
- 완료 보장: 직접 `finish` 이벤트 확인 필요
- 에러 처리: 각 스트림의 `error` 이벤트 별도 처리
- `async/await`: 사용 불가
- 멀티 스트림: 가능하지만 이벤트 수동 관리 필요
<br>

### 예시
```ts
import busboy from 'busboy';
import { createWriteStream, mkdirSync } from 'fs';
import { join } from 'path';
import { IncomingMessage } from 'http';

function handleUpload(req: IncomingMessage) {
  const uploadDir = join(process.cwd(), 'uploads');
  mkdirSync(uploadDir, { recursive: true });

  const bb = busboy({ headers: req.headers });

  bb.on('file', (name, file, info) => {
    const savePath = join(uploadDir, info.filename);
    file.pipe(createWriteStream(savePath)); // ← 여기서 pipe 사용
  });

  bb.on('finish', () => {
    console.log('All files uploaded!');
  });

  req.pipe(bb); // HTTP 요청 데이터를 busboy로 전달
}
```
{: file='pipe 예제' }

업로드된 파일을 **디스크로 바로 저장**한다.

특징
- 메모리 사용 최소화
- Node.js **Backpressure(데이터 흐름 제어. 받아야 할 데이터가 흘러 넘칠 것 같으면 주는 쪽의 데이터 흐름을 잠시 멈추게하는 기술) 자동 처리**
<br><br>



## pipeline()

### 정의
**Node.js 10+에서 제공되는 스트림 연결 함수**다. `pipe()`와 달리 다음 기능을 제공한다.
- 완료 시점 보장
- 에러 처리
- Promise 지원

즉, **pipe의 안전한 확장 버전**이라고 볼 수 있다.
<br>

### 특징
- 사용법: `await pipeline(readable, writable)`
- 반환값: `Promise` (TS면 `Promise<void>`)
- 완료 보장: Promise resolve 시점 = 모든 스트림 완료
- 에러 처리: `try/catch` 가능
- `async/await`: 사용 가능
- 멀티 스트림: `pipeline(a, b, c)` 형태로 체인 가능
<br>

### 예시
```ts
import { pipeline } from 'stream/promises';

await pipeline(file, createWriteStream('uploads/image.jpg'));
```

파일 쓰기 완료 시점까지 **안전하게 기다릴 수 있다.**

특징
- 스트림 완료 시점 보장
- 에러 발생 시 `try/catch` 처리 가능
- 멀티 파일 업로드에도 안전
<br><br>



## pipe와 pipeline 비교

| 항목          | pipe                  | pipeline           |
| :----------- | :---------------------: | ------------------: |
| 완료 보장       | 직접 `finish` 이벤트 확인 필요 | Promise로 완료 보장     |
| 에러 처리       | 각 스트림 `error` 이벤트 필요  | `try/catch`로 처리 가능 |
| async/await    | 불가능                   | 가능                 |
| 코드 가독성      | 이벤트 중첩 가능             | 순차적 코드             |
| 멀티 스트림      | 가능하지만 수동 관리           | 체인 + Promise 처리 가능 |

요약
- pipe = 단순 스트림 연결
- pipeline = 안전한 스트림 연결 + 완료 보장 + async/await 지원
<br><br>



## Busboy + pipeline + Promise.all 예제
```ts
import { pipeline } from 'stream/promises';
import { createWriteStream, mkdirSync } from 'fs';
import { join } from 'path';
import busboy from 'busboy';
import { Request } from 'express';

async function handleUpload(req: Request) {
  const basePath = join(process.cwd(), 'uploads');
  mkdirSync(basePath, { recursive: true });

  return await new Promise<void>((resolve, reject) => {
    const bb = busboy({ headers: req.headers });
    const tasks: Promise<void>[] = [];

    bb.on('file', (name, file, info) => {
      const savePath = join(basePath, info.filename);
      tasks.push(pipeline(file, createWriteStream(savePath)));
    });

    bb.on('error', reject);

    // 정리하면서 처음 알아서 적는다.
    // finish = Writable Stream에서 모든 데이터 처리가 정상적으로 끝났을 때
    // close = 스트림 종료(정상/비정상 전부 작동)
    bb.on('close', () => {
      Promise.all(tasks).then(() => resolve()).catch(reject);
    });

    req.pipe(bb);
  });
}
```
<br><Br>



### 핵심 포인트
- `pipeline(file, createWriteStream(...))`
  - 스트림을 바로 연결하고 Promise 반환
- `tasks.push(...)`
  - 각 파일 스트림 완료 Promise 저장
- `Promise.all(tasks)`
  - 모든 파일 스트림 완료 시점까지 대기

즉, **파일 스트림은 병렬로 흐르면서 메모리에 쌓이지 않는다**. 모든 파일이 안전하게 저장된 뒤 업로드가 완료된다.
<br><br>



## 스트림 동작 비유
스트림 처리를 간단히 비유하면 다음과 같다.

```
pipeline → 수도꼭지 → 호스 연결 → 물이 바로 흐름
Promise.all → 모든 호스의 물이 다 채워질 때까지 기다림
```

즉 **데이터를 모아서 처리하는 것이 아니라 실시간으로 흘려보내는 스트리밍 구조**다.
<br><br>



## 언제 pipe와 pipeline을 사용할까

### pipe 사용
- 단일 파일 업로드
- 간단한 스트림 연결
- 완료 시점 관리가 중요하지 않은 경우
<br>

### pipeline 사용
- 멀티 파일 업로드
- 업로드 완료 시점 필요
- async/await 기반 코드 작성
- 에러 처리를 안전하게 관리
<br><br>



## 정리
`pipe()`는 **단순한 스트림 연결 함수**다. <br>
`pipeline()`은 `pipe()` 기능과 함께 다음 기능을 추가로 제공한다.
- 스트림 완료 보장
- 에러 처리 통합
- async/await 지원
<br>

### 한 줄 요약
pipe = 단순 스트림 연결 <br>
pipeline = 안전한 스트림 연결 + 완료 보장 + async/await + 멀티 파일 처리 가능


### req.pipe()
이 게시물은 단순 pipe와 pipeline을 설명하는데에 있다. busboy에서 사용했던 req.pipe()는 req 스트림을 busboy로 흘려보내서 busboy가 multipart를 파싱하기 때문에 꼭 써야하는 설정이다.