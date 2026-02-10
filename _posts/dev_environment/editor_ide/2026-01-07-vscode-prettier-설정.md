---
title: "[VSCode] prettier 적용 방법"
description: 개떡같이 작성해도 찰떡같이 작성되기!
date: 2026-01-07 14:56:00 +0900
categories: [개발 환경, 에디터 / IDE]
---

prettier의 사용법은 크게 2가지로 나뉜다.

## 개인용 설정
```json
{
  "editor.tabSize": 2,

  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",

  "prettier.semi": true,
  "prettier.tabWidth": 2,
  "prettier.useTabs": false,
  "prettier.trailingComma": "es5"
}
```
{ file='vscode/settings.json' }


## 팀원 단위 설정
Prettier의 자체 설정 파일(.prettierrc)을 활용한다. <br>
CLI 버전에서도 prettier의 사용이 가능하기 때문에 VSCode의 Extensions가 아닌 Node 패키지를 따로 받아야 한다.

```json
// 여기서는 prettier 연결만 담당한다.
// 설정을 여기서 해도 되지만, 그러면 충돌 가능성이 있다.
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
}
```
{ file='vscode/settings.json' }

```json
{
  "singleQuote": true,
  "semi": false,
  "printWidth": 100,
  "tabWidth": 2,
  "trailingComma": "all"
}
```
{ file='.prettierrc json 버전' }