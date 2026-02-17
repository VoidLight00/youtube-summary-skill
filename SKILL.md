---
name: youtube-summary
description: 유튜브 링크를 보내면 summarize CLI로 트랜스크립트를 추출하고 Obsidian 마크다운 노트를 자동 생성하는 스킬. 타임스탬프 연동, 구간별 상세 정리, 사고 확장 프롬프트를 포함한 구조화된 노트를 만든다. Use when a user sends a YouTube URL and wants it summarized into an Obsidian note.
---

# YouTube Summary Skill

유튜브 링크를 받아 트랜스크립트를 추출하고, 구조화된 Obsidian 마크다운 노트를 자동 생성한다.

## 트리거

사용자가 유튜브 링크(youtube.com/watch, youtu.be 등)를 보내면 이 스킬이 활성화된다.

## 요구사항

- `summarize` CLI 설치 필요
  ```bash
  brew install steipete/tap/summarize
  ```
- **YouTube OneClick Timestamp** Obsidian 플러그인 (필수)
  - BRAT으로 설치: `VoidLight00/obsidian-youtube-oneclick`
  - 이 플러그인이 없으면 타임스탬프 클릭 시 유튜브 외부로 이동함
  - 설치하면 노트 안 임베드 영상에서 해당 시간으로 점프

## 설정

### 저장 경로
- 기본: `~/Documents/YouTubeNotes/`
- 사용자가 지정한 Obsidian Vault 경로 사용 가능

### 카테고리 폴더
| 카테고리 | 폴더명 |
|---------|--------|
| AI | `942-AI/` |
| 생산성 | `943-생산성/` |
| 비즈니스 | `944-비즈니스/` |
| 기타 | `945-기타/` |

### 파일명
`{영상제목 요약 한글}.md` — 카테고리 번호는 폴더에만, 파일명에는 넣지 않음.

## 절차

### Step 1: 트랜스크립트 추출

```bash
summarize "{URL}" --youtube auto --extract-only --length xxl
```

전체 트랜스크립트를 추출한다. 타임스탬프 정보가 포함된다.

### Step 2: 트랜스크립트 부족 시 요약 추출

트랜스크립트가 없거나 너무 짧은 경우:

```bash
summarize "{URL}" --youtube auto --length xxl
```

### Step 3: 노트 작성

아래 템플릿에 맞춰 마크다운 노트를 작성한다. 트랜스크립트 내용을 분석하여 구간을 나누고, 핵심 개념을 추출한다.

### Step 4: 파일 생성

카테고리를 판단하여 해당 폴더에 파일을 생성한다.

## 규칙

- frontmatter `author`는 항상 사용자 이름 (기본 "현")
- `speaker`에 채널명 또는 출연자
- 3줄 요약은 최상단에 배치
- 구간 가이드: **최소 5개** 이상, 타임스탬프는 `play` 형식
- 각 구간 상세에 접기 iframe (`> [!info]- 📺 이 구간 보기`) 필수
- 핵심 개념 3~7개, ★ 중요도 표기
- 적용점 최소 3개
- 💭 내 생각 섹션은 **빈칸으로 유지** (사용자가 직접 작성)
- 메인 iframe: `id="video-player"`, `youtube.com` 사용 (`youtube-nocookie.com` 아님)
- 구간 iframe: `youtube.com/embed`, `start`/`end` 파라미터 사용
- 인상적인 발언 2~3개 포함

## 마크다운 템플릿

```markdown
---
type: youtube
aliases:
  - {영상 제목 한글}
tags:
  - 유튜브
  - {카테고리 태그들}
CMDS: 
author: {사용자 이름, 기본 "현"}
speaker: {채널명 또는 출연자}
published: false
date created: {YYYY-MM-DD, HH:mm}
date modified: {YYYY-MM-DD, HH:mm}
index: 
status: inProgress
source_url: {유튜브URL}
duration: "{영상길이}"
rating: 
---

# 🎬 {영상 제목}

> 📅 {날짜} · 📺 {채널명} · ⏱️ {영상길이}

---

## 📌 3줄 요약

1. **{핵심1}**
2. **{핵심2}**
3. **{핵심3}**

---

## 📺 영상

<iframe id="video-player" width="100%" height="400" src="https://www.youtube.com/embed/{VIDEO_ID}?enablejsapi=1" frameborder="0" allowfullscreen></iframe>

---

## ⏱️ 구간 가이드

| # | 시간 | 구간 | 핵심 1줄 |
|---|------|------|----------|
| 1 | [play 00:00](https://youtube.com/watch?v={VIDEO_ID}&t=0s) | 인트로 | {요약} |
| ... | ... | ... | ... |

> 💡 타임스탬프 클릭 시 노트 안의 임베드 영상에서 해당 시간으로 이동 (youtube-iframe-timestamps 플러그인)

(최소 5개 이상 구간. 타임스탬프 형식: [play MM:SS](https://youtube.com/watch?v={VIDEO_ID}&t={초}s))

---

## 🧠 핵심 개념

| 개념 | 중요도 | 설명 |
|------|--------|------|
| {개념} | ★★★★★ | {설명} |

(3~7개)

---

## 📝 구간별 상세

### 1. {구간제목} `M:SS ~ M:SS`

> [!info]- 📺 이 구간 보기
> <iframe width="100%" height="250" src="https://www.youtube.com/embed/{VIDEO_ID}?start={START_SEC}&end={END_SEC}&enablejsapi=1" frameborder="0" allowfullscreen></iframe>

> [!quote] 핵심
> {한줄 요약}

{본문 상세 정리}

> [!tip]+ 💡 인사이트
> {실전 적용 팁}

---

(구간 반복)

---

## 🎯 적용점 (Action Items)

| # | 인사이트 | 적용 방향 |
|---|----------|----------|
| 1 | {발견} | {적용} |

(최소 3개)

---

## 💎 인상적인 발언

> [!quote] 💬
> "{인용문}"

(2~3개)

---

## 🔗 관련 자료

- {관련 링크/노트}

---

## 💭 내 생각

> [!abstract] 🧠 사고 확장 프롬프트
> 아래 질문들을 따라가며 자유롭게 적어보세요. 정답은 없습니다.

### 이 영상에서 가장 뇌리에 남는 한 마디는?
> 

### 왜 그 말이 나한테 꽂혔을까?
> 

### 이 내용이 내 현재 상황/프로젝트에 어떻게 연결될까?
> 

### 발표자의 주장에 동의하지 않는 부분이 있다면?
> 

### 만약 내가 이 주제로 강의한다면, 무엇을 다르게 말할까?
> 

### 6개월 후의 나에게 이 영상이 왜 중요할까?
> 

---

> 📋 템플릿: 크라켄 유튜브 요약 v1
```
