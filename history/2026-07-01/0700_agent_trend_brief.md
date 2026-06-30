# 0700 Agent Trend Brief

- 작성 시점: 2026-07-01 07:00 KST
- 범위: Scientific agent workbench, frontier model / domain agent releases, multi-agent runtime, agent evaluation, skill optimization, memory, agent M&A and governance signals
- HTML: `report/agent-trend-brief/2026-07-01-0700/`

## 1. Executive Summary

참고 링크: [Anthropic Claude Science](https://www.anthropic.com/news/claude-science-ai-workbench), [Anthropic Claude Sonnet 5](https://www.anthropic.com/news/claude-sonnet-5), [OpenAI GeneBench-Pro](https://openai.com/index/introducing-genebench-pro/), [Google ADK Go 2.0](https://developers.googleblog.com/announcing-adk-go-20/), [Google Agent Quality Flywheel](https://developers.googleblog.com/driving-the-agent-quality-flywheel-from-your-coding-agent/), [LangChain untrusted agent code](https://www.langchain.com/blog/running-untrusted-agent-code-without-a-sandbox), [LangChain Harbor eval stack](https://www.langchain.com/blog/unified-stack-for-evaluating-agents), [Microsoft SkillOpt](https://www.microsoft.com/en-us/research/blog/skillopt-agent-skills-as-trainable-parameters/)

오늘 핵심은 agent가 범용 채팅/코딩 도구에서 도메인 워크벤치, 멀티에이전트 런타임, 평가 플라이휠, 스킬 최적화 레이어로 더 구체화되고 있다는 점이다. Anthropic은 과학자를 위한 Claude Science와 Sonnet 5를 공개했고, OpenAI는 computational biology에서 ambiguity와 consequential judgment를 재는 GeneBench-Pro를 공개했다. Google은 ADK Go 2.0의 graph-based workflow engine과 coding agent가 직접 평가 루프를 운전하는 Agent Quality Flywheel을 냈고, LangChain은 untrusted agent-written code, Harbor 기반 long-running eval, wiki memory 패턴을 동시에 강조했다.

핵심 판단:

1. 과학/생명과학 agent가 새로운 경쟁 축으로 부상했다. Claude Science와 GeneBench-Pro는 단순 질의응답이 아니라 도구, 데이터, 계산 자원, 감사 가능한 산출물, multi-stage judgment를 묶는 방향이다.
2. 멀티에이전트 런타임은 graph, HITL, resumability, observability가 기본 요구사항이 됐다. Google ADK Go 2.0은 이 요구를 공식 SDK 수준에서 명시한다.
3. Agent 품질 관리는 "프롬프트 수정"에서 독립 평가 서비스, AutoRaters, failure clustering, baseline comparison으로 이동한다. Google Quality Flywheel과 LangChain/Harbor가 같은 방향이다.
4. Agent-written code와 dynamic subagents는 보안 경계가 없으면 위험하다. LangChain은 execution isolation, capability isolation, durable pauses를 핵심 요구로 제시한다.
5. 스킬과 메모리는 수동 문서가 아니라 최적화·재사용 가능한 agent state가 되고 있다. Microsoft SkillOpt와 LangChain Wiki Memory가 같은 신호다.

이번 회차 결론: AI 플랫폼은 domain workbench, graph runtime, agent eval runner, skill optimizer, wiki memory, untrusted-code guardrail을 분리된 기능이 아니라 하나의 agent operating layer로 묶어야 한다.

## 2. Source Coverage

| 소스 | 확인 여부 | 유의미한 변화 | 신뢰도 |
| --- | --- | --- | --- |
| Anthropic | Checked | Claude Science workbench와 Claude Sonnet 5 발표. 과학 도메인 agent product와 모델 agentic performance 신호. | Official / Confirmed |
| OpenAI | Checked | GeneBench-Pro 발표. computational biology에서 multi-stage statistical reasoning과 consequential judgment를 측정. | Official / Confirmed |
| Google Developers | Checked | ADK Go 2.0 graph workflow, HITL, dynamic orchestration, unified runtime. Agent Quality Flywheel의 AutoRaters와 failure analysis. | Official / Confirmed |
| LangChain | Checked | Deep Agents의 untrusted code guardrail, Harbor eval stack, Wiki Memory 패턴 발표. | Official / Confirmed |
| Microsoft Research | Checked | SkillOpt가 agent skill file을 trainable/reusable parameter처럼 최적화하는 연구 흐름을 제시. | Research / Confirmed |
| Salesforce | Checked | Fin 인수는 6월 중순 발표였지만 agentic customer service M&A 기준점으로 유지. 오늘 새 대형 M&A는 확인하지 못함. | Official / Prior signal |
| NIST / policy | Checked | 새 규제 발표보다 기존 agent identity/authorization concept paper를 governance 후속 기준으로 유지. | Government / Prior signal |

## 3. Project / Product / Policy Alerts

### Anthropic Claude Science and Sonnet 5

참고 링크: [Claude Science](https://www.anthropic.com/news/claude-science-ai-workbench), [Claude Sonnet 5](https://www.anthropic.com/news/claude-sonnet-5), [The Briefing: AI for Science](https://www.anthropic.com/events/the-briefing-ai-for-science)

- 우선순위: High
- 확인된 사실: Anthropic은 Claude Science를 과학자를 위한 AI workbench로 공개했고, Sonnet 5를 coding, agents, professional work scale의 새 모델로 발표했다.
- 해석: vertical agent는 더 이상 chat wrapper가 아니라 domain toolchain, compute access, auditable artifact, workspace governance를 묶는 제품이다.
- AI 플랫폼 영향: domain workbench template, artifact lineage, tool/package registry, compute quota, experiment audit, sensitive-domain access tier가 필요하다.
- 후속 확인: Claude Science의 데이터 커넥터, 컴퓨팅 실행 환경, artifact format, enterprise admin controls, bio-risk access policy를 추적한다.

### OpenAI GeneBench-Pro

참고 링크: [GeneBench-Pro](https://openai.com/index/introducing-genebench-pro/), [Inside GeneBench-Pro](https://openai.com/index/genebench-pro/case-studies/), [GeneBench-Pro paper](https://cdn.openai.com/pdf/21938268-21af-442f-af93-3b2249afb241/genebench-pro.pdf)

- 우선순위: High
- 확인된 사실: OpenAI는 computational biology에서 ambiguity와 consequential judgment를 측정하는 GeneBench-Pro를 발표했다.
- 해석: agent eval이 단일 답변/코딩 task에서 multi-stage scientific reasoning, estimator choice, diagnostics, downstream go/no-go decision으로 이동한다.
- AI 플랫폼 영향: domain-specific eval packs, expert rubric, trace replay, intermediate decision grading, uncertainty/provenance scoring이 필요하다.
- 후속 확인: Life science benchmark 결과가 domain workbench product와 procurement 기준으로 연결되는지 확인한다.

### Google ADK Go 2.0 and Agent Quality Flywheel

참고 링크: [ADK Go 2.0](https://developers.googleblog.com/announcing-adk-go-20/), [Agent Quality Flywheel](https://developers.googleblog.com/driving-the-agent-quality-flywheel-from-your-coding-agent/)

- 우선순위: High
- 확인된 사실: Google은 ADK Go 2.0에서 graph-based workflow engine, built-in HITL, dynamic orchestration, unified node runtime을 공개했고, coding agent가 평가 데이터를 준비하고 inference, grading, failure analysis, optimization loop를 돌리는 Quality Flywheel을 소개했다.
- 해석: agent framework의 핵심은 graph composition과 evaluation automation을 결합하는 것이다. agent를 만드는 도구와 agent를 평가/개선하는 도구가 같은 workflow 안에 들어온다.
- AI 플랫폼 영향: graph runtime, checkpoint/resume, human approval node, trace export, AutoRater integration, regression gate를 core runtime schema로 설계해야 한다.
- 후속 확인: ADK Go 2.0의 graph persistence, failure recovery, OpenTelemetry trace, A2A interop, Quality Flywheel skill의 CI/CD 연결을 확인한다.

### LangChain Deep Agents guardrails, Harbor eval, Wiki Memory

참고 링크: [Untrusted agent code](https://www.langchain.com/blog/running-untrusted-agent-code-without-a-sandbox), [Harbor eval stack](https://www.langchain.com/blog/unified-stack-for-evaluating-agents), [Wiki Memory](https://www.langchain.com/blog/wiki-memory), [Dynamic Subagents](https://www.langchain.com/blog/introducing-dynamic-subagents-in-deep-agents)

- 우선순위: High
- 확인된 사실: LangChain은 Deep Agents의 dynamic subagent orchestration에서 agent-written code의 execution isolation, capability isolation, durable pauses를 강조했고, Harbor/Deep Agents/LangSmith 기반 long-running agent eval stack과 wiki memory 패턴을 제시했다.
- 해석: agent runtime은 code execution, long-running eval, memory curation을 동시에 갖춰야 한다. 특히 prompt injection을 전제로 한 capability boundary가 필요하다.
- AI 플랫폼 영향: sandbox 또는 capability-secure interpreter, resumable pause, long-running eval runner, memory ingestion/compaction/review workflow를 product primitive로 넣어야 한다.
- 후속 확인: Deep Agents의 untrusted-code 경계가 OS sandbox, capability token, interpreter API, tool proxy 중 어디서 enforce되는지 확인한다.

### Microsoft SkillOpt and reusable agent skills

참고 링크: [SkillOpt](https://www.microsoft.com/en-us/research/blog/skillopt-agent-skills-as-trainable-parameters/)

- 우선순위: Medium-High
- 확인된 사실: Microsoft Research는 SkillOpt를 통해 agent skill files를 특정 모델이나 harness에 과적합된 지시문이 아니라 transfer 가능한 task-solving procedure로 최적화하는 방향을 제시했다.
- 해석: skills는 사람이 한 번 쓰는 운영 문서가 아니라 benchmark/eval과 연결해 지속 개선하는 외부 state가 된다.
- AI 플랫폼 영향: skill registry, skill eval suite, transfer test, versioning, rollback, skill provenance가 필요하다.
- 후속 확인: SkillOpt류 접근을 Claude Skills, Codex skills, Deep Agents wiki memory와 같은 lifecycle로 통합할 수 있는지 비교한다.

## 4. 주요 동향과 중요도

| 중요도 | 동향 | 확인된 사실 | 해석 / 추론 | AI 플랫폼 적용 포인트 |
| --- | --- | --- | --- | --- |
| High | Scientific agent workbench | Claude Science, GeneBench-Pro 동시 신호. | 과학/생명과학은 agent product와 eval이 함께 성숙하는 도메인. | domain workbench, artifact audit, compute connector, expert rubric. |
| High | Graph multi-agent runtime | ADK Go 2.0 graph workflow, HITL, unified runtime. | multi-agent orchestration은 graph + state + human approval이 기본. | graph runtime, checkpoint, approval node, OTel trace. |
| High | Agent eval flywheel | Google AutoRaters/failure analysis, LangChain Harbor eval stack. | agent 품질은 독립 평가 서비스와 baseline comparison으로 운영된다. | eval runner, auto-rater, failure clustering, regression gate. |
| High | Untrusted agent code | LangChain이 dynamic subagent code 실행의 isolation 요구를 명시. | agent-written code는 prompt injection을 전제로 capability boundary가 필요. | capability-secure interpreter, durable pause, tool proxy. |
| Medium-High | Skill and memory optimization | SkillOpt, Wiki Memory 발표. | skills/memory는 static docs가 아니라 versioned, eval-backed state. | skill registry, memory wiki, review queue, transfer eval. |
| Medium | Agent M&A benchmark | Salesforce-Fin $3.6B prior signal 유지. | customer service agent는 독립 제품에서 platform consolidation 단계로 이동. | outcome pricing, channel coverage, CRM integration, incident rollback. |

## 5. AI Platform / Service Implications

1. Domain workbench를 agent product 단위로 정의한다.
   - 과학, 금융, 법무, 고객지원처럼 데이터·도구·평가가 강한 도메인은 단순 tool connector가 아니라 workbench template로 관리한다.
   - artifact lineage, dataset version, compute environment, approval, citation, export를 기본 메타데이터로 둔다.

2. Graph runtime과 eval runtime을 분리하지 않는다.
   - ADK Go 2.0의 graph workflow와 Google/LangChain eval 흐름을 같은 trace schema에 넣어야 한다.
   - node별 input/output, human approval, retry, pause/resume, failure cluster, before/after metric을 저장한다.

3. Agent-written code는 capability-secure execution으로 다룬다.
   - sandbox 여부와 무관하게 agent code가 접근 가능한 data/action capability를 명시적으로 넘긴다.
   - durable pause와 human input resume을 interpreter/runtime 계약에 포함한다.

4. Skills와 wiki memory를 eval-backed state로 관리한다.
   - skill file과 memory page는 owner, source, version, last-eval score, transfer test, rollback path를 가져야 한다.
   - raw source → compact wiki memory → agent-readable knowledge → eval loop를 pipeline으로 만든다.

5. Scientific/regulated domains에는 expert rubric과 access tier가 필요하다.
   - GeneBench-Pro류 benchmark는 모델 점수보다 judgment failure taxonomy와 uncertainty reporting이 중요하다.
   - bio, finance, legal, HR은 trusted access, data retention, evidence, human signoff를 같이 설계한다.

## 6. Recommended Actions

- 지금: `agent_run_trace` schema에 graph node, human-in-the-loop state, eval verdict, failure cluster, skill/memory version 필드를 추가한다.
- 지금: `domain_workbench` template 초안을 만든다. 필드는 domain, dataset connector, tool/package list, compute target, artifact lineage, approval tier, eval pack이다.
- 30일: Claude Science, GeneBench-Pro, LifeSciBench를 묶어 scientific agent workbench/eval 비교 문서를 만든다.
- 30일: ADK Go 2.0 graph runtime, LangChain Deep Agents, 기존 AX runtime을 graph/resume/HITL/eval 관점으로 비교한다.
- 계속: Salesforce-Fin 같은 agentic service M&A, NIST identity/authorization, model bio-risk access policy를 governance 신호로 추적한다.

## 7. Risks / Watch Items

| 리스크 | 설명 | 대응 |
| --- | --- | --- |
| Domain workbench overclaim | 과학 워크벤치가 auditable artifact 없이 demo 중심으로 보일 수 있다. | artifact lineage, reproducibility, expert review, dataset provenance 필수화. |
| Agent-written code escape | dynamic subagent code가 prompt injection으로 host/tool/data를 오용할 수 있다. | execution isolation, capability isolation, deny-by-default tool proxy, durable pause. |
| Eval self-gaming | optimizer가 자기 산출물을 직접 채점하면 metric gaming 위험이 커진다. | independent evaluator, held-out eval set, baseline comparison, manual audit. |
| Skill drift | 자동 최적화된 skill이 특정 harness나 benchmark에 과적합될 수 있다. | transfer test, rollback, provenance, multi-harness eval. |
| Scientific safety/access | bio-related tools와 모델이 research acceleration과 misuse risk를 동시에 키울 수 있다. | trusted access, action approval, evidence log, export controls, policy review. |

## 8. Sources

### 왜 이걸 정리하게 되었는가

- [Anthropic - Claude Science, an AI workbench for scientists](https://www.anthropic.com/news/claude-science-ai-workbench)
- [Anthropic - Introducing Claude Sonnet 5](https://www.anthropic.com/news/claude-sonnet-5)
- [OpenAI - Introducing GeneBench-Pro](https://openai.com/index/introducing-genebench-pro/)
- [Google - ADK Go 2.0](https://developers.googleblog.com/announcing-adk-go-20/)
- [Google - Agent Quality Flywheel](https://developers.googleblog.com/driving-the-agent-quality-flywheel-from-your-coding-agent/)

### 딥리서치 출처

- [OpenAI - Inside GeneBench-Pro](https://openai.com/index/genebench-pro/case-studies/)
- [GeneBench-Pro paper](https://cdn.openai.com/pdf/21938268-21af-442f-af93-3b2249afb241/genebench-pro.pdf)
- [LangChain - Running Untrusted Agent Code Without a Sandbox](https://www.langchain.com/blog/running-untrusted-agent-code-without-a-sandbox)
- [LangChain - Harbor x LangChain: A Unified Stack for Evaluating Agents](https://www.langchain.com/blog/unified-stack-for-evaluating-agents)
- [LangChain - Wiki Memory](https://www.langchain.com/blog/wiki-memory)
- [LangChain - Introducing Dynamic Subagents in Deep Agents](https://www.langchain.com/blog/introducing-dynamic-subagents-in-deep-agents)
- [Microsoft Research - SkillOpt](https://www.microsoft.com/en-us/research/blog/skillopt-agent-skills-as-trainable-parameters/)
- [Salesforce - Definitive agreement to acquire Fin](https://www.salesforce.com/news/press-releases/2026/06/15/salesforce-signs-definitive-agreement-to-acquire-fin/)
- [NIST - Software and AI Agent Identity and Authorization](https://www.nccoe.nist.gov/sites/default/files/2026-02/accelerating-the-adoption-of-software-and-ai-agent-identity-and-authorization-concept-paper.pdf)
