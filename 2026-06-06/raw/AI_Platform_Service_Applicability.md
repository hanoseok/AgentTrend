# AI 플랫폼 및 서비스 적용 포인트

- 작성 시점: 2026-06-06 23:38 KST
- 조사 수준: Cross-Report Synthesis
- HTML 정본: `2026-06-06/AI_Platform_Service_Applicability.html`
- 범위: 특정 회사명 제외. AI 플랫폼 및 사용자-facing 서비스에서 사용할 수 있는 점 중심.

## 1. Executive Summary

참고 링크: [Selected Deep Dive](../Selected_Agent_Project_Paper_Deep_Dive.html), [A2A Deep Dive](../A2A_Deep_Dive.html), [UCP Deep Dive](../UCP_Deep_Dive.html), [AP2 Deep Dive](../AP2_Deep_Dive.html), [AG-UI Deep Dive](../AG_UI_Deep_Dive.html), [A2-UI Deep Dive](../A2_UI_Deep_Dive.html)

핵심 판단:

> 에이전트 트렌드는 “모델이 더 똑똑해진다”보다 “권한 있는 실행자가 되는 agent를 플랫폼이 어떻게 통제, 연결, 평가, 표시, 결제/승인하게 만들 것인가”로 이동하고 있다.

- Runtime이 제품의 중심이다. 장시간 실행, subagent 병렬 처리, checkpoint, cancellation, verifier, cost cap이 없는 agent 서비스는 실사용 업무로 확장하기 어렵다.
- Registry는 catalog가 아니다. Agent, MCP tool, skill, UI component를 등록하는 기능은 permission, risk tier, audit, versioning, deprecation까지 포함해야 한다.
- UI는 채팅창을 넘어선다. AG-UI/A2-UI 흐름은 streaming, interrupt, shared state, 승인 UI, 선언형 component payload가 agent 앱의 기본 UX가 될 가능성을 보여준다.
- 결제/커머스는 별도 신뢰 계층이다. UCP/AP2는 agentic commerce가 추천 문제가 아니라 checkout, mandate, trusted surface, 책임 증거 문제임을 보여준다.
- Skill은 양보다 검증이다. SkillNet은 skill graph/registry 방향을, SWE-Skills-Bench는 무검증 skill marketplace의 위험을 보여준다.
- Persistent agent는 매력적이지만 위험하다. Hermes Agent류의 memory, gateway, cron, multi-provider routing은 제품 벤치마크 가치가 크지만 privacy와 권한 통제가 먼저다.

## 2. AI Platform / Service Layer Map

| 레이어 | 핵심 역할 | 관련 트렌드 | 플랫폼 산출물 | 초기 우선순위 |
| --- | --- | --- | --- | --- |
| User Surface | 대화형 서비스, 업무 도구, 커머스/예약/CS 화면에서 agent와 사용자 상호작용 제공 | AG-UI, A2-UI | Agent event stream, interrupt UX, declarative UI renderer | High |
| Agent Runtime | task decomposition, subagent orchestration, checkpoint, resume, cancellation, verifier 실행 | Dynamic Workflows, Hermes Agent | Workflow runtime, task graph, budget policy, model routing | High |
| Agent / Tool Registry | agent discovery, tool capability, skill, component, version, SLA, risk metadata 관리 | A2A, MCP, AgentBound, SkillNet | Agent Card registry, MCP manifest, skill card, component catalog | High |
| Commerce / Payment Trust | 구매, 예약, 주문, 결제, 취소/환불, 분쟁 대응을 agent-safe하게 처리 | UCP, AP2 | Capability profile, checkout mandate, payment mandate, trusted approval surface | Medium-High |
| Skill Infrastructure | 스킬 생성, 검색, 조합, 배포, 평가, 버전 호환성을 관리 | SkillNet, SWE-Skills-Bench, COLLEAGUE.SKILL | Skill graph, eval metadata, compatibility matrix, expert-skill policy | Medium |
| Governance / Observability | 권한, 감사, 비용, 품질, 정책 위반, 모델/도구별 책임 추적 | AgentBound, A2A Enterprise, AG-UI traces | Audit schema, trace viewer, risk policy, human approval policy | High |

## 3. Runtime: 실행형 Agent의 Control Plane

참고 링크: [Dynamic Workflows](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code), [open-dynamic-workflows](https://github.com/imsai-sh/open-dynamic-workflows), [Hermes Docs](https://hermes-agent.nousresearch.com/docs/user-guide/skills/bundled/autonomous-ai-agents/autonomous-ai-agents-hermes-agent), [Hermes GitHub](https://github.com/NousResearch/hermes-agent)

AI 플랫폼이 단순 챗봇을 넘어 업무 수행 agent를 만들려면 runtime이 먼저 필요하다. 모델이 계획을 세우더라도, 실행은 task graph, subagent pool, tool permission, checkpoint, verifier, retry policy, budget policy로 통제되어야 한다.

| 사용 가능한 점 | 왜 필요한가 | 구현 힌트 |
| --- | --- | --- |
| 장시간 task 실행 | 코드 마이그레이션, 운영 점검, 고객 이슈 처리, 문서 작성처럼 한 번의 답변으로 끝나지 않는 업무를 처리 | Task DAG, run id, checkpoint, resume/cancel API를 표준화 |
| Subagent 병렬화 | 탐색, 분석, 검증, 요약을 동시에 실행해 처리 시간을 줄이고 관점을 다양화 | subagent cap, per-branch budget, branch result verifier를 둔다 |
| Executable verifier | agent output은 말로 검증하지 말고 테스트, 빌드, 정책 체크, 스크린샷, schema validation으로 검증해야 한다 | verifier result를 Artifact로 남기고 다음 agent가 읽게 한다 |
| 비용/품질 라우팅 | 모든 subtask를 최고가 모델에 맡기면 비용이 급증한다 | task type, risk tier, expected value 기준의 model routing을 둔다 |

추천 PoC:

- 개발/운영 업무 하나를 골라 “계획 agent → 병렬 조사 subagents → verifier → human approval → 최종 artifact” 흐름을 30일 안에 구현한다.

## 4. Registry / Governance: Agent와 Tool을 안전하게 발견시키는 방법

참고 링크: [A2A Key Concepts](https://a2a-protocol.org/latest/topics/key-concepts/), [A2A Agent Discovery](https://a2a-protocol.org/latest/topics/agent-discovery/), [A2A Enterprise Ready](https://a2a-protocol.org/latest/topics/enterprise-ready/), [AgentBound PDF](https://programming-group.com/assets/pdf/papers/2026_AgentBound-Securing-Execution-Boundaries-of-AI-Agents.pdf)

A2A의 Agent Card와 AgentBound의 permission manifest 관점은 함께 봐야 한다. Agent discovery는 기능 소개가 아니라 “누가, 어떤 권한으로, 어떤 데이터를, 어떤 조건에서, 어떤 감사 로그를 남기며 실행할 수 있는가”를 정의하는 계약이다.

| Registry 항목 | 필수 metadata | 서비스 적용 의미 |
| --- | --- | --- |
| Agent Card | name, endpoint, skills, input/output modes, auth, owner, SLA | 내부/외부 agent를 발견하고 orchestrator가 task를 위임할 수 있다 |
| Tool / MCP Manifest | declared capabilities, file/network/secret/payment scope, rate limit, egress rule | tool server를 trust-by-default로 두지 않고 실행 전 정책 검사를 한다 |
| Risk Tier | read-only, write, external action, personal data, payment, irreversible action | 위험도별 자동 실행/사용자 승인/관리자 승인 정책을 분리한다 |
| Audit Schema | actor, model, tool, payload hash, approval id, result, rollback link | 분쟁, 보안 감사, 품질 회고가 가능해진다 |
| Deprecation / Version | supported versions, sunset date, compatibility, migration note | agent가 오래된 tool/skill을 잘못 호출하는 문제를 줄인다 |

## 5. Commerce / Payment: Agentic Action의 신뢰 계층

참고 링크: [UCP](https://ucp.dev/), [UCP Core Concepts](https://ucp.dev/documentation/core-concepts/), [UCP + AP2](https://ucp.dev/documentation/ucp-and-ap2/), [AP2](https://ap2-protocol.org/), [FIDO](https://fidoalliance.org/fido-alliance-to-develop-standards-for-trusted-ai-agent-interactions/)

구매/예약/결제 agent는 “추천 잘하기”보다 “사용자가 정확히 무엇을 승인했는가”가 더 중요하다. UCP는 merchant capability와 checkout/order lifecycle을, AP2는 mandate와 trusted surface를 통해 결제 권한과 책임 증거를 다룬다.

| 서비스 시나리오 | 필요한 표준형 구성 | 리스크 | 플랫폼 대응 |
| --- | --- | --- | --- |
| 상품 탐색과 비교 | catalog.search, catalog.lookup, real-time price/stock | 가격/재고 불일치, 추천 근거 불명확 | 상품 ID, timestamp, merchant source, comparison reason을 Artifact로 남긴다 |
| 장바구니와 checkout | cart, checkout, order capability profile | 옵션/배송지/할인/환불 조건 누락 | checkout summary와 user confirmation을 분리한다 |
| 결제 승인 | Checkout Mandate, Payment Mandate, trusted approval surface | LLM 생성 UI가 승인 범위를 왜곡할 수 있음 | 결제 서비스가 제어하는 deterministic approval UI만 mandate를 발행한다 |
| 예약/주문 후 처리 | order status, cancel, refund, support capability | 취소/변경 책임 소재 불명확 | order lifecycle event와 dispute evidence를 audit log에 연결한다 |

하지 말아야 할 것:

- agent가 생성한 채팅 문구만으로 결제를 확정하면 안 된다.
- 결제/개인정보/외부 발송 action을 모델 응답에서 바로 실행하면 안 된다.
- 모든 고위험 action은 interrupt와 trusted surface를 거쳐야 한다.

## 6. Agent UI: 채팅을 넘어선 Event / Declarative UI

참고 링크: [AG-UI Docs](https://docs.ag-ui.com/), [AG-UI Events](https://docs.ag-ui.com/concepts/events), [A2UI Intro](https://a2ui.org/introduction/what-is-a2ui/), [A2UI v0.9](https://a2ui.org/specification/v0_9/docs/a2ui_protocol/), [Macaron-A2UI](https://arxiv.org/abs/2605.24830)

사용자-facing agent 서비스는 텍스트 답변만으로 충분하지 않다. 장시간 실행 상태, tool call, 승인 요청, 취소/재시도, 비교표, 상품 카드, 예약 form, 증빙 업로드 UI가 agent run과 함께 움직여야 한다.

| UI 요구 | AG-UI 관점 | A2-UI 관점 | 적용 포인트 |
| --- | --- | --- | --- |
| 실행 상태 표시 | Run/Step lifecycle, activity, tool call event | 상태 card, progress surface | 사용자가 agent가 무엇을 하고 있는지 추적 |
| Human interrupt | 승인/수정/취소가 필요한 시점에 event로 중단 | ApprovalSummary, OptionSelector, ConfirmButton | 결제, 예약 확정, 개인정보 제공, 외부 발송을 안전하게 처리 |
| Shared state | StateSnapshot/Delta, JSON Patch | component data binding | 상품 비교, 장바구니, CS ticket 상태가 계속 업데이트 |
| 선언형 UI | transport/event stream | catalog 기반 component payload | agent가 임의 HTML을 만들지 않고 허용된 component intent만 생성 |

추천 PoC:

- AG-UI event stream 위에 A2-UI payload를 실어 “상품 비교 → 옵션 선택 → 결제 승인 interrupt → 주문 추적” 흐름을 하나의 대화형 UI로 만든다.

## 7. Skills: Marketplace보다 Eval Infrastructure

참고 링크: [SkillNet](https://arxiv.org/abs/2603.04448), [SkillNet GitHub](https://github.com/zjunlp/SkillNet), [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401), [SWE-Skills-Bench GitHub](https://github.com/GeniusHTX/SWE-Skills-Bench), [COLLEAGUE.SKILL](https://arxiv.org/abs/2605.31264)

SkillNet은 skill graph와 대규모 skill repository 방향을 보여주지만, SWE-Skills-Bench는 많은 public skill이 실제 pass-rate 개선을 만들지 못하거나 token cost만 늘릴 수 있음을 보여준다. 따라서 AI 플랫폼의 skill 전략은 “등록 수”가 아니라 “측정된 효용” 중심이어야 한다.

| 설계 원칙 | 구현 항목 | KPI |
| --- | --- | --- |
| Skill card 표준화 | purpose, supported versions, activation condition, required tools, risk tier | skill 오작동률, fallback 비율 |
| Task-specific eval | acceptance criteria, deterministic test, benchmark dataset, regression history | pass-rate 개선, time-to-complete, token cost |
| Compatibility matrix | 모델/도구/서비스 버전별 지원 여부와 실패 사례 | version mismatch로 인한 실패 감소 |
| Expert skill governance | trace 수집 동의, PII masking, IP, 삭제권, rollback, access control | 개인정보/권한 위반 0건, skill review SLA |

## 8. 30 / 60 / 90일 실행안

| 기간 | 목표 | 산출물 | 성공 기준 |
| --- | --- | --- | --- |
| 30일 | 통제 가능한 agent runtime과 registry 초안 확보 | Workflow runtime PoC, Agent Card schema, MCP manifest draft, audit schema, 1개 verifier | 하나의 업무 task를 end-to-end로 실행하고 trace, cost, approval, verifier result를 남김 |
| 60일 | 사용자-facing agent UX와 고위험 action interrupt 검증 | AG-UI-compatible event schema, A2-UI component catalog, approval surface prototype, skill card/eval schema | 결제/예약/개인정보 제공 같은 고위험 action을 interrupt 기반으로 중단/승인/취소 가능 |
| 90일 | 서비스 PoC를 production architecture 후보로 전환 | Agent registry beta, tool policy enforcement, commerce capability profile pilot, skill eval dashboard, trace viewer | 2개 이상 서비스 도메인에서 agent workflow, UI event, audit, policy, eval이 공통 플랫폼으로 동작 |

## 9. 우선순위 의사결정

1. 먼저 할 것: workflow runtime, Agent/Tool Registry, audit/approval policy.
2. 동시에 볼 것: AG-UI/A2-UI 기반 사용자 UX와 commerce/payment trusted surface.
3. 조심해서 할 것: skill marketplace, persistent memory, messaging gateway full-tool access.
4. 하지 말아야 할 것: 무검증 skill 대량 등록, LLM 생성 UI 기반 결제 확정, 권한 없는 MCP/tool server 실행, pre-v1 spec 전면 고정 도입.

## 10. Source Links

- Google A2A: https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/
- A2A Concepts: https://a2a-protocol.org/latest/topics/key-concepts/
- Dynamic Workflows: https://claude.com/blog/introducing-dynamic-workflows-in-claude-code
- AgentBound: https://programming-group.com/assets/pdf/papers/2026_AgentBound-Securing-Execution-Boundaries-of-AI-Agents.pdf
- UCP: https://ucp.dev/
- AP2: https://ap2-protocol.org/
- AG-UI: https://docs.ag-ui.com/
- A2UI: https://a2ui.org/
- SkillNet: https://arxiv.org/abs/2603.04448
- SWE-Skills-Bench: https://arxiv.org/abs/2603.15401
- COLLEAGUE.SKILL: https://arxiv.org/abs/2605.31264
- Hermes Agent: https://github.com/NousResearch/hermes-agent

## 출처

### 왜 이걸 정리하게 되었는가

- [A2A Deep Dive](A2A_Deep_Dive.html)
- [UCP Deep Dive](UCP_Deep_Dive.html)
- [AP2 Deep Dive](AP2_Deep_Dive.html)
- [AG-UI Deep Dive](AG_UI_Deep_Dive.html)
- [A2-UI Deep Dive](A2_UI_Deep_Dive.html)
- [Selected Overview](Selected_Agent_Project_Paper_Deep_Dive.html)

### 딥리서치 출처

- [Dynamic Workflows](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code)
- [A2A Key Concepts](https://a2a-protocol.org/latest/topics/key-concepts/)
- [UCP Core Concepts](https://ucp.dev/documentation/core-concepts/)
- [AP2](https://ap2-protocol.org/)
- [AgentBound PDF](https://programming-group.com/assets/pdf/papers/2026_AgentBound-Securing-Execution-Boundaries-of-AI-Agents.pdf)
- [SkillNet](https://arxiv.org/abs/2603.04448)
- [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401)
