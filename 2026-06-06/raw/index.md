# Agent Trend 전체 인덱스

- 최종 갱신: 2026-06-06 21:28 KST
- HTML 정본: `index.html`
- raw 원본: `2026-06-06/raw/index.md`
- 정기 업데이트: 00:00 / 08:00 / 16:00 KST

## 1. Quick Read

- 지금 가장 중요한 축: Dynamic Workflows + AgentBound. agent가 더 많은 일을 병렬로 실행할수록 workflow runtime, cost cap, checkpoint, human approval, permission boundary가 같이 필요하다.
- 카카오 사업 연결 축: UCP + AP2 + AG-UI + A2-UI. 톡/선물하기/쇼핑/예약/Pay를 agentic commerce로 묶으려면 commerce, payment, UI event, declarative UI가 함께 필요하다.
- 플랫폼 품질 축: SkillNet + SWE-Skills-Bench + COLLEAGUE.SKILL. 스킬은 많이 모으는 것이 아니라 task 개선, token cost, version compatibility, governance까지 검증해야 한다.

브리핑 시작점:

1. `2026-06-06/2000_agent_trend_brief.html`
2. `2026-06-06/Selected_Agent_Project_Paper_Deep_Dive.html`
3. `2026-06-06/UCP_Deep_Dive.html`
4. `2026-06-06/AP2_Deep_Dive.html`

## 2. Protocol / Platform Map

| 레이어 | 관련 자료 | 무엇을 해결하나 | 카카오 관점 |
| --- | --- | --- | --- |
| Agent-to-Agent | `2026-06-06/A2A_Deep_Dive.html` | agent discovery, task delegation, Agent Card, task lifecycle, artifact 교환 | 카카오 내부/외부 agent registry와 orchestrator의 표준 후보 |
| Commerce | `2026-06-06/UCP_Deep_Dive.html` | 상품 탐색, cart, checkout, order, identity linking, merchant capability discovery | 선물하기, 쇼핑, 예약, 지도/로컬, 채널 파트너를 agent-readable commerce로 전환 |
| Payment Trust | `2026-06-06/AP2_Deep_Dive.html` | 결제 위임, 사용자 의도 증명, mandate, trusted surface, dispute evidence | Kakao Pay agent 결제의 승인 UX, 감사, 분쟁 대응 기준 |
| Agent UI Events | `2026-06-06/AG_UI_Deep_Dive.html` | agent run lifecycle, text streaming, tool call, state delta, interrupt, custom event | 카카오톡/카나나/CS/커머스 agent UX의 공통 event schema |
| Generative UI | `2026-06-06/A2_UI_Deep_Dive.html` | agent가 안전한 선언형 UI payload를 만들고 client가 native UI로 렌더링 | 톡 말풍선 안의 상품 카드, 예약 form, 승인 summary, CS action UI |
| Security / Skills / Runtime | `2026-06-06/Selected_Agent_Project_Paper_Deep_Dive.html` | Dynamic Workflows, AgentBound, SWE-Skills-Bench, COLLEAGUE.SKILL, Hermes Agent, SkillNet | workflow runtime, MCP permission boundary, skill registry/eval, persistent agent governance |

## 3. Reading Paths

### 브리핑 / 전략

1. `2026-06-06/2000_agent_trend_brief.html`
2. `2026-06-06/Selected_Agent_Project_Paper_Deep_Dive.html`
3. `2026-06-06/UCP_Deep_Dive.html`
4. `2026-06-06/AP2_Deep_Dive.html`

### Agent Platform 설계

1. `2026-06-06/A2A_Deep_Dive.html`
2. `2026-06-06/Selected_Agent_Project_Paper_Deep_Dive.html`
3. `2026-06-06/AG_UI_Deep_Dive.html`

### 커머스 / 결제 / Pay

1. `2026-06-06/UCP_Deep_Dive.html`
2. `2026-06-06/AP2_Deep_Dive.html`
3. `2026-06-06/A2_UI_Deep_Dive.html`

### UI / Product UX

1. `2026-06-06/AG_UI_Deep_Dive.html`
2. `2026-06-06/A2_UI_Deep_Dive.html`
3. `2026-06-06/UCP_Deep_Dive.html`

## 4. Reports

| 날짜 | 문서 | 유형 | 핵심 용도 |
| --- | --- | --- | --- |
| 2026-06-06 | `2026-06-06/A2A_Deep_Dive.html` | Deep Dive | agent-to-agent 표준과 카카오 agent registry/orchestrator 설계 |
| 2026-06-06 | `2026-06-06/Agent_Trend_Scout.html` | Scout | GeekNews, MarkTechPost, GitHub Blog, Reddit 기반 딥다이브 후보 발굴 |
| 2026-06-06 | `2026-06-06/2000_agent_trend_brief.html` | 8시간 Brief | 정기 브리프 형식의 트렌드/업데이트 요약 |
| 2026-06-06 | `2026-06-06/Selected_Agent_Project_Paper_Deep_Dive.html` | Deep Dive | Dynamic Workflows, AgentBound, skills, commerce stack, Hermes Agent, SkillNet 묶음 |
| 2026-06-06 | `2026-06-06/UCP_Deep_Dive.html` | Deep Dive | agentic commerce capability/discovery/checkout/order 표준 |
| 2026-06-06 | `2026-06-06/AP2_Deep_Dive.html` | Deep Dive | agent 결제 위임, mandate, trusted surface, dispute evidence |
| 2026-06-06 | `2026-06-06/AG_UI_Deep_Dive.html` | Deep Dive | agent frontend event protocol, interrupt, state delta, tool call UX |
| 2026-06-06 | `2026-06-06/A2_UI_Deep_Dive.html` | Deep Dive | agent-generated declarative UI payload/spec와 카카오 component catalog 설계 |

## 5. Raw Sources

- `2026-06-06/raw/A2A_Deep_Dive.md`
- `2026-06-06/raw/Agent_Trend_Scout.md`
- `2026-06-06/raw/2000_agent_trend_brief.md`
- `2026-06-06/raw/Selected_Agent_Project_Paper_Deep_Dive.md`
- `2026-06-06/raw/UCP_Deep_Dive.md`
- `2026-06-06/raw/AP2_Deep_Dive.md`
- `2026-06-06/raw/AG_UI_Deep_Dive.md`
- `2026-06-06/raw/A2_UI_Deep_Dive.md`

## 6. Operations

- `RESEARCH_LOG.html`: 날짜별 조사 이력.
- `SOURCE_WATCHLIST.html`: 정기 확인 소스 목록.
- `REPORTING_STANDARD.html`: 브리핑과 내부 공유 작성 원칙.
