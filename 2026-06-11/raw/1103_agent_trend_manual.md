# 1103 Agent Trend Manual Brief

- 작성 시점: 2026-06-11 11:03 KST
- 질문: 사람들이 AI Agent를 어느 분야에 가장 많이 사용하고 있는가
- 범위: 개인 사용, 기업 배포, 개발자/IT 사용, 고객지원/업무 자동화, 플랫폼 전략
- HTML: `2026-06-11/1103_agent_trend_manual.html`

## 1. Executive Summary

한 줄 결론: 현재 AI Agent 사용이 가장 많이 확인되는 분야는 기업 기준으로는 고객지원/컨택센터와 리서치/데이터 분석이다. 개인/지식근로자 기준으로는 조사/요약, 정보 탐색, 글쓰기, 개인 생산성 보조가 가장 강하다. 개발자/IT/보안 agent는 성장 속도와 전략 중요도는 높지만, 전체 대중 사용 관점에서는 아직 주류라기보다 고강도 초기 채택 영역에 가깝다.

Sources: [LangChain State of Agent Engineering 2025](https://www.langchain.com/state-of-agent-engineering), [LangChain State of AI Agents 2024](https://www.langchain.com/stateofaiagents), [OpenAI ChatGPT Usage Study](https://openai.com/index/how-people-are-using-chatgpt/), [McKinsey State of AI 2025](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai), [Deloitte State of AI in the Enterprise 2026](https://www.deloitte.com/us/en/what-we-do/capabilities/applied-artificial-intelligence/content/state-of-ai-in-the-enterprise.html), [PwC AI Agent Survey 2025](https://www.pwc.com/us/en/tech-effect/ai-analytics/ai-agent-survey.html)

핵심 판단:

1. 고객지원이 현재 가장 명확한 기업 agent 배포처다. LangChain 2025는 primary agent deployment 중 customer service가 26.5%로 1위라고 보고했고, Deloitte 2026도 agentic AI의 최고 영향 영역을 customer support로 본다.
2. 리서치/데이터 분석은 개인과 기업을 모두 관통하는 강한 사용처다. LangChain 2025는 research & data analysis를 24.4%로 2위로 보고했고, LangChain 2024는 research/summarization을 agent가 잘하는 상위 과업으로 58%까지 제시했다.
3. 내부 업무 자동화와 개인 생산성은 대기업에서 더 강하다. LangChain 2025는 10,000명 이상 조직에서 internal productivity가 26.8%로 가장 높은 primary use case라고 보고했다.
4. IT/개발/보안 agent는 플랫폼 관점의 핵심 전장이다. McKinsey는 agent 사용이 IT와 knowledge management에서 가장 자주 보고된다고 설명했고, PwC는 agent를 쓰거나 6개월 내 쓸 계획인 기업 중 IT/cybersecurity가 53%라고 제시했다.
5. 일반 소비자 AI 사용은 agent보다 넓은 범주지만, 사용 의도는 agent 전략에도 직접 힌트를 준다. OpenAI의 ChatGPT 사용 연구는 practical guidance, seeking information, writing이 소비자 대화의 큰 비중을 차지한다고 보고했다.

## 2. What Changed

| 구분 | 2024년 신호 | 2025-2026년 신호 | 해석 |
|---|---|---|---|
| Agent 사용처 | LangChain 2024는 research/summarization 58%, personal productivity/assistance 53.5%, customer service 45.8%를 상위 사용처로 제시. | LangChain 2025는 실제 primary deployment 기준 customer service 26.5%, research & data analysis 24.4%, internal workflow automation 18%를 제시. | 실험/기대 단계에서는 지식 작업과 개인 생산성이 강했고, production 배포 단계에서는 고객지원과 데이터/리서치가 앞선다. |
| 기업 확산 단계 | 여러 산업에서 agent production 계획이 높다는 신호. | McKinsey 2025는 62%가 agent를 실험 중이거나 scaling 중이지만, 개별 기능별 scaling은 10%를 넘지 않는다고 설명. | 관심은 넓지만 scale은 아직 초기다. 기능별로는 좁고 검증 가능한 업무부터 간다. |
| 고객 접점 | 챗봇/FAQ 중심. | Deloitte, PwC, ServiceNow, Zendesk 모두 customer support, CRM, case resolution, service automation을 강하게 밀고 있다. | 외부 고객 접점은 volume과 ROI가 명확하지만 brand, compliance, audit 리스크도 가장 크다. |
| 개발/IT | coding assistant와 autocomplete 중심. | coding agent, service desk agent, AIOps/SRE agent, security triage agent로 확장. Stack Overflow 2025 기준 개발자 agent daily 사용은 14.1%, weekly는 9%로 아직 제한적. | 개발/IT는 가장 빠르게 agent화되지만 일반화까지는 품질, 신뢰, 권한 통제가 병목이다. |

## 3. High-Importance Updates

### 3.1 현재 가장 많이 쓰는 분야 순위

| 순위 | 분야 | 근거 수준 | 왜 많이 쓰이는가 | 플랫폼/서비스 의미 |
|---|---|---|---|---|
| 1 | 고객지원 / 컨택센터 / CRM | Confirmed by surveys + vendor deployments | 문의량이 많고, FAQ/주문/예약/환불/케이스 라우팅처럼 반복 업무가 많으며, human escalation 기준을 잡기 쉽다. | external-facing agent이므로 brand tone, PII redaction, 답변 근거, escalation, audit log, SLA를 runtime 기본 기능으로 넣어야 한다. |
| 2 | 리서치 / 요약 / 데이터 분석 / 지식관리 | Confirmed by surveys | 정보 수집, 요약, 비교, 보고서 초안, 데이터 해석은 agent가 여러 소스와 도구를 순차적으로 다루기 좋은 업무다. | retrieval, citation, data connector, notebook/spreadsheet tool, evidence trace, analyst review surface가 핵심이다. |
| 3 | 개인 생산성 / 내부 워크플로 자동화 | Confirmed, stronger in large orgs | 일정, 이메일, 회의 액션아이템, 문서 정리, HR/finance/procurement 요청 같은 반복 사무 업무가 많다. | employee identity, approval, workflow handoff, ERP/HRIS/ticketing connector, scheduled run policy가 필요하다. |
| 4 | IT / 개발 / 사이버보안 | Confirmed but uneven adoption | 코드 작성, 테스트, 로그 분석, 서비스데스크, 취약점 triage는 tool-using agent와 잘 맞는다. 다만 권한과 품질 리스크가 크다. | privileged tool access, sandbox, branch/worktree isolation, incident audit, rollback, approval matrix가 우선이다. |
| 5 | 세일즈 / 마케팅 / 콘텐츠 | Confirmed near-term plans | 고객 세그먼트 조사, 캠페인 초안, 제안서, CRM 업데이트, 리드 qualification이 반복된다. | brand policy, claim review, CRM write-back, campaign approval, attribution tracking이 중요하다. |
| 6 | 공급망 / R&D / 제품개발 / 금융/HR/법무/구매 | High-potential, less clearly top today | 업무 가치는 크지만 domain data, 책임 소재, 승인 절차가 복잡하다. | vertical agent template, policy-aware workflow, domain eval, human-in-the-loop approval부터 설계해야 한다. |

### 3.2 고객지원이 1위로 보이는 이유

Confirmed facts:

- LangChain 2025는 primary agent use case 중 customer service를 26.5%로 가장 흔한 배포처라고 제시했다.
- Deloitte 2026은 agentic AI가 customer support에서 가장 큰 impact를 낼 것으로 보고, supply chain, R&D, knowledge management, cybersecurity도 high-potential 영역으로 제시했다.
- PwC 2025는 AI agent를 쓰거나 6개월 내 쓸 계획인 기업 중 customer service/support가 57%, sales/marketing 54%, IT/cybersecurity 53%라고 보고했다.
- ServiceNow 2026 발표는 IT, CRM, employee service, security/risk에 걸친 role-scoped AI specialist를 제품화하고 있으며, CRM 영역에서 대규모 case/order/quote 처리량을 공개했다.

Interpretation:

고객지원은 agent가 "말만 하는 챗봇"에서 "케이스를 끝내는 업무 수행자"로 이동하기 가장 쉬운 표면이다. 고객 요청은 자연어로 들어오지만, 뒤의 처리는 order lookup, refund rule, ticket update, escalation, email/message response처럼 tool call과 workflow로 쪼개진다. 그래서 agent runtime, connector, approval, audit가 모두 드러나는 대표 use case다.

### 3.3 리서치/데이터 분석은 개인과 기업을 동시에 잡는다

Confirmed facts:

- LangChain 2025는 research & data analysis를 24.4%로 customer service에 근접한 2위 primary deployment로 제시했다.
- LangChain 2024는 research/summarization 58%와 personal productivity/assistance 53.5%를 agent가 적합한 상위 과업으로 제시했다.
- OpenAI의 ChatGPT 사용 연구는 consumer usage에서 practical guidance, seeking information, writing이 큰 비중을 차지한다고 설명한다. 이 자료는 agent 전용 조사는 아니지만, 사람들이 AI에게 맡기는 기본 수요가 정보 탐색과 지식 작업에 있다는 근거로 볼 수 있다.
- McKinsey 2025는 agent use가 IT와 knowledge management에서 가장 자주 보고된다고 설명한다.

Interpretation:

리서치/데이터 분석은 agent가 "한 번 답하기"보다 "여러 단계로 확인하고 정리하기"를 수행할 수 있어 agent의 차별성이 뚜렷하다. 다만 이 분야는 신뢰 가능한 citation, source ranking, data lineage, 재현 가능한 analysis log 없이는 내부 보고서 품질 리스크가 커진다.

### 3.4 개발/IT agent는 전략 중요도 대비 대중 확산은 아직 제한적이다

Confirmed facts:

- Stack Overflow 2025는 전체 응답자 기준 agent daily 사용 14.1%, weekly 사용 9%, monthly/infrequent 7.8%를 제시했고, 52%는 agent를 쓰지 않거나 단순 AI 도구에 머문다고 설명했다.
- PwC는 IT/cybersecurity를 agent near-term 사용/계획 상위 기능 중 하나로 제시했고, 별도 IT agent 글에서 53%를 언급한다.
- ServiceNow 2026은 L1 IT Service Desk, AIOps, SRE, asset lifecycle, security/risk specialist를 전면에 내세웠다.

Interpretation:

개발/IT agent는 tool 권한이 높고 산출물이 코드/인프라/보안 상태를 바꾸기 때문에 platform primitive를 가장 많이 요구한다. 그래서 "많은 사람이 이미 쓴다"보다는 "agent control plane이 가장 먼저 정교해져야 하는 영역"으로 보는 편이 정확하다.

## 4. Project, Paper, Product, and Protocol Signals

| 신호 | 유형 | 요지 | 왜 중요한가 |
|---|---|---|---|
| LangChain State of Agent Engineering 2025 | Survey | production agent의 primary use case가 customer service와 research/data analysis에 집중. | agent builder/enterprise audience 기준으로 현재 배포 우선순위를 보여준다. |
| McKinsey State of AI 2025 | Survey | 62%가 agent를 실험 또는 scaling 중이지만, 기능별 scaling은 아직 낮음. | "관심은 넓고 scale은 좁다"는 도입 상태를 보여준다. |
| Deloitte State of AI 2026 | Survey / analyst | customer support가 agentic AI impact 최상위로 제시되고, supply chain/R&D/KM/cybersecurity가 뒤따름. | 고객지원 1위 판단을 보강하며 다음 확장 분야를 제시한다. |
| PwC AI Agent Survey 2025 | Executive survey | customer service/support, sales/marketing, IT/cybersecurity가 near-term 기능 상위권. | CX, revenue, IT 운영이 agent budget을 끌어당기는 축임을 보여준다. |
| ServiceNow Autonomous Workforce 2026 | Product signal | IT, CRM, employee service, security/risk를 role-scoped specialist로 제품화. | enterprise workflow vendor들이 agent를 부가 기능이 아니라 workflow 실행 계층으로 배치하고 있다. |
| Stack Overflow Developer Survey 2025 | Developer survey | 개발자 agent 사용은 아직 mainstream이라고 보기 어려움. | coding agent 과열 해석을 보정하고 adoption gap을 보여준다. |

## 5. AI Platform and Service Implications

### 5.1 우선 투자 순서

1. 고객지원/CRM agent: 사용량과 ROI가 가장 명확하다. 단, 외부 노출이므로 safety, escalation, audit를 먼저 구축해야 한다.
2. 리서치/데이터 분석 agent: 내부 생산성 효과가 크고 도메인별로 확장하기 쉽다. citation, source quality, data connector가 핵심이다.
3. 내부 워크플로/employee service agent: HR, finance, procurement, legal 같은 back-office 요청 처리에 적합하다. identity와 approval workflow가 중요하다.
4. IT/개발/보안 agent: platform team 관점의 전략 우선순위가 높다. sandbox, policy, rollback, incident audit 없이는 scale하면 위험하다.
5. 세일즈/마케팅/content agent: 빠르게 확산될 수 있지만 brand claim과 compliance review가 필요하다.

### 5.2 공통 플랫폼 기능

| 플랫폼 기능 | 필요한 이유 | 우선 적용 분야 |
|---|---|---|
| Agent identity / scope / owner | 누가 어떤 권한으로 어떤 업무를 맡겼는지 추적. | 전 분야 |
| Tool permission matrix | 고객정보, 결제, 주문, 코드, 보안 시스템 접근을 제한. | 고객지원, IT/보안, 커머스 |
| Execution ledger / trace | agent의 판단, tool call, 결과, 실패를 재생 가능하게 저장. | 고객지원, 리서치, IT |
| Human escalation / approval | 고위험 액션을 사람 승인으로 넘김. | 환불, 결제, 인프라 변경, 법무 |
| Evidence and citation layer | 답변과 보고서의 근거를 명시. | 리서치, 데이터 분석, 고객지원 |
| Domain eval suite | 분야별 성공/실패 기준을 측정. | 전 분야 |
| Artifact review surface | transcript 밖에서 케이스, 보고서, PR, 캠페인, 주문 변경안을 검토. | 고객지원, 개발, 세일즈/마케팅 |

## 6. Recommended Actions

1. 고객지원과 리서치/데이터 분석을 첫 번째 benchmark use case로 잡는다. 두 분야는 수요와 근거가 가장 강하고, 외부-facing/내부-facing agent 요구사항을 모두 드러낸다.
2. "분야별 agent registry"를 만든다. customer support, research, productivity, IT/security, sales/marketing, back-office로 나누고 각 agent에 owner, toolset, risk tier, approval policy, eval score를 붙인다.
3. 고객지원 agent는 자동 답변보다 triage, draft, retrieval-grounded answer, escalation부터 시작한다. full autonomous resolution은 audit와 rollback이 쌓인 뒤에 넓힌다.
4. 리서치/데이터 agent는 citation, source ranking, stale-source warning, data lineage를 기본 기능으로 둔다.
5. 개발/IT/보안 agent는 별도 privileged runtime으로 격리한다. worktree/sandbox, read-only default, production write approval, incident replay를 최소 조건으로 둔다.
6. 30일 안에 분야별 adoption metric을 정한다: deflection rate, answer acceptance, cycle time, human edit distance, escalation rate, tool error, policy violation, cost per completed task.

## 7. Risks, Uncertainties, and Watch Items

| 리스크 | 설명 | 대응 |
|---|---|---|
| 정의 혼선 | survey마다 AI agent, chatbot, AI assistant, agentic workflow의 정의가 다르다. | "자율 계획/도구 사용/상태 유지/업무 완료" 수준별로 분류한다. |
| 표본 편향 | LangChain은 builder/enterprise audience, Stack Overflow는 개발자, PwC/Deloitte/McKinsey는 기업 임원/조직 응답 중심이다. | 개인 사용, 개발자 사용, 기업 배포를 섞지 않고 따로 본다. |
| 고객지원 rollback | 고객지원은 많이 쓰이지만 hallucination, data exposure, audit 부재가 바로 고객 피해로 이어질 수 있다. | high-risk query escalation, PII redaction, answer grounding, post-resolution audit를 필수화한다. |
| 내부 자동화의 의도 drift | 일정/업무 agent가 오래 반복 실행되면 사용자의 최신 의도와 어긋날 수 있다. | expiry, re-consent, scheduled run review, dry-run mode를 둔다. |
| 개발/IT agent 권한 | coding/IT/security agent는 파일, repo, cloud, ticket, identity system을 건드린다. | least privilege, sandbox, branch isolation, approval checkpoint, rollback plan을 둔다. |

Follow-up checks:

- 다음 회차에는 customer support agent rollback/incident 사례를 별도 조사해 "많이 쓰는 분야와 실패가 많이 나는 분야가 같은가"를 확인한다.
- agentic commerce, travel booking, personal shopping은 visibility는 높지만 현재 주요 survey에서 최상위 사용처로 확정하기에는 약하다. UCP/AP2 같은 프로토콜 신호와 함께 계속 추적한다.
- healthcare, public sector, education은 수요가 크지만 규제와 책임 문제가 크므로 사용량보다 governance maturity를 중심으로 봐야 한다.

## 8. Inline Evidence

| Claim | Evidence | Label |
|---|---|---|
| 기업 production agent의 1위 use case는 customer service에 가깝다. | LangChain 2025: customer service 26.5%, research & data analysis 24.4%. | Confirmed fact |
| agent에 적합한 개인/지식 업무는 research/summarization과 personal productivity가 강하다. | LangChain 2024: research/summarization 58%, personal productivity/assistance 53.5%, customer service 45.8%. | Confirmed fact |
| 일반 AI 사용은 practical guidance, information seeking, writing 중심이다. | OpenAI 2025 ChatGPT usage study. 단, agent 전용 조사는 아니다. | Adjacent evidence |
| 기업 agent는 아직 초기 scale 단계다. | McKinsey 2025: 62%가 experimenting/scaling, 기능별 scaling은 10% 이하. | Confirmed fact |
| customer support, supply chain, R&D, KM, cybersecurity가 agentic AI 영향권이다. | Deloitte 2026 State of AI in the Enterprise. | Confirmed fact |
| 개발자 agent 사용은 빠르게 커지지만 mainstream은 아니다. | Stack Overflow 2025: daily 14.1%, weekly 9%, no/plan/no-plan 비중 큼. | Confirmed fact |
| IT/CRM/employee/security agent는 제품화가 빠르다. | ServiceNow 2026 Autonomous Workforce 발표. | Product signal |

## 9. Sources

### 왜 이걸 정리하게 되었는가

- 사용자가 "사람들이 AI Agent를 어느 분야 가장 많이 사용하고 있는지"를 요청했다.
- 기존 Agent Trend 문서가 runtime/control plane, agent protocol, skill governance 중심이어서, 실제 사용 분야 분포를 별도 수동 브리프로 정리할 필요가 있었다.

### 딥리서치 출처

- [LangChain - State of Agent Engineering 2025](https://www.langchain.com/state-of-agent-engineering)
- [LangChain - State of AI Agents 2024](https://www.langchain.com/stateofaiagents)
- [OpenAI - How people are using ChatGPT](https://openai.com/index/how-people-are-using-chatgpt/)
- [McKinsey - The State of AI in 2025](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai)
- [Deloitte - The State of AI in the Enterprise 2026](https://www.deloitte.com/us/en/what-we-do/capabilities/applied-artificial-intelligence/content/state-of-ai-in-the-enterprise.html)
- [Deloitte Insights - Agentic AI is scaling faster than guardrails](https://www.deloitte.com/us/en/insights/topics/emerging-technologies/ai-agents-scaling-faster.html)
- [PwC - AI Agent Survey 2025](https://www.pwc.com/us/en/tech-effect/ai-analytics/ai-agent-survey.html)
- [PwC - AI agents for IT](https://www.pwc.com/us/en/tech-effect/ai-analytics/agentic-ai-in-it.html)
- [Stack Overflow Developer Survey 2025 - AI](https://survey.stackoverflow.co/2025/ai)
- [ServiceNow - Autonomous Workforce 2026 announcement](https://newsroom.servicenow.com/press-releases/details/2026/ServiceNow-brings-Autonomous-Workforce-to-every-major-business-function/default.aspx)
- [Zendesk - AI customer service statistics 2026](https://www.zendesk.com/blog/ai/productivity/ai-customer-service-statistics/)
