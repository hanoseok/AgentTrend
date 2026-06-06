# 선정 에이전트 프로젝트와 논문 딥다이브

- 작성 시점: 2026-06-06 20:13 KST
- 조사 수준: Deep Dive
- 대상: 사용자가 지정한 1, 3, 4, 5, 7번 항목 + Hermes Agent + SkillNet
- HTML 정본: `2026-06-06/Selected_Agent_Project_Paper_Deep_Dive.html`

## 1. Executive Summary

참고 링크: [Dynamic Workflows](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code), [AgentBound](https://programming-group.com/assets/pdf/papers/2026_AgentBound-Securing-Execution-Boundaries-of-AI-Agents.pdf), [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401), [UCP](https://ucp.dev/), [AG-UI](https://docs.ag-ui.com/)

사용자가 지정한 7개 항목을 묶어 조사한 결론은 명확하다. 에이전트 플랫폼 경쟁은 모델 품질만이 아니라 workflow runtime, permission boundary, skill registry, eval, commerce/payment, UI protocol을 누가 먼저 제품화하느냐의 싸움으로 이동 중이다.

브리핑 한 줄:

> 에이전트 산업은 “똑똑한 챗봇”에서 “권한 있는 소프트웨어 실행자”로 넘어가고 있으며, 승부처는 모델보다 agent runtime, 보안 경계, 스킬 유통, 결제/승인/감사 체계가 될 가능성이 높다.

핵심 판단:

- Claude Code Dynamic Workflows는 “하나의 agent가 대화로 일하는 방식”에서 “agent가 workflow code를 만들고 다수 subagent를 orchestration하는 방식”으로 전환했다.
- AgentBound는 MCP 서버를 trust-by-default로 두는 것이 위험하다는 점을 실험과 정책 모델로 보여준다.
- SkillNet과 COLLEAGUE.SKILL은 스킬의 생성, 축적, 개인화, 배포 방향을 보여주지만 SWE-Skills-Bench는 public SWE skills 대부분이 실제 개선을 주지 못한다고 보고한다.
- UCP/AP2/AG-UI는 대화형 AI 서비스, 커머스, 쇼핑, 예약, 결제, 로컬 서비스에서 agentic commerce의 직접적인 사업 기회다.
- Hermes Agent는 persistent personal agent, memory, skills, gateway, provider-agnostic routing이 하나로 묶이는 제품 패턴을 보여준다.

## 2. Priority Matrix

| 항목 | 트렌드 강도 | AI 플랫폼 관련성 | 지금 볼 이유 | 추천 대응 |
| --- | --- | --- | --- | --- |
| 1. Claude Code Dynamic Workflows / ultracode | Very High | 개발자 agent, 사내 업무 agent, 장시간 task 자동화 | 공식 발표, Reddit 반응, token burn 논쟁, 대규모 코드 마이그레이션 사례가 동시에 강함 | 내부 workflow runtime PoC를 즉시 설계. cost cap, checkpoint, approval, trace를 기본 요구사항으로 둠 |
| 3. AgentBound | Very High | MCP registry, enterprise connector, agent sandbox | MCP 서버가 OS, 파일, 네트워크, secret에 접근하는 구조가 보편화되며 보안 경계가 필요해짐 | MCP permission manifest와 runtime policy engine을 플랫폼 baseline으로 검토 |
| 4. SWE-Skills-Bench | Very High | skill marketplace, coding agent, 내부 생산성 도구 | 스킬 주입이 항상 성능을 올린다는 가정에 반례를 제시 | 모든 skill 등록 전에 task fit, token cost, version compatibility, pass-rate 개선을 측정 |
| 5. COLLEAGUE.SKILL | High | 사내 expert skill, CS/운영/개발 노하우 이전 | 사람의 작업 trace를 inspectable, updateable skill로 바꾸는 방향이 분명함 | 개인정보, 동의, 지식재산권, 권한 경계를 포함한 expert skill 정책을 먼저 설계 |
| 7. UCP / AP2 / AG-UI | Very High | 대화형 AI 서비스, 커머스, 쇼핑, 예약, 결제, 로컬 서비스 | agentic commerce가 추천에서 결제, 주문, 사후처리로 확장 중 | 대화형 AI 서비스/결제 승인 UI 기반의 agent approval UX와 audit trail을 설계 |
| Hermes Agent | High | persistent personal agent, self-hosted internal assistant | memory, skills, gateway, provider-agnostic 실행을 묶어 “지속 실행 agent” 패턴을 보여줌 | 오픈소스 agent OS 관점에서 구조를 벤치마크. 사내 적용 시 memory/privacy/permission을 먼저 제한 |
| SkillNet | High | skill registry, skill graph, skill eval infra | 스킬을 재사용 가능한 자산으로 만들기 위한 ontology, 관계 그래프, 다차원 평가를 제안 | skill registry의 데이터 모델 후보로 검토하되, SWE-Skills-Bench 방식의 실효성 검증을 같이 붙임 |

## 3. Platform Reading

이번 7개 항목은 흩어진 뉴스가 아니라 하나의 플랫폼 아키텍처로 이어진다.

1. Workflow Runtime
   - 관련: Dynamic Workflows, Hermes Agent
   - 의미: agent가 task를 쪼개고 subagent를 만들고, 장시간 실행하고, 결과를 검증한다.

2. Security Boundary
   - 관련: AgentBound
   - 의미: MCP/A2A/tool server가 파일, 네트워크, secret, 결제 권한을 어떻게 제한받는지 결정한다.

3. Skill Infrastructure
   - 관련: SkillNet, SWE-Skills-Bench, COLLEAGUE.SKILL
   - 의미: 스킬 생성, 평가, 버전관리, 호환성, 개인화, 배포를 관리한다.

4. Service Protocols
   - 관련: UCP/AP2/AG-UI
   - 의미: 사용자가 agent에게 구매, 예약, 결제, 승인, UI 조작을 맡기는 흐름을 표준화한다.

## 4. 1번: Claude Code Dynamic Workflows / ultracode

참고 링크: [Anthropic Blog](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code), [open-dynamic-workflows](https://github.com/imsai-sh/open-dynamic-workflows), [Reddit r/ClaudeCode](https://www.reddit.com/r/ClaudeCode/comments/1tq9pge/introducing_dynamic_workflows_in_claude_code/)

### 무엇인가

Anthropic이 2026-05-28 공개한 Claude Code 기능이다. Claude가 동적으로 orchestration script를 만들고, 큰 작업을 subtasks로 나눈 뒤 tens to hundreds of parallel subagents를 실행한다. Claude Code CLI, Desktop, VS Code extension, API, Bedrock, Vertex AI, Microsoft Foundry 등에서 research preview로 제공된다고 공식 블로그가 설명한다.

### 왜 트렌디한가

- agent가 더 이상 한 번의 대화 context 안에서만 일하지 않고, workflow code를 만들어 외부 control plane에서 실행한다.
- 대규모 legacy codebase 마이그레이션, bug hunt, 보안 audit, independent verification 같은 병렬성이 큰 작업을 겨냥한다.
- Anthropic은 Bun Zig-to-Rust port 사례에서 약 750,000 lines, 11 days, 99.8% test pass를 사례로 제시했다. 이 수치는 공식 발표의 사례이며, production 적용 여부와 재현성은 별도 확인이 필요하다.
- 커뮤니티에서는 “코드로 control flow, 모델로 judgment”라는 방향에 동의하는 반응과 함께 token 사용량, 실패 경계, human review signal에 대한 우려가 동시에 나온다.

### AI 플랫폼/서비스에서 봐야 할 점

| 기능 요구 | 설명 | 초기 구현 방향 |
| --- | --- | --- |
| Workflow runtime | 장시간 agent task를 subtask graph로 쪼개고 실행 상태를 저장 | Task DAG, subagent pool, checkpoint/resume, cancellation 지원 |
| Cost and quota control | Dynamic workflow는 token 사용량이 일반 session보다 크다는 점이 공식 경고와 커뮤니티 논쟁의 핵심 | workflow별 예산, subagent cap, tool call cap, model routing, budget exhaustion policy |
| Verification layer | 복수 agent가 만든 결과를 그대로 merge하면 위험 | test/build/screenshot/security check 같은 executable verifier 우선 |
| Human approval | 실패한 branch가 계속 진행되면 피해가 커짐 | high-risk action 전 중단, 승인, 수정, 재시도 UI |

판단: 이 항목은 가장 먼저 깊게 파야 한다. AI agent platform이 “상담/검색형 agent”를 넘어 “업무 수행형 agent”가 되려면 dynamic workflow와 같은 control plane이 필요하다.

## 5. 3번: AgentBound

참고 링크: [AgentBound PDF](https://programming-group.com/assets/pdf/papers/2026_AgentBound-Securing-Execution-Boundaries-of-AI-Agents.pdf), [FSE page](https://conf.researchr.org/details/fse-2026/fse-2026-research-papers/14/AgentBound-Securing-Execution-Boundaries-of-AI-Agents)

### 무엇인가

AgentBound는 AI agent 실행 경계, 특히 MCP server의 권한 문제를 다루는 FSE 2026 논문이다. 핵심은 MCP 서버가 필요한 resource access를 manifest로 선언하고, runtime policy enforcement engine이 파일, 네트워크, secret 등 실제 접근을 제한하는 구조다.

### 핵심 기술 신호

- 논문은 MCP 서버가 trust-by-default로 실행될 때 privilege escalation, data tampering, exfiltration, tool poisoning, rug pull 공격이 가능하다고 본다.
- AgentBound는 Android permission model에 가까운 capability declaration 방식을 제안한다.
- 296개 popular MCP server dataset을 수집해 평가했다고 밝힌다.
- 개발자 리뷰를 받은 자동 생성 manifest 중 80.9%가 수정 없이 정확했고, 정책 vocabulary가 필요한 capabilities를 포괄한다고 보고한다.
- 악성 MCP server 실험에서 system resource attack과 data exfiltration을 완화했고, 평균 0.6ms 수준의 제한적 overhead를 보고한다.

### AI 플랫폼/서비스에서 봐야 할 점

MCP/A2A를 조직 내부와 외부 파트너 agent에 연결하면, tool server는 곧 AI 플랫폼 자산에 접근하는 실행 단위가 된다. 따라서 registry가 단순 catalog이면 부족하다. AI 플랫폼용 MCP registry에는 최소한 아래 필드가 필요하다.

- `declared_capabilities`: filesystem, network, secret, database, payment, user profile, location 등.
- `runtime_policy`: 허용 endpoint, file path, credential scope, rate limit, data egress rule.
- `risk_tier`: read-only, write, external action, payment, personal data access.
- `approval_policy`: 자동 실행 가능 여부, 사용자 승인 필요 여부, 관리자 승인 필요 여부.
- `audit_schema`: 어떤 action과 payload를 남길지.

판단: AgentBound는 논문이지만 바로 제품 요구사항으로 번역 가능하다. AI agent platform에서 MCP connector를 허용하려면 permission manifest와 enforcement는 “나중에 보안 강화”가 아니라 첫 설계 요구사항이어야 한다.

## 6. 4번: SWE-Skills-Bench

참고 링크: [arXiv](https://arxiv.org/abs/2603.15401), [GitHub](https://github.com/GeniusHTX/SWE-Skills-Bench)

### 무엇인가

SWE-Skills-Bench는 agent skill이 실제 software engineering task에 도움이 되는지 분리 측정한 벤치마크 논문이다. 49개 public SWE skills, pinned GitHub repositories, acceptance criteria가 있는 requirement documents, deterministic tests를 사용해 skill 주입 전후의 marginal utility를 비교한다.

### 왜 중요한가

- 논문은 49개 public SWE skills 중 39개가 pass-rate 개선을 만들지 못했다고 보고한다.
- 평균 개선은 +1.2%로 제한적이며, token overhead는 unchanged pass rate에서 최대 451% 증가할 수 있다고 보고한다.
- 일부 specialized skills는 최대 +30% 개선을 보였지만, version-mismatched guidance는 최대 -10% 성능 저하를 만들었다.
- 결론적으로 skill은 범용 만능 지식팩이 아니라 domain fit, abstraction level, project compatibility에 강하게 의존한다.

### AI 플랫폼/서비스에서 봐야 할 점

| Skill Marketplace 가정 | SWE-Skills-Bench가 주는 경고 | AI 플랫폼 설계 원칙 |
| --- | --- | --- |
| 스킬이 많을수록 좋다 | 많은 public skills는 성능 개선이 없었다 | 등록 수가 아니라 검증된 task coverage를 KPI로 둔다 |
| 좋은 설명은 좋은 실행을 만든다 | 추상적이거나 outdated guidance는 오히려 방해될 수 있다 | skill마다 supported versions, repository patterns, failure cases를 기록 |
| 스킬은 무료 context다 | token overhead가 큰 경우가 있다 | skill activation cost, expected benefit, retrieval threshold를 평가 |

## 7. 5번: COLLEAGUE.SKILL

참고 링크: [arXiv](https://arxiv.org/abs/2605.31264)

### 무엇인가

COLLEAGUE.SKILL은 사람 또는 role의 작업 trace를 agent가 사용할 수 있는 versioned skill package로 distill하는 시스템을 제안한다. 논문은 capability track과 bounded behavior track을 나눠, 업무 방식, 판단 휴리스틱, 커뮤니케이션 스타일, correction history를 inspectable, updateable, rollback 가능한 skill로 만들 수 있다고 설명한다.

### 왜 트렌디한가

- memory, persona, skill framework가 하나로 수렴하는 방향을 보여준다.
- agent가 단순 지식 검색이 아니라 특정 사람이나 role의 업무 방식을 재현하려는 시도가 늘고 있다.
- 기업에서는 “A팀의 운영 노하우”, “리드 개발자의 리뷰 관점”, “CS 우수 상담원의 응대 방식”을 agent에 이식하려는 요구가 생길 수밖에 없다.

### AI 플랫폼/서비스 적용 가능성

- 개발: 코드리뷰, 장애 대응, 배포 점검, 레거시 시스템 운영 스킬.
- 서비스 운영: 정책 위반 판단, 고객 문의 분류, escalation 판단, 공지 작성.
- 콘텐츠/커뮤니티: 톤앤매너, safety 판단, moderation playbook.
- 비즈니스: 광고/커머스 운영 rule, 파트너 대응, 상품 추천 가이드.

위험: 개인의 trace를 skill로 만드는 순간 privacy, consent, labor/IP, 평가/감시 이슈가 생긴다. AI 플랫폼에서 이 방향을 검토한다면 기술 PoC보다 먼저 동의, anonymization, PII masking, 접근권한, 삭제권, rollback 정책이 필요하다.

## 8. 7번: UCP / AP2 / AG-UI

참고 링크: [UCP](https://ucp.dev/), [AP2](https://ap2-protocol.org/), [AG-UI](https://docs.ag-ui.com/)

### 무엇인가

이 항목은 하나의 프로젝트가 아니라 agentic commerce와 user-facing agent app을 구성하는 protocol stack이다.

- UCP: agent와 merchant가 discovery, checkout, order, post-purchase support를 interoperable하게 처리하도록 하는 commerce protocol.
- AP2: agent가 결제할 때 authorization, authenticity, accountability를 verifiable credential과 mandate로 다루는 payment protocol.
- AG-UI: agent backend와 user-facing frontend 사이의 event-based interaction protocol.

### 왜 AI 플랫폼/서비스에 특히 중요한가

- AI 플랫폼은 대화형 AI 서비스, 커머스, 쇼핑, 예약, 로컬, 결제, 알림, 채널이라는 agentic commerce 접점을 이미 갖고 있다.
- agent가 추천만 하는 시대에는 UX가 중요했지만, agent가 구매/예약/결제/취소/반품을 수행하면 authorization과 audit이 제품의 핵심이 된다.
- AG-UI는 agent UX가 단순 채팅창을 넘어 shared state, interrupt, frontend tool call, backend tool rendering, subagent trace를 포함해야 한다는 점을 보여준다.

### 프로토콜별 의미

| 프로토콜 | 핵심 역할 | AI 플랫폼 관점의 질문 |
| --- | --- | --- |
| UCP | agent가 merchant capability를 이해하고 상품 검색, checkout, 주문 처리를 표준 방식으로 수행 | AI 플랫폼 커머스와 파트너 merchant가 agent-readable capability profile을 제공할 것인가? |
| AP2 | 사용자 의도와 결제 승인을 mandate/VDC로 증명하고 transaction accountability를 만든다 | 결제 서비스 agent 결제에서 “사용자가 무엇을 언제 어디까지 승인했는가”를 cryptographic audit로 남길 것인가? |
| AG-UI | agent와 앱 UI 사이의 streaming, interrupt, shared state, tool rendering, subagent composition을 표준화 | 대화형 AI 서비스 agent UX가 단순 말풍선이 아니라 승인, 비교, 수정, 취소, 추적 가능한 작업 UI를 제공할 것인가? |

판단: UCP/AP2/AG-UI는 AI 플랫폼 입장에서 단순 해외 표준 추적이 아니라 사업 기회다. agentic commerce에서 “conversational surface 기반 승인 UX + 결제 기반 mandate + 파트너 merchant protocol”을 선점할 수 있다.

## 9. Hermes Agent

참고 링크: [Hermes Docs](https://hermes-agent.nousresearch.com/docs/user-guide/skills/bundled/autonomous-ai-agents/autonomous-ai-agents-hermes-agent), [GitHub](https://github.com/NousResearch/hermes-agent)

### 무엇인가

Nous Research의 Hermes Agent는 terminal, messaging platforms, IDE에서 실행되는 open-source AI agent framework다. 공식 문서는 Claude Code, Codex, OpenClaw와 같은 autonomous coding/task-execution agent 범주로 설명하며, provider-agnostic model support, skills, persistent memory, multi-platform gateway, profiles, plugins, MCP servers, webhook triggers, cron scheduling을 특징으로 내세운다.

### 왜 봐야 하는가

- “one-shot assistant”가 아니라 지속 실행되는 personal digital employee 패턴을 보여준다.
- skills와 memory를 agent 성장 방식으로 묶고, Telegram/Discord/Slack/WhatsApp/Signal/Matrix/Email 같은 gateway를 통해 사용 접점을 확장한다.
- provider-agnostic 구조는 AI 플랫폼이 자체 모델, 외부 LLM, local model, specialized model을 라우팅하는 구조와 맞닿아 있다.
- 오픈소스 agent가 빠르게 기능을 붙이는 속도는 제품팀에 좋은 benchmark지만, enterprise 적용에는 강한 통제가 필요하다.

### AI 플랫폼/서비스가 배울 점과 조심할 점

| 배울 점 | 주의점 |
| --- | --- |
| memory, skills, gateway, cron, MCP를 하나의 personal agent experience로 묶는 제품 감각 | persistent memory에는 개인정보, 사내 비밀, long-term profiling 문제가 생긴다 |
| 여러 model provider를 workflow 중 바꿀 수 있는 routing 구조 | 모델별 security, logging, data retention, cost policy를 통합해야 한다 |
| messaging gateway로 agent를 일상 채널에 붙이는 방식 | 메신저형 서비스에서 full tool access를 주면 승인 UX와 abuse 대응이 핵심이 된다 |

판단: Hermes Agent는 조직 내부 업무 assistant와 대화형 AI 서비스형 persistent agent를 상상할 때 좋은 참고 사례다. 다만 그대로 도입할 대상이라기보다, “어떤 기능 묶음이 사용자에게 agent처럼 느껴지는가”를 보기 위한 product benchmark로 봐야 한다.

## 10. SkillNet

참고 링크: [arXiv](https://arxiv.org/abs/2603.04448), [GitHub](https://github.com/zjunlp/SkillNet), [Project site](https://skillnet.openkg.cn)

### 무엇인가

SkillNet은 AI skills를 생성, 평가, 연결하기 위한 open infrastructure를 제안한 2026년 arXiv 논문이다. 논문은 heterogeneous source에서 skill을 만들고, unified ontology로 관계를 연결하며, Safety, Completeness, Executability, Maintainability, Cost-awareness 기준으로 다차원 평가하는 구조를 제안한다. SkillNet은 200,000개 이상의 skills repository, interactive platform, Python toolkit을 포함한다고 설명한다.

### 왜 중요한가

- skill을 prompt 조각이 아니라 재사용 가능한 software asset으로 다룬다.
- skill 간 관계, prerequisite, composition, versioning, evaluation metadata가 중요해진다.
- ALFWorld, WebShop, ScienceWorld 실험에서 average reward 40% 향상, execution steps 30% 감소를 보고한다. 단, 이 결과는 해당 benchmark 환경 기준이며 실제 업무 task에는 별도 검증이 필요하다.

### SWE-Skills-Bench와 함께 읽어야 하는 이유

SkillNet은 “스킬을 대규모로 만들고 연결할 수 있다”는 공급 측 논리를 보여준다. SWE-Skills-Bench는 “그 스킬이 실제 software task에 도움이 되는지는 별도 검증해야 한다”는 수요 측 반론을 제시한다. 두 논문을 함께 읽으면 AI 플랫폼 skill platform의 설계 원칙이 분명해진다.

- 스킬은 graph와 registry가 필요하다.
- 하지만 registry의 핵심 metadata는 popularity가 아니라 measured utility여야 한다.
- skill activation은 retrieval 문제이면서 cost optimization 문제다.
- 서비스별 skill은 “공통 skill + 도메인 skill + 사용자/조직별 skill” 구조로 나누는 것이 적합하다.

## 11. Platform Actions

### 우선순위

1. Dynamic Workflow PoC: 개발/운영 업무 하나를 골라 subagent workflow, checkpoint, cost cap, verifier를 붙인 내부 prototype을 만든다.
2. MCP Permission Baseline: 모든 MCP/tool connector에 manifest, risk tier, policy enforcement, audit schema를 요구하는 draft spec을 만든다.
3. Skill Registry with Eval: SkillNet식 registry를 참고하되 SWE-Skills-Bench식 deterministic/acceptance-test 기반 eval을 필수 metadata로 넣는다.
4. Expert Skill Governance: COLLEAGUE.SKILL류 trace-to-skill을 검토하기 전에 동의, PII masking, IP, 삭제권, 접근권한 정책을 정리한다.
5. Agentic Commerce UX: UCP/AP2/AG-UI를 기준으로 대화형 AI 서비스/결제/커머스에서 approval, mandate, audit, cancel/retry, dispute flow를 설계한다.
6. Persistent Agent Benchmark: Hermes Agent를 reference로 memory, skills, gateway, cron, multi-provider routing의 제품/보안 요구사항을 추출한다.

### 30일 안에 할 일

| 워크스트림 | 30일 산출물 | 성공 기준 |
| --- | --- | --- |
| Workflow Runtime | subagent task graph prototype, workflow budget/cancel/checkpoint spec | 한 개 사내 개발/운영 task를 end-to-end로 실행하고 trace를 남김 |
| Security Boundary | MCP connector manifest draft, policy enforcement PoC | filesystem/network/secret access를 allow/deny로 통제하고 audit log 확인 |
| Skill Eval | skill card schema, eval result schema, compatibility metadata | 최소 10개 내부 skill에 대해 pass-rate, token cost, failure mode를 기록 |
| Commerce UX | agent approval wireflow, AP2-style mandate concept note | 사용자 승인 범위, 결제 증빙, 취소/분쟁 흐름을 한 화면 흐름으로 설명 가능 |

### 다음 딥다이브 추천

가장 먼저 더 깊게 파야 할 주제는 Dynamic Workflows + AgentBound 조합이다. 이유는 agent가 더 많은 일을 병렬로 실행할수록 권한 경계와 감사가 없으면 위험도도 같이 커지기 때문이다. 두 번째는 SkillNet + SWE-Skills-Bench + COLLEAGUE.SKILL 묶음이다. AI 플랫폼이 skill marketplace를 만든다면 이 세 개가 바로 제품 요구사항과 평가 기준으로 연결된다.

## 12. Sources

- Anthropic Claude Blog - Introducing dynamic workflows in Claude Code: https://claude.com/blog/introducing-dynamic-workflows-in-claude-code
- GitHub - open-dynamic-workflows: https://github.com/imsai-sh/open-dynamic-workflows
- AgentBound PDF - Securing Execution Boundaries of AI Agents: https://programming-group.com/assets/pdf/papers/2026_AgentBound-Securing-Execution-Boundaries-of-AI-Agents.pdf
- FSE 2026 AgentBound program page: https://conf.researchr.org/details/fse-2026/fse-2026-research-papers/14/AgentBound-Securing-Execution-Boundaries-of-AI-Agents
- arXiv 2603.15401 - SWE-Skills-Bench: https://arxiv.org/abs/2603.15401
- GitHub - SWE-Skills-Bench: https://github.com/GeniusHTX/SWE-Skills-Bench
- arXiv 2605.31264 - COLLEAGUE.SKILL: https://arxiv.org/abs/2605.31264
- arXiv 2603.04448 - SkillNet: https://arxiv.org/abs/2603.04448
- GitHub - zjunlp/SkillNet: https://github.com/zjunlp/SkillNet
- SkillNet project site: https://skillnet.openkg.cn
- Hermes Agent official docs: https://hermes-agent.nousresearch.com/docs/user-guide/skills/bundled/autonomous-ai-agents/autonomous-ai-agents-hermes-agent
- GitHub - NousResearch/hermes-agent: https://github.com/NousResearch/hermes-agent
- Universal Commerce Protocol: https://ucp.dev/
- Agent Payments Protocol: https://ap2-protocol.org/
- AG-UI documentation: https://docs.ag-ui.com/
- Reddit r/ClaudeCode - Dynamic workflows discussion: https://www.reddit.com/r/ClaudeCode/comments/1tq9pge/introducing_dynamic_workflows_in_claude_code/
- Reddit r/ClaudeAI - Dynamic workflows discussion: https://www.reddit.com/r/ClaudeAI/comments/1tq9ofy/introducing_dynamic_workflows_in_claude_code/
- Reddit r/vibecoding - Dynamic workflows discussion: https://www.reddit.com/r/vibecoding/comments/1tqe2yp/anthropic_just_introduced_dynamic_workflows_in/
