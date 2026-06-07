# Agent Trend 조사 이력

- 최종 갱신: 2026-06-07 11:17 KST
- 기준: 현재 폴더에 저장 완료된 HTML 정본과 raw 원본
- 정기 업데이트: 08:00 / 16:00 / 00:00 KST

## 0. Source Catalog

조사에 사용한 전체 외부 링크 카탈로그는 `index.html#references`와 `2026-06-06/raw/index.md`의 Source Catalog에 둔다. 개별 보고서의 근거 링크는 관련 요약 바로 아래에 붙인다. 아래는 조사 이력에서 가장 자주 참조하는 핵심 출처다.

참고 링크: [A2A](https://a2a-protocol.org/latest/), [MCP](https://modelcontextprotocol.io/), [UCP](https://ucp.dev/), [AP2](https://ap2-protocol.org/), [AG-UI](https://docs.ag-ui.com/), [A2UI](https://a2ui.org/), [Dynamic Workflows](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code), [AgentBound](https://programming-group.com/assets/pdf/papers/2026_AgentBound-Securing-Execution-Boundaries-of-AI-Agents.pdf), [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401), [COLLEAGUE.SKILL](https://arxiv.org/abs/2605.31264), [Hermes Agent](https://github.com/NousResearch/hermes-agent), [SkillNet](https://arxiv.org/abs/2603.04448), [Google AX](https://github.com/google/ax), [Google AX v0.1.0](https://github.com/google/ax/releases/tag/v0.1.0), [Agent Substrate](https://github.com/agent-substrate/substrate), [GKE Agent Sandbox](https://docs.cloud.google.com/kubernetes-engine/docs/how-to/agent-sandbox), [Kimi Code CLI](https://github.com/MoonshotAI/kimi-code), [Cline SDK](https://docs.cline.bot/sdk/overview), [Agent Tasks API](https://github.blog/changelog/2026-06-04-agent-tasks-rest-api-now-available-for-copilot-pro-pro-and-max/)

## 1. 저장 완료된 조사 자료

| 날짜 | 조사 주제 | 조사 수준 | 핵심 범위 | 산출물 |
| --- | --- | --- | --- | --- |
| 2026-06-07 | Google Agent Executor / AX | Detailed Deep Dive, Distributed agent runtime / control plane | official site, GitHub README/release/proto/code/issues, Agent Substrate, GKE Agent Sandbox, Kubernetes Agent Sandbox, GeekNews/community signals를 확인. Controller, registry, event log, SQLite ledger, resumption, fork, trace, A2A/ADK/Colab interop, MCP readiness gap, credential/security gap, PoC plan을 AI 플랫폼 및 서비스 설계 관점으로 정리 | `2026-06-07/Google_AX_Deep_Dive.html`, `2026-06-07/raw/Google_AX_Deep_Dive.md` |
| 2026-06-07 | 0900 Agent Trend Brief | 8시간 Brief, Watchlist update / runtime signals | 필수 소스와 공식 보조 소스를 확인하고 Google AX, Kimi Code CLI, Cline SDK/runtime, Copilot Agent Tasks API, Copilot App canvas/cloud automation, Colab CLI, Codex Sites, Task Observer를 Project / Paper Alert와 신규 사항으로 정리 | `2026-06-07/0900_agent_trend_brief.html`, `2026-06-07/raw/0900_agent_trend_brief.md` |
| 2026-06-07 | Source Watchlist Update | Operations, 지속 모니터링 소스 확장 | Google AX, Agent Substrate/GKE Agent Sandbox, Kimi Code CLI, Cline SDK/runtime, OpenAI Codex Sites, GitHub Copilot agent platform updates, Google Colab CLI를 관련 보조 소스로 추가하고 추가 이유와 관찰할 신호를 기록 | `SOURCE_WATCHLIST.html`, `2026-06-07/raw/SOURCE_WATCHLIST.md` |
| 2026-06-07 | Document Update History | Operations, 문서 업데이트 이력 | 각 HTML 정본의 생성/업데이트 타임라인, 최신 업데이트순 Reports 정렬 기준, 문서별 history anchor를 정리 | `DOCUMENT_HISTORY.html`, `2026-06-07/raw/DOCUMENT_HISTORY.md` |
| 2026-06-07 | Claude Code Dynamic Workflows / ultracode | Expanded Deep Dive, Agent runtime / orchestration | dynamic workflow code 생성, subagent 병렬 실행, task-specific harness, checkpoint, verifier, cost cap, human approval, 평가 프로토콜을 workflow runtime 요구사항으로 정리 | `2026-06-07/Claude_Code_Dynamic_Workflows_Deep_Dive.html`, `2026-06-07/raw/Claude_Code_Dynamic_Workflows_Deep_Dive.md` |
| 2026-06-07 | AgentBound | Expanded Deep Dive, MCP permission boundary | MCP server access control, AgentManifest, policy vocabulary, runtime enforcement, threat model, connector admission flow, risk tier, approval policy, audit schema를 AI 플랫폼 설계 관점으로 정리 | `2026-06-07/AgentBound_Deep_Dive.html`, `2026-06-07/raw/AgentBound_Deep_Dive.md` |
| 2026-06-07 | SWE-Skills-Bench | Expanded Deep Dive, Skill eval benchmark | public SWE skills의 marginal utility, paired evaluation method, pass-rate delta, token overhead, version mismatch, skill contract, 내부 평가 playbook을 정리 | `2026-06-07/SWE_Skills_Bench_Deep_Dive.html`, `2026-06-07/raw/SWE_Skills_Bench_Deep_Dive.md` |
| 2026-06-07 | COLLEAGUE.SKILL | Expanded Deep Dive, Trace-to-skill / expert skill | human/role trace를 versioned skill package로 distill하는 흐름, lifecycle, artifact contract, capability track, bounded behavior track, privacy/consent/IP/governance 리스크와 평가 프레임을 정리 | `2026-06-07/COLLEAGUE_SKILL_Deep_Dive.html`, `2026-06-07/raw/COLLEAGUE_SKILL_Deep_Dive.md` |
| 2026-06-07 | Hermes Agent | Detailed Deep Dive, Persistent agent runtime / product benchmark | official docs/GitHub/release/community/security sources를 재확인하고 architecture, memory/profile state model, skill lifecycle, MCP catalog/filtering, cron/gateway automation, security controls, PoC metrics까지 확장 정리 | `2026-06-07/Hermes_Agent_Deep_Dive.html`, `2026-06-07/raw/Hermes_Agent_Deep_Dive.md` |
| 2026-06-07 | SkillNet | Expanded Deep Dive, Skill infrastructure / graph | skill repository, ontology, graph, supply pipeline, 5-D evaluation, skill data model, SWE-Skills-Bench와 결합한 measured utility 중심 skill platform 설계를 정리 | `2026-06-07/SkillNet_Deep_Dive.html`, `2026-06-07/raw/SkillNet_Deep_Dive.md` |
| 2026-06-06 | AI Platform / Service Applicability | Synthesis, 브리핑 + 적용 설계 | A2A, UCP, AP2, AG-UI, A2-UI, Dynamic Workflows, AgentBound, Hermes Agent, SkillNet 조사 결과를 특정 회사명 없이 AI 플랫폼 및 서비스 적용 관점으로 재정리. 레이어 맵, 사용 가능 포인트, 30/60/90일 실행안, 리스크를 포함 | `2026-06-06/AI_Platform_Service_Applicability.html`, `2026-06-06/raw/AI_Platform_Service_Applicability.md` |
| 2026-06-06 | A2A / Agent2Agent Protocol | Deep Dive, 브리핑 + 기술 상세 | Google 공개, Linux Foundation 이관, A2A v1.0, MCP와의 차이, Agent Card, Task lifecycle, Artifact, 프로토콜 흐름, 보안/거버넌스, Agent Registry와 Orchestrator 적용 방향 | `2026-06-06/A2A_Deep_Dive.html`, `2026-06-06/raw/A2A_Deep_Dive.md` |
| 2026-06-06 | Agent Trend Scout | Scout, 딥다이브 후보 발굴 | GeekNews, MarkTechPost, GitHub Blog, Reddit 커뮤니티를 훑어 Claude Code Dynamic Workflows, MCP Production Mess, GitHub Agent-Native UX, Agent Runtime Sandbox, Skills Marketplace, Agentic Web, Agentic Commerce Protocol Stack, Observability/Memory/Audit 후보를 정리 | `2026-06-06/Agent_Trend_Scout.html`, `2026-06-06/raw/Agent_Trend_Scout.md` |
| 2026-06-06 | 2000 Agent Trend Brief | 8시간 Brief, 수동 업데이트 | watchlist 기반 수동 업데이트. Claude Code Dynamic Workflows, MCP 2026-07-28 RC, AgentBound, SWE-Skills-Bench, COLLEAGUE.SKILL, GitHub Copilot App/gh skill, UCP/AP2/AG-UI를 Project / Paper Alert 중심으로 정리 | `2026-06-06/2000_agent_trend_brief.html`, `2026-06-06/raw/2000_agent_trend_brief.md` |
| 2026-06-06 | Selected Agent Project / Paper Overview | Overview, 브리핑 + 플랫폼 설계 후보 | 사용자가 지정한 1, 3, 4, 5, 7번 항목과 Hermes Agent, SkillNet 조사. Claude Code Dynamic Workflows, AgentBound, SWE-Skills-Bench, COLLEAGUE.SKILL, UCP/AP2/AG-UI, Hermes Agent, SkillNet을 workflow runtime, security boundary, skill infrastructure, service protocol 관점으로 묶은 개요. 실제 딥다이브는 2026-06-07 개별 문서로 분리 | `2026-06-06/Selected_Agent_Project_Paper_Deep_Dive.html`, `2026-06-06/raw/Selected_Agent_Project_Paper_Deep_Dive.md` |
| 2026-06-06 | UCP / Universal Commerce Protocol | Deep Dive, Agentic commerce protocol | commerce capability/discovery/checkout/order 표준. UCP roles, capability negotiation, `/.well-known/ucp`, REST/MCP/A2A/Embedded transport, AP2 결합, Google/Shopify/Universal Cart 업데이트, AI 플랫폼 커머스/쇼핑/예약/결제 적용 방향 | `2026-06-06/UCP_Deep_Dive.html`, `2026-06-06/raw/UCP_Deep_Dive.md` |
| 2026-06-06 | AP2 / Agent Payments Protocol | Deep Dive, Agent payment trust layer | Checkout Mandate, Payment Mandate, Verifiable Digital Credential, Trusted Surface, human-present/human-not-present 결제 위임, FIDO 표준화 흐름, 결제 플랫폼 agent 결제 승인/감사/분쟁 증거 설계 | `2026-06-06/AP2_Deep_Dive.html`, `2026-06-06/raw/AP2_Deep_Dive.md` |
| 2026-06-06 | AG-UI / Agent-User Interaction Protocol | Deep Dive, Agent frontend interaction | agent backend와 user-facing app을 연결하는 event protocol. Lifecycle, text, tool call, state snapshot/delta, activity, interrupt, custom events, AG-UI와 A2-UI 차이, 대화형 AI 서비스/커머스/CS UX event schema 적용 방향 | `2026-06-06/AG_UI_Deep_Dive.html`, `2026-06-06/raw/AG_UI_Deep_Dive.md` |
| 2026-06-06 | A2-UI / Agent-to-User Interface | Deep Dive, Generative UI spec | agent-generated declarative UI payload/spec. Surface, component, data model, action, catalog, validator, A2A/AG-UI/SSE/MCP transport, version status, Macaron-A2UI 논문, 대화형 AI 서비스 native renderer와 service component catalog 적용 방향 | `2026-06-06/A2_UI_Deep_Dive.html`, `2026-06-06/raw/A2_UI_Deep_Dive.md` |

## 2. 조사 깊이 기준

- Deep Dive: 특정 주제를 브리핑 판단과 실무 기술 설계까지 이어지도록 깊게 정리한 자료. 공식 출처, 기술 구조, 시장 신호, AI 플랫폼/서비스 적용 방향, 리스크, 권장 액션을 포함한다.
- 8시간 Brief: 새로 나온 트렌드와 기존 조사 업데이트를 빠르게 훑는 정기 브리프. 중요도, 변경점, AI 플랫폼/서비스 영향, 추적 리스크를 중심으로 정리한다.

## 3. 현재까지 설정된 지속 모니터링 범위

아래 범위는 아직 모두 개별 딥다이브로 저장된 것은 아니며, 8시간마다 새 소식과 기존 조사 업데이트를 확인하기로 설정된 추적 범위다.

| 트렌드 영역 | 조사 상태 | 확인할 내용 |
| --- | --- | --- |
| AI Agent Platforms | Monitoring | OpenAI, Google, Anthropic, Microsoft 등 플랫폼의 SDK, orchestration, hosted agent 기능 변화 |
| MCP | Monitoring | agent-to-tool 표준화, enterprise connector, 보안 모델, A2A와의 결합 패턴 |
| A2A | Deep Dive 완료 | agent-to-agent 표준화, Agent Card, Task lifecycle, signed card, enterprise security, 조직 내부 호환 표준 |
| Agent Commerce / Payments | Monitoring | 에이전트가 구매, 예약, 결제를 수행하는 흐름과 승인/책임/정산 모델 |
| Agent Security / Governance | Monitoring | prompt injection, delegated authorization, audit trace, human approval, policy enforcement |
| Evals / Observability | Monitoring | 에이전트 선택, 작업 성공률, tool call 품질, tracing, regression eval, 운영 지표 |
| Memory / Personalization | Monitoring | 장기 기억, 개인화 context, privacy boundary, 사용자 제어권 |
| On-device / Multimodal / Voice Agents | Monitoring | 음성, 이미지, 화면 이해, 기기 내 실행, 모바일 UX, 대화형 AI 서비스/디바이스 접점 |

## 4. 다음 업데이트 방식

- 새 HTML 보고서가 생성되면 이 조사 이력에 날짜, 주제, 조사 수준, 핵심 범위, 링크를 추가한다.
- 기존 주제가 업데이트되면 같은 날짜에 "기존 조사 업데이트" 항목으로 남긴다.
- 브리핑에 쓸 수 있는 수준의 자료는 `Deep Dive`, 빠른 정기 스캔은 `8시간 Brief`로 구분한다.
