# 0700 Agent Trend Brief

- 작성 시점: 2026-07-02 07:00 KST
- 범위: Frontier model access governance, scientific agent workbench, full-stack/graph agent runtime, agent evaluation, skill and memory optimization, M&A and policy signals
- HTML: `report/agent-trend-brief/2026-07-02-0700/`

## 1. Executive Summary

참고 링크: [Anthropic Fable 5 redeploy](https://www.anthropic.com/news/redeploying-fable-5), [Anthropic Claude Science](https://www.anthropic.com/news/claude-science-ai-workbench), [OpenAI GeneBench-Pro](https://openai.com/index/introducing-genebench-pro/), [Google Genkit Agents API](https://developers.googleblog.com/build-agentic-full-stack-apps-with-genkit/), [Google ADK Go 2.0](https://developers.googleblog.com/announcing-adk-go-20/), [Google Why ADK 2.0](https://developers.googleblog.com/why-we-built-adk-20/), [Google Agent Quality Flywheel](https://developers.googleblog.com/driving-the-agent-quality-flywheel-from-your-coding-agent/), [LangChain untrusted agent code](https://www.langchain.com/blog/running-untrusted-agent-code-without-a-sandbox), [Microsoft SkillOpt](https://www.microsoft.com/en-us/research/blog/skillopt-agent-skills-as-trainable-parameters/)

오늘 핵심은 두 갈래다. 첫째, frontier model access governance가 제품 출시 경로의 일부가 됐다. Anthropic은 Fable 5/Mythos 5 접근 복구와 함께 new classifier, jailbreak severity framework, government pre-release evaluation 협업을 공개했다. 둘째, agent engineering stack은 full-stack API, graph runtime, quality flywheel, untrusted-code guardrail, skill/memory optimization으로 구체화되고 있다. Google의 Genkit Agents API/ADK Go 2.0/Quality Flywheel, LangChain의 Deep Agents guardrail/eval/memory, OpenAI GeneBench-Pro와 Microsoft SkillOpt가 같은 방향을 가리킨다.

핵심 판단:

1. Frontier model release는 모델 성능보다 access tier, safety classifier, jailbreak severity, government/third-party eval, cloud channel rollout이 먼저 관리되는 단계로 들어갔다.
2. Agent 앱 개발은 server/frontend/state/tool loop/streaming을 반복 구현하는 단계에서 full-stack agent API로 이동한다. Google Genkit Agents API가 이 흐름을 공식화한다.
3. 멀티에이전트 런타임은 graph, HITL, resumability, observability가 기본 요구사항이 됐다. Google ADK Go 2.0과 ADK 2.0 rationale은 이 요구를 SDK 설계 원칙으로 제시한다.
4. Agent 품질 관리는 독립 평가 서비스, AutoRaters, failure clustering, baseline comparison으로 이동한다. Google Quality Flywheel과 LangChain Harbor eval stack이 같은 방향이다.
5. 과학/생명과학 agent는 도메인 워크벤치와 domain-specific benchmark가 같이 성숙하는 대표 vertical이다. Claude Science와 GeneBench-Pro가 이를 보여준다.
6. Skills와 memory는 정적 문서가 아니라 versioned, eval-backed state가 되고 있다. Microsoft SkillOpt와 LangChain Wiki Memory를 같은 lifecycle로 봐야 한다.

이번 회차 결론: AI 플랫폼은 model access governance, full-stack agent API, graph runtime, eval flywheel, capability-secure execution, domain workbench, skill/memory lifecycle을 하나의 agent operating layer로 설계해야 한다.

## 2. Source Coverage

| 소스 | 확인 여부 | 유의미한 변화 | 신뢰도 |
| --- | --- | --- | --- |
| Anthropic | Checked | Fable 5/Mythos 5 접근 복구, jailbreak severity framework 제안, 정부 사전 평가 협업, Claude Science/Sonnet 5 발표. | Official / Confirmed |
| Google Developers | Checked | Genkit Agents API, ADK Go 2.0 graph workflow, ADK 2.0 rationale, Agent Quality Flywheel 확인. | Official / Confirmed |
| OpenAI | Checked | GeneBench-Pro 발표. computational biology에서 multi-stage reasoning과 consequential judgment를 측정. | Official / Confirmed |
| LangChain | Checked | Deep Agents의 untrusted code guardrail, Harbor eval stack, Wiki Memory, dynamic subagents 패턴 확인. | Official / Confirmed |
| Microsoft Research | Checked | SkillOpt가 agent skill file을 trainable/reusable parameter처럼 최적화하는 연구 흐름을 제시. | Research / Confirmed |
| Salesforce / NIST | Checked | Salesforce-Fin 인수와 NIST identity/authorization은 agentic service consolidation/governance prior signal로 유지. | Official / Prior signal |

## 3. Project / Product / Policy Alerts

### Anthropic Fable 5/Mythos 5 redeploy and release governance

참고 링크: [Redeploying Fable 5](https://www.anthropic.com/news/redeploying-fable-5), [White House AI security executive order](https://www.whitehouse.gov/presidential-actions/2026/06/promoting-advanced-artificial-intelligence-innovation-and-security/)

- 우선순위: High
- 확인된 사실: Anthropic은 7월 1일 업데이트에서 Fable 5와 Mythos 5 접근이 복구됐다고 밝혔다. Fable 5는 Claude Platform, Claude.ai, Claude Code, Claude Cowork에서 글로벌 제공을 재개하고, Mythos 5는 승인된 미국 조직부터 복구한다. Anthropic은 새로운 classifier가 보고된 bypass behavior를 99% 이상 차단한다고 설명하고, Amazon/Microsoft/Google 등 Glasswing partners와 jailbreak severity framework를 개발 중이라고 밝혔다.
- 해석: frontier model 출시는 release note, safety classifier, external red-team evidence, government pre-release evaluation, cloud channel re-enable plan이 결합된 운영 절차가 됐다.
- AI 플랫폼 영향: model access tier, user/org eligibility, safety classifier telemetry, jailbreak severity score, government/third-party eval record, cloud channel status를 model catalog에 넣어야 한다.
- 후속 확인: proposed jailbreak severity framework가 CVSS처럼 공개 rubric이 되는지, cloud marketplace별 재개 시점과 enterprise audit 로그가 어떻게 제공되는지 추적한다.

### Google full-stack and graph agent runtime

참고 링크: [Genkit Agents API](https://developers.googleblog.com/build-agentic-full-stack-apps-with-genkit/), [ADK Go 2.0](https://developers.googleblog.com/announcing-adk-go-20/), [Why ADK 2.0](https://developers.googleblog.com/why-we-built-adk-20/)

- 우선순위: High
- 확인된 사실: Google은 Genkit Agents API를 beta로 공개해 server-defined agent, persistence store, frontend `chat()` interface, `remoteAgent()` 연결을 제공한다고 설명했다. ADK Go 2.0은 graph-based workflow engine, built-in HITL, dynamic orchestration, unified node runtime을 제공한다.
- 해석: agent 앱 개발은 backend tool loop와 frontend streaming을 매번 조립하는 방식에서 full-stack protocol/API와 graph runtime을 결합하는 방식으로 이동한다.
- AI 플랫폼 영향: agent endpoint, session store, frontend streaming protocol, graph node schema, HITL node, checkpoint/resume, remote agent handoff를 product primitive로 제공해야 한다.
- 후속 확인: Genkit Agents API의 persistence semantics, detached task, history branching, multi-agent coordination이 ADK/Agent Runtime과 어떻게 연결되는지 확인한다.

### Agent evaluation flywheel and untrusted-code guardrails

참고 링크: [Agent Quality Flywheel](https://developers.googleblog.com/driving-the-agent-quality-flywheel-from-your-coding-agent/), [LangChain untrusted code](https://www.langchain.com/blog/running-untrusted-agent-code-without-a-sandbox), [Harbor eval stack](https://www.langchain.com/blog/unified-stack-for-evaluating-agents)

- 우선순위: High
- 확인된 사실: Google은 coding agent가 평가 데이터를 준비하고 inference, grading, failure analysis, optimization loop를 수행하는 Quality Flywheel을 소개했다. LangChain은 agent-written code의 execution isolation, capability isolation, durable pauses를 강조했고, Harbor/Deep Agents/LangSmith 기반 long-running eval stack을 제시했다.
- 해석: agent runtime은 code execution, evaluation automation, memory curation을 동시에 갖춰야 하며, prompt injection을 전제로 capability boundary를 설계해야 한다.
- AI 플랫폼 영향: eval runner, adaptive rater, failure clustering, baseline comparison, capability-secure interpreter, durable pause/resume, tool proxy를 같은 observability layer에 넣어야 한다.
- 후속 확인: Deep Agents의 untrusted-code 경계가 OS sandbox, capability token, interpreter API, tool proxy 중 어디서 enforce되는지 확인한다.

### Scientific agent workbench and domain benchmarks

참고 링크: [Claude Science](https://www.anthropic.com/news/claude-science-ai-workbench), [Claude Sonnet 5](https://www.anthropic.com/news/claude-sonnet-5), [GeneBench-Pro](https://openai.com/index/introducing-genebench-pro/), [GeneBench-Pro paper](https://cdn.openai.com/pdf/21938268-21af-442f-af93-3b2249afb241/genebench-pro.pdf)

- 우선순위: High
- 확인된 사실: Anthropic은 Claude Science를 과학자를 위한 AI workbench로 공개했고, Sonnet 5를 coding/agents/professional work scale의 새 모델로 발표했다. OpenAI는 computational biology에서 ambiguity와 consequential judgment를 측정하는 GeneBench-Pro를 공개했다.
- 해석: vertical agent는 chat wrapper가 아니라 domain toolchain, compute access, auditable artifact, workspace governance, expert rubric을 묶는 제품이다.
- AI 플랫폼 영향: domain workbench template, artifact lineage, dataset version, compute environment, expert eval pack, regulated access tier가 필요하다.

### SkillOpt and wiki memory as optimized state

참고 링크: [Microsoft SkillOpt](https://www.microsoft.com/en-us/research/blog/skillopt-agent-skills-as-trainable-parameters/), [LangChain Wiki Memory](https://www.langchain.com/blog/wiki-memory), [Dynamic Subagents](https://www.langchain.com/blog/introducing-dynamic-subagents-in-deep-agents)

- 우선순위: Medium-High
- 확인된 사실: Microsoft Research는 SkillOpt를 통해 agent skill files를 transfer 가능한 task-solving procedure로 최적화하는 방향을 제시했다. LangChain은 wiki-style memory를 agent state로 운영하는 패턴과 dynamic subagents를 제시했다.
- 해석: skills와 memory는 사람이 한 번 쓰는 운영 문서가 아니라 benchmark/eval과 연결해 지속 개선하는 외부 state가 된다.
- AI 플랫폼 영향: skill registry, memory wiki, review queue, transfer eval, versioning, rollback, provenance가 필요하다.

## 4. 주요 동향과 중요도

| 중요도 | 동향 | 확인된 사실 | 해석 / 추론 | AI 플랫폼 적용 포인트 |
| --- | --- | --- | --- | --- |
| High | Frontier model access governance | Fable 5/Mythos 5 access restored with safeguards and government collaboration. | model release는 classifier, jailbreak severity, pre-release eval, eligibility tier를 포함한다. | model access catalog, jailbreak severity score, classifier telemetry, channel status. |
| High | Full-stack agent API | Genkit Agents API beta가 server-defined agents, persistence, frontend chat interface를 제공. | agent 앱 개발이 full-stack protocol/API로 추상화된다. | session store, streaming protocol, remote agent bridge, detached task. |
| High | Graph multi-agent runtime | ADK Go 2.0 graph workflow, HITL, unified runtime. | multi-agent orchestration은 graph + state + human approval이 기본. | graph runtime, checkpoint, approval node, OTel trace. |
| High | Agent eval flywheel | Google AutoRaters/failure analysis, LangChain Harbor eval stack. | agent 품질은 독립 평가 서비스와 baseline comparison으로 운영된다. | eval runner, auto-rater, failure clustering, regression gate. |
| High | Scientific agent workbench | Claude Science, GeneBench-Pro 동시 신호. | 과학/생명과학은 agent product와 eval이 함께 성숙하는 도메인. | domain workbench, artifact audit, compute connector, expert rubric. |
| Medium-High | Skill and memory optimization | SkillOpt, Wiki Memory 발표. | skills/memory는 static docs가 아니라 versioned, eval-backed state. | skill registry, memory wiki, review queue, transfer eval. |

## 5. AI Platform / Service Implications

1. Frontier model access governance를 model catalog에 넣는다.
   - model별 access tier, geography/org eligibility, safety classifier version, jailbreak severity findings, government/third-party eval status, cloud channel rollout 상태를 기록한다.
   - user-visible false positive와 fallback model behavior도 incident/eval 지표로 남긴다.

2. Full-stack agent API를 표준 product surface로 만든다.
   - agent endpoint, session store, message history, tool loop, streaming, branch history, detached tasks, frontend SDK를 같은 contract로 묶는다.
   - backend agent definition과 frontend `chat()` interface 사이에 trace id와 consent/approval metadata를 통과시킨다.

3. Graph runtime과 eval runtime을 분리하지 않는다.
   - graph node별 input/output, human approval, retry, pause/resume, failure cluster, before/after metric을 저장한다.
   - eval runner가 graph trace를 직접 읽고 regression gate를 걸 수 있어야 한다.

4. Agent-written code는 capability-secure execution으로 다룬다.
   - sandbox 여부와 무관하게 agent code가 접근 가능한 data/action capability를 명시적으로 넘긴다.
   - durable pause와 human input resume을 interpreter/runtime 계약에 포함한다.

5. Domain workbench를 agent product 단위로 정의한다.
   - 과학, 금융, 법무, 고객지원처럼 데이터·도구·평가가 강한 도메인은 workbench template로 관리한다.
   - artifact lineage, dataset version, compute environment, approval, citation, export를 기본 메타데이터로 둔다.

6. Skills와 wiki memory를 eval-backed state로 관리한다.
   - skill file과 memory page는 owner, source, version, last-eval score, transfer test, rollback path를 가져야 한다.
   - raw source → compact wiki memory → agent-readable knowledge → eval loop를 pipeline으로 만든다.

## 6. Recommended Actions

- 지금: `model_access_governance` schema 초안을 만든다. 필드는 model, access tier, eligible org/user class, safety classifier version, jailbreak severity record, fallback model, cloud channel status다.
- 지금: `agent_run_trace` schema에 graph node, HITL state, eval verdict, failure cluster, skill/memory version, frontend session id를 추가한다.
- 지금: `domain_workbench` template 초안을 만든다. 필드는 domain, dataset connector, tool/package list, compute target, artifact lineage, approval tier, eval pack이다.
- 30일: Genkit Agents API, ADK Go 2.0, LangChain Deep Agents, 기존 AX runtime을 full-stack/session/graph/resume/eval 관점으로 비교한다.
- 30일: Claude Science, GeneBench-Pro, LifeSciBench를 묶어 scientific agent workbench/eval 비교 문서를 만든다.
- 계속: Fable 5/Mythos 5 release governance, Salesforce-Fin 같은 agentic service M&A, NIST identity/authorization을 governance 신호로 추적한다.

## 7. Risks / Watch Items

| 리스크 | 설명 | 대응 |
| --- | --- | --- |
| Release governance opacity | advanced model 접근이 정부/파트너 평가와 함께 움직이면 고객은 왜 접근이 막히거나 복구되는지 알기 어렵다. | model access catalog, status page, classifier change log, severity framework, customer notice. |
| Full-stack abstraction leakage | frontend SDK가 tool approval, consent, trace id, session branch를 숨기면 운영 감사가 어려워진다. | frontend/backend shared trace id, explicit approval events, session export. |
| Eval self-gaming | optimizer가 자기 산출물을 직접 채점하면 metric gaming 위험이 커진다. | independent evaluator, held-out eval set, baseline comparison, manual audit. |
| Agent-written code escape | dynamic subagent code가 prompt injection으로 host/tool/data를 오용할 수 있다. | execution isolation, capability isolation, deny-by-default tool proxy, durable pause. |
| Skill drift | 자동 최적화된 skill이 특정 harness나 benchmark에 과적합될 수 있다. | transfer test, rollback, provenance, multi-harness eval. |
| Scientific safety/access | bio-related tools와 모델이 research acceleration과 misuse risk를 동시에 키울 수 있다. | trusted access, action approval, evidence log, export controls, policy review. |

## 8. Sources

### 왜 이걸 정리하게 되었는가

- [Anthropic - Redeploying Fable 5](https://www.anthropic.com/news/redeploying-fable-5)
- [Google - Build agentic full-stack apps with Genkit](https://developers.googleblog.com/build-agentic-full-stack-apps-with-genkit/)
- [Google - ADK Go 2.0](https://developers.googleblog.com/announcing-adk-go-20/)
- [Google - Why we built ADK 2.0](https://developers.googleblog.com/why-we-built-adk-20/)
- [Google - Agent Quality Flywheel](https://developers.googleblog.com/driving-the-agent-quality-flywheel-from-your-coding-agent/)

### 딥리서치 출처

- [Anthropic - Claude Science, an AI workbench for scientists](https://www.anthropic.com/news/claude-science-ai-workbench)
- [Anthropic - Introducing Claude Sonnet 5](https://www.anthropic.com/news/claude-sonnet-5)
- [OpenAI - Introducing GeneBench-Pro](https://openai.com/index/introducing-genebench-pro/)
- [GeneBench-Pro paper](https://cdn.openai.com/pdf/21938268-21af-442f-af93-3b2249afb241/genebench-pro.pdf)
- [LangChain - Running Untrusted Agent Code Without a Sandbox](https://www.langchain.com/blog/running-untrusted-agent-code-without-a-sandbox)
- [LangChain - Harbor x LangChain: A Unified Stack for Evaluating Agents](https://www.langchain.com/blog/unified-stack-for-evaluating-agents)
- [LangChain - Wiki Memory](https://www.langchain.com/blog/wiki-memory)
- [LangChain - Introducing Dynamic Subagents in Deep Agents](https://www.langchain.com/blog/introducing-dynamic-subagents-in-deep-agents)
- [Microsoft Research - SkillOpt](https://www.microsoft.com/en-us/research/blog/skillopt-agent-skills-as-trainable-parameters/)
- [Salesforce - Definitive agreement to acquire Fin](https://www.salesforce.com/news/press-releases/2026/06/15/salesforce-signs-definitive-agreement-to-acquire-fin/)
- [NIST - Software and AI Agent Identity and Authorization](https://www.nccoe.nist.gov/sites/default/files/2026-02/accelerating-the-adoption-of-software-and-ai-agent-identity-and-authorization-concept-paper.pdf)
