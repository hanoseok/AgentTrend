# Agent Trend 자료 저장 규칙

이 폴더는 에이전트 트렌드 조사 자료를 날짜별로 저장한다.

## 작성 기준

이 자료는 내부 공유와 브리핑에 사용할 수 있는 수준으로 작성한다.

- 단순 뉴스 요약이 아니라 기술 변화, 시장 신호, 제품화 가능성, AI 플랫폼/서비스 영향, 의사결정 포인트를 함께 정리한다.
- 중요한 주장은 1차 출처 또는 신뢰 가능한 공식 출처를 우선 확인하고 출처 링크를 남긴다.
- 사실, 해석, 제안을 구분한다.
- AI 플랫폼 및 서비스 관점에서 무엇을 해야 하는지까지 결론을 낸다.
- 기존에 조사한 주제는 매번 업데이트 여부, 변경점, 리스크 변화를 확인한다.
- 브리핑에서 5분 안에 읽히는 핵심 요약과 실무자가 바로 이어서 볼 수 있는 기술 상세를 함께 둔다.

## 폴더 규칙

- 날짜 폴더: `YYYY-MM-DD`
- 정기 브리프 파일: `HHmm_agent_trend_brief.html`
- 주제별 딥다이브 파일: `{Topic}_Deep_Dive.html`
- 원본/raw 파일: `YYYY-MM-DD/raw/{파일명}.md`
- 보고 템플릿: `tools/templates/brief-template.html`
- 운영 원칙: `report/reporting-standard/`
- 조사 이력: `report/research-log/`
- 소스 watchlist: `source/`

예:

- `report/a2a-deep-dive/`
- `history/2026-06-06/A2A_Deep_Dive.md`
- `2026-06-07/0800_agent_trend_brief.html`
- `2026-06-07/raw/0800_agent_trend_brief.md`
- `report/research-log/`
- `wiki/sources/research-log.md`
- `source/`
- `history/2026-06-06/SOURCE_WATCHLIST.md`

## 정기 브리프 체크 항목

정기 브리프는 매번 아래 항목을 확인한다.

- 새로 나온 중요한 에이전트 관련 뉴스
- 이전에 조사한 주제의 업데이트, 정정, 제품화, 보안 이슈
- AI 플랫폼 및 서비스에 필요한 액션
- 당장 추적해야 할 리스크와 의사결정 포인트

## 외부 참고 링크

지금까지 실제 조사에 사용한 전체 외부 링크 카탈로그는 `index.html#references`와 `wiki/index.md`의 Source Catalog에 주제별로 둔다. 개별 보고서의 근거 링크는 관련 요약 바로 아래에 붙인다.

참고 링크: [A2A](https://a2a-protocol.org/latest/), [MCP](https://modelcontextprotocol.io/), [UCP](https://ucp.dev/), [AP2](https://ap2-protocol.org/), [AG-UI](https://docs.ag-ui.com/), [A2UI](https://a2ui.org/), [Dynamic Workflows](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code), [AgentBound](https://programming-group.com/assets/pdf/papers/2026_AgentBound-Securing-Execution-Boundaries-of-AI-Agents.pdf), [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401), [COLLEAGUE.SKILL](https://arxiv.org/abs/2605.31264), [SkillNet](https://arxiv.org/abs/2603.04448), [Hermes Agent](https://github.com/NousResearch/hermes-agent)

## 현재 문서

- `report/research-log/`: 조사 이력 요약
- `index.html`: Agent Trend 전체 인덱스
- `source/`: 정기 확인 소스 Watchlist
- `report/ai-platform-service-applicability/`: 2026-06-06 AI 플랫폼 및 서비스 적용 포인트
- `report/ucp-deep-dive/`: 2026-06-06 UCP Deep Dive
- `report/ap2-deep-dive/`: 2026-06-06 AP2 Deep Dive
- `report/ag-ui-deep-dive/`: 2026-06-06 AG-UI Deep Dive
- `report/a2-ui-deep-dive/`: 2026-06-06 A2-UI Deep Dive
- `report/selected-agent-project-paper-overview/`: 2026-06-06 Selected Agent Project / Paper Deep Dive
- `report/agent-trend-brief/2026-06-06-2000/`: 2026-06-06 20:00 Agent Trend Brief
- `report/agent-trend-scout/`: 2026-06-06 Agent Trend Scout
- `report/a2a-deep-dive/`: 2026-06-06 A2A Deep Dive
- `report/claude-code-dynamic-workflows/`: 2026-06-07 Claude Code Dynamic Workflows Deep Dive
- `report/agentbound-deep-dive/`: 2026-06-07 AgentBound Deep Dive
- `report/swe-skills-bench-deep-dive/`: 2026-06-07 SWE-Skills-Bench Deep Dive
- `report/colleague-skill-deep-dive/`: 2026-06-07 COLLEAGUE.SKILL Deep Dive
- `report/hermes-agent-deep-dive/`: 2026-06-07 Hermes Agent Deep Dive
- `report/skillnet-deep-dive/`: 2026-06-07 SkillNet Deep Dive
- `history/2026-06-06/UCP_Deep_Dive.md`: UCP raw 원본
- `history/2026-06-06/AP2_Deep_Dive.md`: AP2 raw 원본
- `history/2026-06-06/AG_UI_Deep_Dive.md`: AG-UI raw 원본
- `history/2026-06-06/A2_UI_Deep_Dive.md`: A2-UI raw 원본
- `history/2026-06-06/AI_Platform_Service_Applicability.md`: AI 플랫폼 및 서비스 적용 raw 원본
- `wiki/index.md`: 전체 인덱스 raw 원본
- `history/2026-06-06/Selected_Agent_Project_Paper_Deep_Dive.md`: Selected Agent Project / Paper raw 원본
- `history/2026-06-06/A2A_Deep_Dive.md`: A2A raw 원본
- `history/2026-06-07/Claude_Code_Dynamic_Workflows_Deep_Dive.md`: Dynamic Workflows raw 원본
- `history/2026-06-07/AgentBound_Deep_Dive.md`: AgentBound raw 원본
- `history/2026-06-07/SWE_Skills_Bench_Deep_Dive.md`: SWE-Skills-Bench raw 원본
- `history/2026-06-07/COLLEAGUE_SKILL_Deep_Dive.md`: COLLEAGUE.SKILL raw 원본
- `history/2026-06-07/Hermes_Agent_Deep_Dive.md`: Hermes Agent raw 원본
- `history/2026-06-07/SkillNet_Deep_Dive.md`: SkillNet raw 원본
