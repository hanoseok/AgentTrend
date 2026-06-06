# Agent Trend 자료 저장 규칙

이 폴더는 에이전트 트렌드 조사 자료를 날짜별로 저장한다.

## 작성 기준

이 자료는 내부 공유와 CEO 보고에 사용할 수 있는 수준으로 작성한다.

- 단순 뉴스 요약이 아니라 기술 변화, 시장 신호, 제품화 가능성, 카카오 영향, 의사결정 포인트를 함께 정리한다.
- 중요한 주장은 1차 출처 또는 신뢰 가능한 공식 출처를 우선 확인하고 출처 링크를 남긴다.
- 사실, 해석, 제안을 구분한다.
- 카카오 AI 서비스와 에이전트 플랫폼 관점에서 무엇을 해야 하는지까지 결론을 낸다.
- 기존에 조사한 주제는 매번 업데이트 여부, 변경점, 리스크 변화를 확인한다.
- CEO 보고용으로 5분 안에 읽히는 핵심 요약과 실무자가 바로 이어서 볼 수 있는 기술 상세를 함께 둔다.

## 폴더 규칙

- 날짜 폴더: `YYYY-MM-DD`
- 정기 브리프 파일: `HHmm_agent_trend_brief.html`
- 주제별 딥다이브 파일: `{Topic}_Deep_Dive.html`
- 원본/raw 파일: `YYYY-MM-DD/raw/{파일명}.md`
- 보고 템플릿: `_templates/Brief_Template.html`
- 운영 원칙: `REPORTING_STANDARD.html`
- 조사 이력: `RESEARCH_LOG.html`
- 소스 watchlist: `SOURCE_WATCHLIST.html`

예:

- `2026-06-06/A2A_Deep_Dive.html`
- `2026-06-06/raw/A2A_Deep_Dive.md`
- `2026-06-07/0800_agent_trend_brief.html`
- `2026-06-07/raw/0800_agent_trend_brief.md`
- `RESEARCH_LOG.html`
- `2026-06-06/raw/RESEARCH_LOG.md`
- `SOURCE_WATCHLIST.html`
- `2026-06-06/raw/SOURCE_WATCHLIST.md`

## 정기 브리프 체크 항목

정기 브리프는 매번 아래 항목을 확인한다.

- 새로 나온 중요한 에이전트 관련 뉴스
- 이전에 조사한 주제의 업데이트, 정정, 제품화, 보안 이슈
- 카카오 AI 서비스와 에이전트 플랫폼에 필요한 액션
- 당장 추적해야 할 리스크와 의사결정 포인트

## 현재 문서

- `RESEARCH_LOG.html`: 조사 이력 요약
- `index.html`: Agent Trend 전체 인덱스
- `SOURCE_WATCHLIST.html`: 정기 확인 소스 Watchlist
- `2026-06-06/UCP_Deep_Dive.html`: 2026-06-06 UCP Deep Dive
- `2026-06-06/AP2_Deep_Dive.html`: 2026-06-06 AP2 Deep Dive
- `2026-06-06/AG_UI_Deep_Dive.html`: 2026-06-06 AG-UI Deep Dive
- `2026-06-06/A2_UI_Deep_Dive.html`: 2026-06-06 A2-UI Deep Dive
- `2026-06-06/Selected_Agent_Project_Paper_Deep_Dive.html`: 2026-06-06 Selected Agent Project / Paper Deep Dive
- `2026-06-06/2000_agent_trend_brief.html`: 2026-06-06 20:00 Agent Trend Brief
- `2026-06-06/Agent_Trend_Scout.html`: 2026-06-06 Agent Trend Scout
- `2026-06-06/A2A_Deep_Dive.html`: 2026-06-06 A2A Deep Dive
- `2026-06-06/raw/UCP_Deep_Dive.md`: UCP raw 원본
- `2026-06-06/raw/AP2_Deep_Dive.md`: AP2 raw 원본
- `2026-06-06/raw/AG_UI_Deep_Dive.md`: AG-UI raw 원본
- `2026-06-06/raw/A2_UI_Deep_Dive.md`: A2-UI raw 원본
- `2026-06-06/raw/index.md`: 전체 인덱스 raw 원본
- `2026-06-06/raw/Selected_Agent_Project_Paper_Deep_Dive.md`: Selected Agent Project / Paper raw 원본
- `2026-06-06/raw/A2A_Deep_Dive.md`: A2A raw 원본
