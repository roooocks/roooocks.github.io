---
title: "[VSCode] settings.json"
description: 개인용 에디터 설정
date: 2026-02-05 16:06:00 +0900
categories: [개발 환경, 에디터 / IDE]
---

vscode/ 안에다 넣어 사용하는 파일로 <ins>에디터 설정을 GUI가 아닌 텍스트로 설정</ins>해서 사용하는 방식이다. <br>

```json
{
  "editor.tabSize": 2,

  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",

  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```
