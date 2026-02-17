# 🎬 YouTube Summary Skill

[![OpenClaw Skill](https://img.shields.io/badge/OpenClaw-Skill-blueviolet)](https://github.com/OpenClaw)
[![Platform](https://img.shields.io/badge/platform-macOS%20%7C%20Linux-lightgrey)](https://github.com/VoidLight00/youtube-summary-skill)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

유튜브 링크를 던지면 **구조화된 Obsidian 마크다운 노트**를 자동 생성하는 OpenClaw 스킬.

## ✨ 기능

| 기능 | 설명 |
|------|------|
| 🎙️ 트랜스크립트 추출 | `summarize` CLI로 자동 자막/트랜스크립트 추출 |
| 📝 구조화 노트 | 3줄 요약, 핵심 개념, 구간별 상세, 적용점 |
| ⏱️ 타임스탬프 연동 | `play MM:SS` 형식으로 Obsidian 내 영상 시간 이동 |
| 💎 Obsidian 네이티브 | callout, iframe 임베드, frontmatter 완벽 지원 |
| 🧠 사고 확장 프롬프트 | 영상 시청 후 깊은 사고를 유도하는 질문 템플릿 |

## 📋 예시 출력

### 구간 가이드
```
| # | 시간                | 구간           | 핵심 1줄                    |
|---|---------------------|----------------|-----------------------------|
| 1 | play 00:00          | 인트로         | 뇌과학으로 본 학습의 원리    |
| 2 | play 03:22          | 기억의 메커니즘 | 해마와 장기기억의 관계       |
| 3 | play 08:45          | 간격 반복      | 에빙하우스 망각곡선 극복법   |
| 4 | play 15:10          | 능동적 회상    | 시험 효과와 인출 연습       |
| 5 | play 22:30          | 실전 적용      | 일상에서 쓰는 뇌과학 학습법 |
```

### 구간별 상세 (일부)
```markdown
### 2. 기억의 메커니즘 `3:22 ~ 8:44`

> [!info]- 📺 이 구간 보기
> <iframe width="100%" height="250" src="https://www.youtube.com/embed/xxxxx?start=202&end=524&enablejsapi=1" ...></iframe>

> [!quote] 핵심
> 해마는 단기기억을 장기기억으로 전환하는 관문이다

해마(hippocampus)는 새로운 정보를 받아들여 대뇌피질로 전송하는 역할을 한다.
수면 중 기억 공고화(memory consolidation)가 일어나며...

> [!tip]+ 💡 인사이트
> 학습 후 충분한 수면이 기억 정착의 핵심. 밤샘 공부보다 분산 학습 + 숙면이 효과적.
```

## 📦 요구사항

- **[OpenClaw](https://github.com/OpenClaw)** — AI 에이전트 플랫폼
- **summarize CLI** — 트랜스크립트 추출 도구
  ```bash
  brew install steipete/tap/summarize
  ```
- **[YouTube OneClick Timestamp](https://github.com/VoidLight00/obsidian-youtube-oneclick)** (필수) — 타임스탬프 클릭 시 노트 안 임베드 영상에서 해당 시간으로 점프

### YouTube OneClick Timestamp 설치 (BRAT)

이 플러그인이 없으면 타임스탬프 링크가 유튜브 외부로 이동합니다. **반드시 설치하세요.**

1. **BRAT 설치** (이미 있으면 건너뛰기)
   - Obsidian → 설정 → 커뮤니티 플러그인 → 찾아보기 → `BRAT` 검색 → 설치 → 활성화

2. **YouTube OneClick Timestamp 추가**
   - Obsidian → 설정 → BRAT → `Add Beta plugin`
   - 아래 URL 입력:
     ```
     VoidLight00/obsidian-youtube-oneclick
     ```
   - `Add Plugin` 클릭

3. **플러그인 활성화**
   - 설정 → 커뮤니티 플러그인 → `YouTube OneClick Timestamp` 토글 ON

4. **확인**
   - 노트에서 `[play 03:30](https://youtube.com/watch?v=...)` 클릭 시 임베드 영상이 3:30으로 점프하면 성공! 🎉

## 🚀 설치

### 방법 1: 링크 던지기 (권장)

OpenClaw에게 유튜브 링크를 보내면 자동으로 스킬이 활성화됩니다.

```
https://www.youtube.com/watch?v=dQw4w9WgXcQ
```

### 방법 2: 수동 설치

1. 이 레포를 클론합니다:
   ```bash
   git clone https://github.com/VoidLight00/youtube-summary-skill.git
   ```
2. `SKILL.md`를 OpenClaw 워크스페이스의 skills 폴더에 복사합니다:
   ```bash
   cp SKILL.md ~/.openclaw/skills/youtube-summary.md
   ```

## ⚙️ 커스터마이징

### 저장 경로
`SKILL.md` 내 저장 경로 섹션에서 기본 경로를 변경할 수 있습니다.
```
기본: ~/Documents/YouTubeNotes/
```

### 카테고리 폴더
영상 주제에 따라 자동 분류됩니다. 폴더 구조를 수정하려면 `SKILL.md`의 카테고리 테이블을 편집하세요.

| 카테고리 | 폴더명 |
|---------|--------|
| AI | `942-AI/` |
| 생산성 | `943-생산성/` |
| 비즈니스 | `944-비즈니스/` |
| 기타 | `945-기타/` |

### frontmatter 필드
`author`, `speaker`, `tags` 등의 기본값을 SKILL.md에서 수정 가능합니다.

### 사고 확장 질문
`💭 내 생각` 섹션의 질문을 자유롭게 추가/수정할 수 있습니다.

## 📁 프로젝트 구조

```
youtube-summary-skill/
├── SKILL.md                        # OpenClaw 스킬 정의
├── README.md                       # 이 파일
├── LICENSE                         # MIT 라이선스
├── .gitignore
└── references/
    └── template-example.md         # 실제 예시 노트
```

## 📄 라이선스

[MIT](LICENSE) © VoidLight00
