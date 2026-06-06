# Agent Trend 조사 이력

- 최종 갱신: 2026-06-06 20:18 KST
- 기준: 현재 폴더에 저장 완료된 HTML 정본과 raw 원본
- 정기 업데이트: 08:00 / 16:00 / 00:00 KST

## 1. 저장 완료된 조사 자료

| 날짜 | 조사 주제 | 조사 수준 | 핵심 범위 | 산출물 |
| --- | --- | --- | --- | --- |
| 2026-06-06 | A2A / Agent2Agent Protocol | Deep Dive, 브리핑 + 기술 상세 | Google 공개, Linux Foundation 이관, A2A v1.0, MCP와의 차이, Agent Card, Task lifecycle, Artifact, 프로토콜 흐름, 보안/거버넌스, 카카오 Agent Registry와 Orchestrator 적용 방향 | `2026-06-06/A2A_Deep_Dive.html`, `2026-06-06/raw/A2A_Deep_Dive.md` |
| 2026-06-06 | Agent Trend Scout | Scout, 딥다이브 후보 발굴 | GeekNews, MarkTechPost, GitHub Blog, Reddit 커뮤니티를 훑어 Claude Code Dynamic Workflows, MCP Production Mess, GitHub Agent-Native UX, Agent Runtime Sandbox, Skills Marketplace, Agentic Web, Agentic Commerce Protocol Stack, Observability/Memory/Audit 후보를 정리 | `2026-06-06/Agent_Trend_Scout.html`, `2026-06-06/raw/Agent_Trend_Scout.md` |
| 2026-06-06 | 2000 Agent Trend Brief | 8시간 Brief, 수동 업데이트 | watchlist 기반 수동 업데이트. Claude Code Dynamic Workflows, MCP 2026-07-28 RC, AgentBound, SWE-Skills-Bench, COLLEAGUE.SKILL, GitHub Copilot App/gh skill, UCP/AP2/AG-UI를 Project / Paper Alert 중심으로 정리 | `2026-06-06/2000_agent_trend_brief.html`, `2026-06-06/raw/2000_agent_trend_brief.md` |
| 2026-06-06 | Selected Agent Project / Paper Deep Dive | Deep Dive, 브리핑 + 플랫폼 설계 후보 | 사용자가 지정한 1, 3, 4, 5, 7번 항목과 Hermes Agent, SkillNet 조사. Claude Code Dynamic Workflows, AgentBound, SWE-Skills-Bench, COLLEAGUE.SKILL, UCP/AP2/AG-UI, Hermes Agent, SkillNet을 workflow runtime, security boundary, skill infrastructure, service protocol 관점으로 정리 | `2026-06-06/Selected_Agent_Project_Paper_Deep_Dive.html`, `2026-06-06/raw/Selected_Agent_Project_Paper_Deep_Dive.md` |
| 2026-06-06 | UCP / Universal Commerce Protocol | Deep Dive, Agentic commerce protocol | commerce capability/discovery/checkout/order 표준. UCP roles, capability negotiation, `/.well-known/ucp`, REST/MCP/A2A/Embedded transport, AP2 결합, Google/Shopify/Universal Cart 업데이트, 카카오 선물하기/쇼핑/예약/Pay 적용 방향 | `2026-06-06/UCP_Deep_Dive.html`, `2026-06-06/raw/UCP_Deep_Dive.md` |
| 2026-06-06 | AP2 / Agent Payments Protocol | Deep Dive, Agent payment trust layer | Checkout Mandate, Payment Mandate, Verifiable Digital Credential, Trusted Surface, human-present/human-not-present 결제 위임, FIDO 표준화 흐름, Kakao Pay agent 결제 승인/감사/분쟁 증거 설계 | `2026-06-06/AP2_Deep_Dive.html`, `2026-06-06/raw/AP2_Deep_Dive.md` |
| 2026-06-06 | AG-UI / Agent-User Interaction Protocol | Deep Dive, Agent frontend interaction | agent backend와 user-facing app을 연결하는 event protocol. Lifecycle, text, tool call, state snapshot/delta, activity, interrupt, custom events, AG-UI와 A2-UI 차이, 카카오톡/카나나/커머스/CS UX event schema 적용 방향 | `2026-06-06/AG_UI_Deep_Dive.html`, `2026-06-06/raw/AG_UI_Deep_Dive.md` |
| 2026-06-06 | A2-UI / Agent-to-User Interface | Deep Dive, Generative UI spec | agent-generated declarative UI payload/spec. Surface, component, data model, action, catalog, validator, A2A/AG-UI/SSE/MCP transport, version status, Macaron-A2UI 논문, 카카오톡 native renderer와 Kakao component catalog 적용 방향 | `2026-06-06/A2_UI_Deep_Dive.html`, `2026-06-06/raw/A2_UI_Deep_Dive.md` |

## 2. 조사 깊이 기준

- Deep Dive: 특정 주제를 브리핑 판단과 실무 기술 설계까지 이어지도록 깊게 정리한 자료. 공식 출처, 기술 구조, 시장 신호, 카카오 적용 방향, 리스크, 권장 액션을 포함한다.
- 8시간 Brief: 새로 나온 트렌드와 기존 조사 업데이트를 빠르게 훑는 정기 브리프. 중요도, 변경점, 카카오 영향, 추적 리스크를 중심으로 정리한다.

## 3. 현재까지 설정된 지속 모니터링 범위

아래 범위는 아직 모두 개별 딥다이브로 저장된 것은 아니며, 8시간마다 새 소식과 기존 조사 업데이트를 확인하기로 설정된 추적 범위다.

| 트렌드 영역 | 조사 상태 | 확인할 내용 |
| --- | --- | --- |
| AI Agent Platforms | Monitoring | OpenAI, Google, Anthropic, Microsoft 등 플랫폼의 SDK, orchestration, hosted agent 기능 변화 |
| MCP | Monitoring | agent-to-tool 표준화, enterprise connector, 보안 모델, A2A와의 결합 패턴 |
| A2A | Deep Dive 완료 | agent-to-agent 표준화, Agent Card, Task lifecycle, signed card, enterprise security, 카카오 내부 호환 표준 |
| Agent Commerce / Payments | Monitoring | 에이전트가 구매, 예약, 결제를 수행하는 흐름과 승인/책임/정산 모델 |
| Agent Security / Governance | Monitoring | prompt injection, delegated authorization, audit trace, human approval, policy enforcement |
| Evals / Observability | Monitoring | 에이전트 선택, 작업 성공률, tool call 품질, tracing, regression eval, 운영 지표 |
| Memory / Personalization | Monitoring | 장기 기억, 개인화 context, privacy boundary, 사용자 제어권 |
| On-device / Multimodal / Voice Agents | Monitoring | 음성, 이미지, 화면 이해, 기기 내 실행, 모바일 UX, 카카오톡/디바이스 접점 |

## 4. 다음 업데이트 방식

- 새 HTML 보고서가 생성되면 이 조사 이력에 날짜, 주제, 조사 수준, 핵심 범위, 링크를 추가한다.
- 기존 주제가 업데이트되면 같은 날짜에 "기존 조사 업데이트" 항목으로 남긴다.
- 브리핑에 쓸 수 있는 수준의 자료는 `Deep Dive`, 빠른 정기 스캔은 `8시간 Brief`로 구분한다.
