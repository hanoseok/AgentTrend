# Agent Trend Brief

- 최종 갱신: 2026-06-28 07:00 KST
- HTML 정본: `report/agent-trend-brief/`
- 정렬 기준: 최신 작성 시각 내림차순
- 운영 슬롯: 매일 07:00 KST

## 1. Summary

Agent Trend Brief만 따로 모아 시간순으로 보는 페이지를 만들었다. 새 브리프가 생성되면 이 페이지의 Time Briefs에 최신순으로 추가하고, 개별 HTML과 raw 원본으로 바로 이동할 수 있게 관리한다.

- 가장 최근 브리프: `report/agent-trend-brief/2026-06-28-0700/`, `history/2026-06-28/0700_agent_trend_brief.md`
- 직전 브리프: `report/agent-trend-brief/2026-06-11-1103-manual/`, `history/2026-06-11/1103_agent_trend_manual.md`
- 운영 원칙: 메인 Reports는 브리프 허브를 대표 항목으로 보여주고, 각 시간대 브리프는 이 페이지 하위 목록에서 접근한다.

## 2. Time Briefs

| 작성 시각 | 브리프 | 핵심 신호 | 후속 액션 | 원본 |
| --- | --- | --- | --- | --- |
| 2026-06-28 07:00 KST | `report/agent-trend-brief/2026-06-28-0700/` | Agentic work, ARD/Agent Finder, Agent 365, identity/authorization, simulated eval, LangGraph 1.0 | Agent Registry / Discovery / Governance 딥다이브 후보 등록. Codex task horizon, Claude Code expertise, NIST identity model 후속 확인 | `history/2026-06-28/0700_agent_trend_brief.md` |
| 2026-06-11 11:03 KST | `report/agent-trend-brief/2026-06-11-1103-manual/` | Customer service, research/data analysis, internal productivity, IT/developer/security adoption | 고객지원과 리서치/데이터 분석을 benchmark use case로 잡고 분야별 agent registry/eval/approval 기준 정의 | `history/2026-06-11/1103_agent_trend_manual.md` |
| 2026-06-07 09:00 KST | `report/agent-trend-brief/2026-06-07-0900/` | Agent runtime/control plane, terminal coding agent, skill lifecycle, cloud execution, product shipping surface | Google AX deep dive 완료. Kimi Code CLI, Cline SDK, Task Observer는 다음 후보로 추적 | `history/2026-06-07/0900_agent_trend_brief.md` |
| 2026-06-06 20:00 KST | `report/agent-trend-brief/2026-06-06-2000/` | Dynamic Workflows, MCP update, AgentBound, skill eval papers, agentic commerce protocol stack | Dynamic Workflows, AgentBound, SWE-Skills-Bench, COLLEAGUE.SKILL, SkillNet, Hermes Agent를 개별 deep dive로 분리 | `history/2026-06-06/2000_agent_trend_brief.md` |

## 3. 2026-06-28

### 07:00 Agent Trend Brief

참고 링크: `report/agent-trend-brief/2026-06-28-0700/`, `history/2026-06-28/0700_agent_trend_brief.md`, `report/research-log/`, `source/`

- 핵심 신호: agent가 chat answer에서 long-horizon delegated task, registry/discovery, identity/governance, simulated eval 중심으로 이동한다.
- 확인된 사실: OpenAI Codex work 데이터, Anthropic Claude Code expertise, Google ARD, GitHub Agent Finder, Microsoft Agent 365, Patronus funding, NIST concept paper를 확인했다.
- 판단: agent job API, ARD/Agent Card registry, identity/authorization, review surface, expertise scaffold, eval/replay가 공통 layer가 되어야 한다.
- 후속: Agent Registry / Discovery / Governance 딥다이브와 Codex task horizon methodology, Claude Code onboarding checklist, NIST identity data model을 확인한다.

## 4. 2026-06-11

### 11:03 AI Agent 사용 분야 수동 브리프

참고 링크: `report/agent-trend-brief/2026-06-11-1103-manual/`, `history/2026-06-11/1103_agent_trend_manual.md`, `report/research-log/`, `source/`

- 핵심 질문: 사람들이 AI Agent를 어느 분야에 가장 많이 사용하는가.
- 결론: 기업 배포 기준 고객지원/컨택센터와 리서치/데이터 분석이 가장 강하고, 개인 사용은 조사/요약, 정보 탐색, 글쓰기, 생산성 보조가 중심이다.
- 판단: 고객지원과 리서치/데이터 분석을 첫 benchmark use case로 잡고, agent identity, tool permission, execution ledger, evidence/citation, domain eval을 공통 layer로 설계해야 한다.
- 후속: customer support agent rollback/incident와 agentic commerce/travel booking 사용량을 계속 확인한다.

## 5. 2026-06-07

### 09:00 Agent Trend Brief

참고 링크: `report/agent-trend-brief/2026-06-07-0900/`, `history/2026-06-07/0900_agent_trend_brief.md`, `report/google-ax-deep-dive/`, `source/`

- Project / Paper Alert: Google AX, Kimi Code CLI, Cline SDK/runtime, Task Observer.
- 신규 사항: Copilot Agent Tasks REST API, Copilot App canvas/cloud automation, enterprise-managed plugins, Colab CLI, Codex Sites.
- 판단: agent는 대화 UI보다 durable task, event log, worktree isolation, schedule, policy, canvas review surface로 제품화되는 흐름이 강하다.
- 후속: runtime/control-plane, terminal harness, skill governance를 같은 비교 축으로 계속 추적한다.

## 6. 2026-06-06

### 20:00 Agent Trend Brief

참고 링크: `report/agent-trend-brief/2026-06-06-2000/`, `history/2026-06-06/2000_agent_trend_brief.md`, `report/claude-code-dynamic-workflows/`, `report/agentbound-deep-dive/`, `report/skillnet-deep-dive/`

- Project / Paper Alert: Claude Code Dynamic Workflows, AgentBound, SWE-Skills-Bench, COLLEAGUE.SKILL, Hermes Agent, SkillNet.
- 프로토콜 축: UCP, AP2, AG-UI, A2-UI를 commerce/payment/UI protocol stack으로 연결했다.
- 판단: agent 플랫폼은 protocol interoperability, security boundary, skill lifecycle, front-end interaction protocol을 동시에 설계해야 한다.
- 후속: 주요 후보를 2026-06-07 개별 deep dive로 분리해 상세화했다.

## 7. Cadence

| 슬롯 | 목적 | 기록 방식 |
| --- | --- | --- |
| 07:00 KST | 하루 1회 정기 브리프. 밤사이 공식 발표, GitHub/Reddit/GeekNews 신호, 기존 deep dive 업데이트를 확인 | 새 HTML은 날짜 폴더에 저장하고 이 페이지 Time Briefs 최상단에 추가 |

## 출처

### 왜 이걸 정리하게 되었는가

정기 브리프가 개별 Reports 목록에 흩어지면 시간 흐름을 빠르게 보기 어렵다. 브리프만 별도 허브로 분리해 회차별 변화와 후속 딥다이브 연결을 한눈에 확인하도록 정리했다.

참고 링크: `index.html`, `history/`, `report/agent-trend-brief/2026-06-28-0700/`, `report/agent-trend-brief/2026-06-11-1103-manual/`, `report/agent-trend-brief/2026-06-07-0900/`, `report/agent-trend-brief/2026-06-06-2000/`

### 딥리서치 출처

각 시간대 브리프의 실제 세부 근거와 원문 링크는 개별 브리프 HTML 및 raw Markdown에 보관한다.

참고 링크: `history/2026-06-28/0700_agent_trend_brief.md`, `history/2026-06-11/1103_agent_trend_manual.md`, `history/2026-06-07/0900_agent_trend_brief.md`, `history/2026-06-06/2000_agent_trend_brief.md`, `source/`, `source/watchlist/`
