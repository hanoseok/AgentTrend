# 0700 Agent Trend Brief

- 작성 시점: 2026-06-28 07:00 KST
- 범위: Agent products/platforms, model/tool usage, frameworks/open source, enterprise adoption, funding, benchmarks/evals, policy/governance, vendor/community signals
- HTML: `report/agent-trend-brief/2026-06-28-0700/`

## 1. Executive Summary

한 줄 결론: 2026-06-28 기준 AI agent 트렌드는 "대화형 도구"에서 "위임 가능한 노동 단위"로 이동했고, 동시에 agent discovery, registry, identity, authorization, eval/simulation, enterprise governance가 핵심 플랫폼 레이어로 부상했다.

Sources: [OpenAI - How agents are transforming work](https://openai.com/index/how-agents-are-transforming-work/), [Anthropic Economic Index June 2026](https://www.anthropic.com/research/economic-index-june-2026-report), [Anthropic - How Claude Code is used in practice](https://www.anthropic.com/research/claude-code-expertise), [Google - Agentic Resource Discovery](https://developers.googleblog.com/announcing-the-agentic-resource-discovery-specification/), [GitHub - Agent Finder](https://github.blog/changelog/2026-06-17-agent-finder-for-github-copilot-now-available/), [Microsoft Build 2026](https://blogs.microsoft.com/blog/2026/06/02/microsoft-build-2026-be-yourself-at-work/), [Patronus AI Series B](https://www.prnewswire.com/news-releases/patronus-ai-raises-50-million-series-b-and-unveils-first-digital-world-models-for-ai-agent-training-and-simulation-302811248.html), [NIST concept paper](https://www.nccoe.nist.gov/sites/default/files/2026-02/accelerating-the-adoption-of-software-and-ai-agent-identity-and-authorization-concept-paper.pdf), [LangChain/LangGraph 1.0](https://www.langchain.com/blog/langchain-langgraph-1dot0)

핵심 판단:

1. OpenAI의 Codex 경제 연구는 agent 사용이 짧은 질의가 아니라 30분-8시간 이상 사람 업무로 추정되는 long-horizon delegated task로 이동 중임을 보여준다. 특히 비개발자와 법무/재무/리크루팅 등 비엔지니어 조직까지 Codex 사용이 확대된 점이 중요하다.
2. Anthropic의 Claude Code 분석은 agent 성공률이 사용자 전문성과 강하게 연결됨을 보여준다. 즉 "더 자율적인 agent"만으로는 부족하고, task framing, domain knowledge, review/eval scaffold가 성능의 일부가 된다.
3. Google의 Agentic Resource Discovery와 GitHub Agent Finder는 agent discovery/registry가 vendor UX 기능을 넘어 생태계 표준화 레이어로 이동하는 신호다.
4. Microsoft Agent 365, NIST agent identity/authorization concept, GitHub Agent Finder는 enterprise agent 운영의 중심이 "누가 어떤 agent를 어떤 권한으로 실행하는가"로 이동함을 보여준다.
5. Patronus의 digital world model/eval 투자 신호는 agent benchmark가 정답률 테스트에서 simulated environment, stress test, rollback/replay 가능한 운영 평가로 이동한다는 의미가 있다.

## 2. Source Coverage

| 소스 | 확인 여부 | 유의미한 변화 | 신뢰도 |
| --- | --- | --- | --- |
| OpenAI | Checked | Codex economic research가 agentic work의 task horizon, non-developer adoption, cross-functional work expansion을 수치로 제시. | Official / Confirmed |
| Anthropic | Checked | Economic Index와 Claude Code expertise report가 work artifact usage와 agentic coding success factor를 보강. | Official / Confirmed |
| Google Developers | Checked | Agentic Resource Discovery spec와 Jules eval 글이 discovery/eval 표준화를 강화. | Official / Confirmed |
| GitHub Blog / Changelog | Checked | Agent Finder가 Google ARD 기반으로 Copilot Chat에서 agent 탐색을 지원. | Official / Confirmed |
| Microsoft | Checked | Build 2026에서 Agent 365, Work IQ, Agent Factory를 enterprise agent governance/creation layer로 제시. | Official / Confirmed |
| LangChain | Checked | LangChain/LangGraph 1.0 발표로 graph-based agent framework가 production 안정화 단계로 이동. | Official / Confirmed |
| Patronus AI | Checked | $50M Series B와 digital world models를 agent training/simulation/eval로 연결. | Company announcement / Confirmed funding announcement |
| NIST NCCoE | Checked | software and AI agent identity/authorization concept paper가 agent identity governance를 정책/표준화 의제로 올림. | Government / Concept |
| Community | Lightweight check | Agent Finder, ARD, LangGraph, coding agent evaluation이 개발자 agent UX의 반복 논점으로 관찰됨. | Community signal / Needs confirmation |

## 3. Major Trends and Importance

| 중요도 | 동향 | 확인된 사실 | 해석 / 추론 | AI 플랫폼 적용 포인트 |
| --- | --- | --- | --- | --- |
| High | Agentic work가 long-horizon delegated task로 이동 | OpenAI는 2026-06-25 Codex 연구에서 sampled individual users 중 80.6%가 30분 초과, 70.2%가 1시간 초과, 25.6%가 8시간 초과 사람 업무로 추정되는 Codex 요청을 적어도 한 번 했다고 보고. | agent는 chat answer보다 job queue/work order에 가까워진다. | agent job API, progress state, cancel/retry, artifact review, budget, owner, audit log가 기본 primitive가 되어야 한다. |
| High | 비개발자 adoption이 agent 전략의 핵심 변수 | OpenAI는 비개발자 individual users 137x, organizational users 189x 성장과 Legal/Finance/Recruiting의 Codex primary tool 전환을 보고. | coding agent가 개발 도구를 넘어 knowledge work automation layer가 되는 중이다. | no-code/low-code agent task templates, safe technical execution, business user approval UX가 필요하다. |
| High | Agent discovery/registry 표준화 | Google은 Agentic Resource Discovery spec을 발표했고, GitHub Agent Finder는 Copilot Chat에서 ARD 기반 agent 탐색을 지원한다고 발표. | agent marketplace는 단순 목록이 아니라 machine-readable capability, trust, permission, routing metadata를 요구한다. | Agent Card/ARD registry, capability schema, trust tier, owner, auth scope, compatibility validation을 설계해야 한다. |
| High | Enterprise agent governance가 제품 카테고리로 분리 | Microsoft Build 2026은 Agent 365와 Agent Factory를 agent 관리/생성/운영 레이어로 제시. NIST는 AI agent identity/authorization concept를 별도 주제로 다룸. | 조직은 agent 수가 늘어날수록 "사용 가능한 agent"보다 "관리 가능한 agent"를 먼저 요구한다. | agent inventory, identity, lifecycle, policy, authorization, decommission, incident trail이 필요하다. |
| High | Agent success는 사용자 전문성/검증 scaffolding에 의존 | Anthropic Claude Code expertise report는 novice session의 verified success와 intermediate/expert session의 success 격차를 제시. | agent 성능은 모델만의 속성이 아니라 task framing, domain constraints, reviewer expertise의 함수다. | expert playbooks, structured prompt templates, eval harness, review checklist, domain-specific guardrails를 붙여야 한다. |
| Medium | Eval이 simulated environment/stress test로 이동 | Patronus는 digital world models for AI agent training and simulation을 발표했고, Google Jules 글도 실제 productivity metric 측정을 강조. | benchmark는 "정답을 맞혔는가"에서 "환경 안에서 안전하게 끝냈는가"로 이동한다. | task simulation, adversarial eval, regression suite, rollback/replay, production shadow mode가 중요하다. |
| Medium | Framework/open source는 안정화와 graph runtime 경쟁 | LangChain/LangGraph 1.0 발표는 agent framework가 production API 안정성, graph runtime, deployment story를 강조하는 단계로 이동했음을 보여준다. | 실험용 agent framework에서 운영 가능한 orchestration framework로 기준이 올라간다. | state graph, checkpoint, human-in-loop, durable execution, observability를 framework 선택 기준으로 본다. |

## 4. Product / Platform Signals

### 4.1 OpenAI Codex: agentic labor as measurable work

Confirmed facts:

- OpenAI는 2026-06-25 Codex economic research에서 agentic AI가 single interaction이 아니라 minutes/hours 단위 delegated task로 이동한다고 설명했다.
- 2026년 5월 기준 sampled individual users 중 80.6%는 30분 초과, 70.2%는 1시간 초과, 25.6%는 8시간 초과 사람 업무로 추정되는 Codex 요청을 적어도 한 번 했다.
- OpenAI 내부에서는 Codex가 Engineering뿐 아니라 Legal, Finance, Recruiting에서도 primary AI tool이 되었다고 보고했다.

Interpretation:

agent product는 이제 "더 좋은 대화 UI"보다 "여러 개의 병렬 delegated job을 관리하는 work operating system"에 가깝다. 사용자별 daily agent hours, queued jobs, artifacts, review queue, delegated-risk budget 같은 지표가 필요해진다.

### 4.2 Anthropic Claude Code: expertise remains a performance multiplier

Confirmed facts:

- Anthropic은 Claude Code interactive sessions 분석에서 user expertise가 session success와 강하게 연결된다고 보고했다.
- novice-rated session은 가장 엄격한 verified success 기준 15%, partial success 77%였고, intermediate 이상은 verified success 28-33%, partial success 91-92%로 보고됐다.
- Economic Index June 2026은 work-related conversations에서 documents/reports, explanations, email drafts, analyses/summaries가 주요 artifact임을 보여준다.

Interpretation:

agent가 더 자율적이어도 사용자의 task decomposition, domain knowledge, acceptance criteria가 성능을 좌우한다. 플랫폼은 "초보자가 agent에게 일을 잘 맡기게 하는 scaffold"를 제품화해야 한다.

### 4.3 Google ARD + GitHub Agent Finder: discovery becomes infrastructure

Confirmed facts:

- Google은 Agentic Resource Discovery specification을 발표했다.
- GitHub는 2026-06-17 Agent Finder를 발표했고, Copilot Chat에서 ARD 기반으로 repository와 organization agent를 찾는 흐름을 제시했다.

Interpretation:

Agent discovery는 marketplace 검색이 아니라 orchestration 문제다. 어떤 agent가 어떤 task/capability를 갖고, 어떤 scope에서, 어떤 trust/policy로 호출 가능한지 machine-readable해야 한다.

### 4.4 Microsoft Agent 365: agent management becomes enterprise surface

Confirmed facts:

- Microsoft Build 2026 발표는 Agent 365, Agent Factory, Work IQ를 workplace agent operating layer로 제시했다.

Interpretation:

기업 도입은 agent 생성보다 agent inventory, policy, identity, user context, audit, lifecycle 관리가 먼저 병목이 된다. 이는 NIST의 agent identity/authorization 의제와도 맞물린다.

### 4.5 Patronus / Jules / LangGraph: eval and runtime maturity

Confirmed facts:

- Patronus는 2026-06-26 $50M Series B와 digital world models for AI agent training and simulation을 발표했다.
- Google은 Jules 평가에서 단순 activity metric보다 developer productivity에 가까운 성과 측정을 강조했다.
- LangChain은 LangChain/LangGraph 1.0을 발표하며 agent/graph framework 안정화 흐름을 보여준다.

Interpretation:

agent 운영의 핵심은 "실제 업무 환경에서 실패 없이 완료하는가"다. framework와 eval vendor 모두 stateful execution, observable trajectory, simulated environment, domain eval로 이동한다.

## 5. AI Platform / Service Implications

| 영역 | 적용 포인트 | 우선순위 |
| --- | --- | --- |
| Agent job layer | 모든 agent 실행을 job/task object로 저장: owner, prompt, scope, tools, budget, progress, artifacts, audit. | High |
| Agent registry/discovery | ARD/Agent Card 호환 metadata, capability, trust tier, source, version, owner, permission policy를 저장. | High |
| Identity and authorization | human user, service account, agent identity, delegated authorization, tool scope를 분리. | High |
| Review surface | chat transcript 대신 PR, report, ticket, case, deployment candidate, spreadsheet diff 같은 artifact review surface 제공. | High |
| Expertise scaffold | novice user용 task template, acceptance criteria builder, reviewer checklist, domain examples 제공. | High |
| Eval/simulation | regression eval, adversarial scenarios, digital world/sandbox, replay trace, failure taxonomy 구축. | Medium |
| Framework selection | LangGraph/ADK/Agents SDK/Copilot custom agents 등은 state graph, checkpoint, human-in-loop, observability, registry 연동 기준으로 비교. | Medium |

## 6. Watch Signals

1. ARD/Agent Finder가 GitHub 외 다른 IDE, agent marketplace, enterprise catalog에 채택되는지.
2. OpenAI Codex와 Claude Code의 non-engineering use case가 legal/finance/recruiting/customer support에서 얼마나 안정적으로 반복되는지.
3. Microsoft Agent 365류 관리 계층이 실제 agent inventory, policy, incident response, decommissioning까지 제공하는지.
4. NIST agent identity/authorization 논의가 vendor implementation, OAuth/MCP/A2A/ARD metadata에 반영되는지.
5. Patronus류 digital world model이 benchmark marketing을 넘어 enterprise procurement/eval 기준이 되는지.
6. LangGraph 1.0, Google ADK/AX, OpenAI Agents SDK, Cline/Kimi/Copilot agent runtime이 durable execution과 eval을 어떤 방식으로 수렴시키는지.
7. Agent 사용량이 늘면서 cost explosion, stale intent, tool permission leakage, artifact verification failure 사례가 증가하는지.

## 7. Follow-up Checks

- ARD + GitHub Agent Finder + Microsoft Agent 365를 묶어 "Agent Registry / Discovery / Governance" 딥다이브 후보로 등록한다.
- OpenAI Codex economic paper PDF의 methodology와 task horizon estimator를 별도 확인해 내부 agent productivity metric에 쓸 수 있는지 본다.
- Anthropic Claude Code expertise report를 기준으로 novice/intermediate/expert별 agent onboarding checklist를 만든다.
- NIST concept paper를 agent identity data model, delegated auth, audit log 설계 기준으로 읽는다.
- 이번 회차에서는 큰 M&A 신호는 확인하지 못했다. Funding은 Patronus AI Series B를 대표 신호로 기록하고, agent identity/eval/security funding을 계속 추적한다.

## 8. Inline Evidence

| Claim | Evidence | Label |
| --- | --- | --- |
| agent는 long-horizon delegated task로 이동 중이다. | OpenAI Codex research의 task horizon, non-developer growth, internal departmental adoption. | Confirmed fact |
| user expertise는 Claude Code session success에 영향을 준다. | Anthropic Claude Code expertise report의 novice vs intermediate/expert success 비교. | Confirmed fact |
| agent discovery가 표준화되고 있다. | Google ARD spec, GitHub Agent Finder. | Confirmed fact |
| enterprise agent 운영 계층이 독립 제품 영역이 되고 있다. | Microsoft Agent 365/Agent Factory, NIST identity/authorization concept. | Confirmed + interpretation |
| eval은 simulated world와 productivity metric으로 이동 중이다. | Patronus digital world models, Google Jules productivity measurement, LangGraph 1.0 stability signal. | Confirmed + interpretation |

## 9. Sources

### 왜 이걸 정리하게 되었는가

- 07:00 KST 자동 브리프는 agent 제품/플랫폼, 모델/도구 사용, 프레임워크/오픈소스, 기업 도입, 투자, 벤치마크, 규제/정책, 벤더/커뮤니티 신호를 매일 갱신하기 위한 운영 산출물이다.
- 이번 회차는 2026-06-25~2026-06-28 사이 확인된 official/vendor source가 agent work, discovery, governance, eval로 집중되어 있어 이 흐름을 중심으로 정리했다.

### 딥리서치 출처

- [OpenAI - How agents are transforming work](https://openai.com/index/how-agents-are-transforming-work/)
- [Anthropic - Economic Index June 2026 report](https://www.anthropic.com/research/economic-index-june-2026-report)
- [Anthropic - How Claude Code is used in practice](https://www.anthropic.com/research/claude-code-expertise)
- [Google Developers - Agentic Resource Discovery specification](https://developers.googleblog.com/announcing-the-agentic-resource-discovery-specification/)
- [Google Developers - Measuring what matters with Jules](https://developers.googleblog.com/measuring-what-matters-with-jules/)
- [GitHub Changelog - Agent Finder for GitHub Copilot](https://github.blog/changelog/2026-06-17-agent-finder-for-github-copilot-now-available/)
- [Microsoft Build 2026 - Be yourself at work](https://blogs.microsoft.com/blog/2026/06/02/microsoft-build-2026-be-yourself-at-work/)
- [Patronus AI - Series B and digital world models](https://www.prnewswire.com/news-releases/patronus-ai-raises-50-million-series-b-and-unveils-first-digital-world-models-for-ai-agent-training-and-simulation-302811248.html)
- [NIST NCCoE - Software and AI Agent Identity and Authorization concept paper](https://www.nccoe.nist.gov/sites/default/files/2026-02/accelerating-the-adoption-of-software-and-ai-agent-identity-and-authorization-concept-paper.pdf)
- [LangChain - LangChain/LangGraph 1.0](https://www.langchain.com/blog/langchain-langgraph-1dot0)
