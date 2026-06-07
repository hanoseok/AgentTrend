# Agent Trend 자료 저장 규칙

에이전트 트렌드 조사 자료를 브리핑과 내부 공유에 바로 쓸 수 있도록 날짜별 HTML 정본과 raw 원본으로 분리해 관리한다.

사람이 읽는 보고서는 날짜 폴더의 `.html`로 저장한다. 조사 원문, 초안, Markdown 원본은 같은 날짜 폴더 아래 `raw/`에 보관한다.

## 작성 기준

- **보고 목적:** 단순 뉴스 요약이 아니라 기술 변화, 시장 신호, 제품화 가능성, AI 플랫폼/서비스 영향, 의사결정 포인트를 함께 정리한다.
- **출처 기준:** 중요한 주장은 1차 출처 또는 신뢰 가능한 공식 출처를 우선 확인하고 링크를 남긴다.
- **판단 구조:** 사실, 해석, 제안을 구분하고 AI 플랫폼 및 서비스 관점의 결론을 낸다.
- **업데이트 추적:** 기존 조사 주제의 변경점, 정정, 제품화, 보안 이슈 변화를 매번 확인한다.

## 폴더 규칙

- 날짜 폴더: `YYYY-MM-DD`
- 정기 브리프 HTML: `YYYY-MM-DD/HHmm_agent_trend_brief.html`
- 정기 브리프 raw 원본: `YYYY-MM-DD/raw/HHmm_agent_trend_brief.md`
- 수동 업데이트 HTML: `YYYY-MM-DD/HHmm_agent_trend_manual.html`
- 수동 업데이트 raw 원본: `YYYY-MM-DD/raw/HHmm_agent_trend_manual.md`
- 주제별 딥다이브 HTML: `YYYY-MM-DD/{Topic}_Deep_Dive.html`
- 주제별 딥다이브 raw 원본: `YYYY-MM-DD/raw/{Topic}_Deep_Dive.md`
- 보고 템플릿: [`_templates/Brief_Template.html`](_templates/Brief_Template.html)
- 운영 원칙: [`REPORTING_STANDARD.html`](REPORTING_STANDARD.html)
- 조사 이력: [`RESEARCH_LOG.html`](RESEARCH_LOG.html)
- 소스 watchlist: [`SOURCE_WATCHLIST.html`](SOURCE_WATCHLIST.html)

## 정기 브리프 체크 항목

- 새로 나온 중요한 에이전트 관련 뉴스
- 이전에 조사한 주제의 업데이트, 정정, 제품화, 보안 이슈
- AI 플랫폼 및 서비스에 필요한 액션
- 당장 추적해야 할 리스크와 의사결정 포인트

## 외부 참고 링크

지금까지 실제 조사에 사용한 전체 외부 링크 카탈로그는 [Agent Trend 전체 인덱스의 Source Catalog](index.html#references)에 주제별로 둔다. 개별 보고서의 근거 링크는 관련 요약 바로 아래에 붙인다.

- [A2A](https://a2a-protocol.org/latest/)
- [MCP](https://modelcontextprotocol.io/)
- [UCP](https://ucp.dev/)
- [AP2](https://ap2-protocol.org/)
- [AG-UI](https://docs.ag-ui.com/)
- [A2UI](https://a2ui.org/)
- [Dynamic Workflows](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code)
- [AgentBound](https://programming-group.com/assets/pdf/papers/2026_AgentBound-Securing-Execution-Boundaries-of-AI-Agents.pdf)
- [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401)
- [COLLEAGUE.SKILL](https://arxiv.org/abs/2605.31264)
- [SkillNet](https://arxiv.org/abs/2603.04448)
- [Hermes Agent](https://github.com/NousResearch/hermes-agent)

## 현재 문서

- [조사 이력 요약](RESEARCH_LOG.html)
- [Agent Trend 전체 인덱스](index.html)
- [정기 확인 소스 Watchlist](SOURCE_WATCHLIST.html)
- [2026-06-06 AI 플랫폼 및 서비스 적용 포인트](2026-06-06/AI_Platform_Service_Applicability.html)
- [2026-06-06 UCP Deep Dive](2026-06-06/UCP_Deep_Dive.html)
- [2026-06-06 AP2 Deep Dive](2026-06-06/AP2_Deep_Dive.html)
- [2026-06-06 AG-UI Deep Dive](2026-06-06/AG_UI_Deep_Dive.html)
- [2026-06-06 A2-UI Deep Dive](2026-06-06/A2_UI_Deep_Dive.html)
- [2026-06-06 Selected Agent Project / Paper Deep Dive](2026-06-06/Selected_Agent_Project_Paper_Deep_Dive.html)
- [2026-06-06 20:00 Agent Trend Brief](2026-06-06/2000_agent_trend_brief.html)
- [2026-06-06 Agent Trend Scout](2026-06-06/Agent_Trend_Scout.html)
- [2026-06-06 A2A Deep Dive](2026-06-06/A2A_Deep_Dive.html)
- [2026-06-07 Claude Code Dynamic Workflows Deep Dive](2026-06-07/Claude_Code_Dynamic_Workflows_Deep_Dive.html)
- [2026-06-07 AgentBound Deep Dive](2026-06-07/AgentBound_Deep_Dive.html)
- [2026-06-07 SWE-Skills-Bench Deep Dive](2026-06-07/SWE_Skills_Bench_Deep_Dive.html)
- [2026-06-07 COLLEAGUE.SKILL Deep Dive](2026-06-07/COLLEAGUE_SKILL_Deep_Dive.html)
- [2026-06-07 Hermes Agent Deep Dive](2026-06-07/Hermes_Agent_Deep_Dive.html)
- [2026-06-07 SkillNet Deep Dive](2026-06-07/SkillNet_Deep_Dive.html)
- [UCP raw 원본](2026-06-06/raw/UCP_Deep_Dive.md)
- [AP2 raw 원본](2026-06-06/raw/AP2_Deep_Dive.md)
- [AG-UI raw 원본](2026-06-06/raw/AG_UI_Deep_Dive.md)
- [A2-UI raw 원본](2026-06-06/raw/A2_UI_Deep_Dive.md)
- [AI 플랫폼 및 서비스 적용 raw 원본](2026-06-06/raw/AI_Platform_Service_Applicability.md)
- [전체 인덱스 raw 원본](2026-06-06/raw/index.md)
- [Selected Agent Project / Paper raw 원본](2026-06-06/raw/Selected_Agent_Project_Paper_Deep_Dive.md)
- [A2A raw 원본](2026-06-06/raw/A2A_Deep_Dive.md)
- [Dynamic Workflows raw 원본](2026-06-07/raw/Claude_Code_Dynamic_Workflows_Deep_Dive.md)
- [AgentBound raw 원본](2026-06-07/raw/AgentBound_Deep_Dive.md)
- [SWE-Skills-Bench raw 원본](2026-06-07/raw/SWE_Skills_Bench_Deep_Dive.md)
- [COLLEAGUE.SKILL raw 원본](2026-06-07/raw/COLLEAGUE_SKILL_Deep_Dive.md)
- [Hermes Agent raw 원본](2026-06-07/raw/Hermes_Agent_Deep_Dive.md)
- [SkillNet raw 원본](2026-06-07/raw/SkillNet_Deep_Dive.md)
