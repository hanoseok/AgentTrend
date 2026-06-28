# Agent Trend

AI 에이전트 제품, 플랫폼, 프로토콜, 프레임워크, 기업 도입, 투자, 벤치마크, 정책 신호를 정리하는 리서치 저장소입니다.

## 구조

```text
.
├── AGENTS.md
├── README.md
├── index.html
├── raw/
│   ├── assets/
│   ├── articles/
│   ├── papers/
│   └── notes/
├── wiki/
│   ├── index.md
│   ├── log.md
│   ├── sources/
│   ├── entities/
│   ├── concepts/
│   ├── comparisons/
│   └── syntheses/
├── history/
│   └── YYYY-MM-DD/
├── report/
│   └── {report-name}/
├── source/
│   └── {category}/
└── tools/
```

## 디렉터리 역할

- `raw/`: 외부 원본 자료 보관 위치입니다. LLM은 이 영역을 읽기만 하고 수정하지 않습니다.
- `wiki/`: LLM이 생성하거나 갱신하는 Markdown 위키입니다.
- `history/`: 날짜별 조사 원문, 이전 raw 문서, 업데이트 로그를 보관합니다.
- `report/`: 공개 HTML 리포트입니다. 각 리포트는 `index.html`을 기본 문서로 사용하고, 업데이트 이력은 `update.html`에 둡니다.
- `source/`: 자동화가 확인할 소스 묶음입니다. 카테고리별로 관리합니다.
- `tools/`: 검색, lint, export 같은 보조 CLI 도구를 둘 수 있는 선택 영역입니다.

## 주요 링크

- [Report Index](index.html)
- [Agent Trend Brief](report/agent-trend-brief/)
- [Document History](history/)
- [Source Watchlist](source/)
- [Wiki Index](wiki/index.md)

## 새 리포트 작성 규칙

1. 공개 HTML은 `report/{report-name}/` 또는 `report/{report-name}/{section}/` 아래에 둡니다.
2. 날짜별 원문 Markdown은 `history/YYYY-MM-DD/`에 둡니다.
3. 자동화용 소스 묶음은 `source/{category}/`에 갱신합니다.
4. 외부 원본 자료는 `raw/` 아래에만 보관하고, 요약과 해석은 `wiki/` 또는 `report/`에 작성합니다.
5. 루트 `index.html`은 리포트별 카탈로그 역할만 합니다.
