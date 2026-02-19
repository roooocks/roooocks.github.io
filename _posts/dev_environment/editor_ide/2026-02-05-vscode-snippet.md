---
title: "[VSCode] snippets 사용법"
description: 그래서 이거 무슨 템플릿임?
date: 2026-02-05 17:53:00 +0900
categories: [개발 환경, 에디터 / IDE]
---

## 출처
- Perplexity


## 정의
매번 같은 내용을 수동으로 만들지 않고 미리 만들어두어 나중에 <ins>약어를 통해 자동으로 내용을 불러오는 방식</ins>을 VSCode에서 snippets이라 부른다. <br>


## 사용법
1. 아래 2가지 방법으로 Snippets 선택
  - 왼쪽 하단에 설정 선택
  - File > Preferences > Configure Snippets
2. 아래 3가지 유형 중 하나의 Snippets 선택
  - 글로벌 방식(어디서든 사용)
  - 폴더 방식(폴더 이름이 나온다. 해당 폴더에서만 사용)
  - 언어 방식(관련 언어에서만 사용)
3. 파일이 생성되면 필요한 내용을 집어 넣는다.


## 구조
```json
{
	"Print to console": {
		"prefix": "log",
		"body": [
			"console.log('$1');",
			"$2"
		],
		"description": "Log output to console"
	}
}
```
{: file='makefile.json 예제' }

- Print to console : 스니펫 이름
- prefix : 에디터에서 사용할 단축어 
- body : 실제 사용될 코드. 여러 줄이라면 배열로 한 줄씩 입력한다.
- description : 자동완성 목록에 보일 설명(선택사항)


## 내장 변수
엄청 많은데, 아래에 있는 것들만 알아도 충분히 써먹는다. <br>
만약 다른걸 쓸 날이 온다면 그 때 다시 작성하도록 하자

### 시간
📅 날짜
- $CURRENT_YEAR → 연도
- $CURRENT_MONTH → 월 (01~12)
- $CURRENT_DATE → 일 (01~31)
- $CURRENT_DAY_NAME → 요일 (Monday)
- $CURRENT_DAY_NAME_SHORT → 요일 축약 (Mon)
<br>

⏰ 시간
- $CURRENT_HOUR → 시 (00~23)
- $CURRENT_MINUTE → 분 (00~59)
- $CURRENT_SECOND → 초 (00~59)


### 파일 / 에디터 관련 변수
📄 파일 정보
- $TM_FILENAME → 현재 파일명 (app.js)
- $TM_FILENAME_BASE → 확장자 없는 파일명 (app)
- $TM_FILEPATH → 전체 경로
- $TM_DIRECTORY → 파일이 있는 디렉터리 경로

📌 커서 / 선택 영역
- $TM_LINE_INDEX → 현재 줄 번호 (0부터)
- $TM_LINE_NUMBER → 현재 줄 번호 (1부터)
- $TM_CURRENT_LINE → 현재 줄 전체 텍스트
- $TM_CURRENT_WORD → 커서가 위치한 단어
- $TM_SELECTED_TEXT → 선택된 텍스트


### 환경 변수
$HOME, $PATH, $USER 등
