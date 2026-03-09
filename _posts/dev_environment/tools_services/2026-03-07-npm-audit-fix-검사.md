---
title: "[Node.js] npm audit fix와 npm audit --omit=dev"
description: npm 보안 취약점 검사와 production 패키지 검사 방법
date: 2026-03-07 16:36:00 +0900
categories: [개발 환경, 도구 & 서비스]
---

## 출처
 - 짤막하게 작성한 설명을 GPT가 요약 한 것을 수정
<br><br>



## npm audit 안내문이 나오는 경우
프로젝트에서 패키지를 설치하거나 실행할 때 아래와 같은 명령어가 안내문에 같이 나올 수 있다.

```bash
npm audit fix
```
{: file='자매품으로 npm audit fix --force(그냥 쓰지마)도 있다.' }

이 명령어는 **패키지에서 발견된 보안 취약점을 자동으로 수정할 수 있다는 의미**다. 이 경우 안내된 대로 명령어를 실행하면 된다. <br>
이 명령어는 **패키지 의존성에서 발견된 취약점을 가능한 범위 내에서 자동 수정한다**.
<br><br>



## production 패키지 검사
취약점 수정 이후에는 아래 명령어로 다시 검사할 수 있다.

```bash
npm audit --omit=dev
```

이 명령어의 의미
- **devDependencies 제외**
- **production 환경에 실제로 배포되는 패키지만 검사**

즉, 개발용 패키지가 아니라 **실제 서비스에 영향을 주는 패키지 보안만 검사**한다. <br>
이 방식은 보통 다음 상황에서 많이 사용한다.
- 배포 전 취약점 검사 및 보안 체크
- 실제 서비스에 영향 있는 패키지 확인
<br><br>



## 정상 결과
검사 결과 아래 메시지가 나오면 문제 없는 상태다.

```bash
# 현재 production에 포함되는 패키지에서 보안 취약점이 발견되지 않았다는 의미
found 0 vulnerabilities
```
