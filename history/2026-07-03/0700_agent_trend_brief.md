# 0700 Agent Trend Brief

- 작성 시점: 2026-07-03 07:00 KST
- 범위: Copilot agent observability, AI spend governance, model routing and open-weight model policy, browser/vision agent tools, MCP-backed planning metadata, agent memory and cost observability
- HTML: `report/agent-trend-brief/2026-07-03-0700/`

## 1. Executive Summary

참고 링크: [Copilot agent session streaming](https://github.blog/changelog/2026-07-02-copilot-agent-session-streaming-is-now-in-public-preview/), [GitHub AI credit pools](https://github.blog/changelog/2026-07-02-cost-centers-now-support-included-usage-caps/), [Copilot CLI auto model selection](https://github.blog/changelog/2026-07-01-copilot-cli-auto-model-selection-routes-based-on-task/), [Enterprise default auto model selection](https://github.blog/changelog/2026-07-01-enterprises-can-default-to-auto-model-selection/), [Kimi K2.7 in Copilot](https://github.blog/changelog/2026-07-01-kimi-k2-7-is-now-available-in-github-copilot/), [Copilot browser tools GA](https://github.blog/changelog/2026-07-01-browser-tools-for-github-copilot-in-vs-code-are-generally-available/), [Copilot vision GA](https://github.blog/changelog/2026-07-01-copilot-vision-is-generally-available/), [Microsoft Memora](https://www.microsoft.com/en-us/research/blog/memora-a-harmonic-memory-representation-balancing-abstraction-and-specificity/), [LangChain coding agent cost](https://www.langchain.com/blog/fix-your-coding-agent-bill)

오늘 핵심은 enterprise coding agent가 "쓸 수 있는 기능"에서 "관측하고, 비용을 통제하고, 모델을 정책적으로 라우팅하는 운영 시스템"으로 이동한다는 점이다. GitHub는 Copilot agent session streaming, AI credit pools, auto model selection, enterprise managed settings, Kimi K2.7 open-weight model, browser tools/vision GA를 연속적으로 내놓았다. Microsoft Research의 Memora와 LangChain의 coding agent spend 분석은 장기 memory와 cost observability가 같은 문제의 두 면임을 보여준다.

핵심 판단:

1. Agent observability가 prompt/response/tool-call 단위로 내려왔다. Copilot agent session streaming은 enterprise AI 사용 기록을 streaming endpoint 또는 REST API로 다룰 수 있게 한다.
2. AI spend governance가 seat 관리에서 session, cost center, included credit pool, model route 정책으로 세분화되고 있다.
3. Model routing은 단순 model picker가 아니라 task difficulty, model health, utilization, policy, credit rate를 결합하는 운영 문제다.
4. Open-weight coding model이 Copilot model picker에 들어오면서 model provenance, hosting boundary, admin opt-in, compliance review가 중요해졌다.
5. Browser/vision tools GA는 coding agent가 live web app과 visual artifacts를 다루는 product QA/diagnostic agent로 확장되는 신호다.
6. Agent memory와 cost observability는 장기 agent를 운영하기 위한 같은 control plane에 들어가야 한다.

이번 회차 결론: AI 플랫폼은 `agent_session_ledger`, `ai_spend_policy`, `model_routing_policy`, `tool_surface_policy`, `memory_index_policy`를 분리된 기능이 아니라 agent operations control plane으로 묶어야 한다.

## 2. Source Coverage

| 소스 | 확인 여부 | 유의미한 변화 | 신뢰도 |
| --- | --- | --- | --- |
| GitHub Changelog | Checked | Copilot agent session streaming, AI credit pools, auto model selection, enterprise default settings, Kimi K2.7, browser tools, vision, issue fields MCP integration 확인. | Official / Confirmed |
| Microsoft Research | Checked | Memora가 long-horizon agent memory에서 rich memory와 lightweight retrieval abstraction을 분리하는 접근을 제시. | Research / Confirmed |
| LangChain | Checked | coding agent spend fragmentation, OpenWiki, Deep Agents/RLM 흐름 확인. 비용 관측은 vendor analysis로 해석. | Vendor / Confirmed blog |
| Google Developers | Checked | Genkit Agents API, ADK 2.0, Agent Quality Flywheel은 전일 신호로 유지. | Official / Prior signal |
| Anthropic / OpenAI | Checked | Fable 5/Sonnet 5/Claude Science/GeneBench-Pro는 전일 governance/domain workbench 신호로 유지. | Official / Prior signal |
| Policy / Standards | Checked | NIST AI Agent Standards Initiative, EU AI Act service desk는 agent identity/authorization/transparency prior signal로 유지. | Official / Prior signal |

## 3. Project / Product / Policy Alerts

### GitHub Copilot agent session streaming

참고 링크: [Copilot agent session streaming](https://github.blog/changelog/2026-07-02-copilot-agent-session-streaming-is-now-in-public-preview/)

- 우선순위: High
- 확인된 사실: GitHub는 Copilot agent session activity, 즉 prompts, responses, tool calls에 대한 visibility를 제공하는 session streaming public preview를 발표했다. 데이터 접근 방식은 streaming endpoint와 REST API이며, AI Controls의 Copilot 하위 페이지에서 usage records streaming/API를 enable해야 한다.
- 해석: agent observability의 최소 단위가 "요청 수"에서 session event stream으로 내려왔다. enterprise는 agent의 tool call과 response를 감사·비용·품질·보안 목적으로 재구성할 수 있어야 한다.
- AI 플랫폼 영향: `agent_session_ledger`에 prompt, response, tool_call, model, cost, policy verdict, trace id, user/org/cost center, artifact link를 기록해야 한다.
- 후속 확인: streaming endpoint의 schema, retention, privacy redaction, SIEM/OTel export 가능 여부를 확인한다.

### GitHub AI credit pools and session spend controls

참고 링크: [AI credit pools](https://github.blog/changelog/2026-07-02-cost-centers-now-support-included-usage-caps/), [AI credit session limits](https://github.blog/changelog/2026-07-01-set-ai-credit-session-limits-in-copilot-cli-and-sdk/), [LangChain coding agent bill](https://www.langchain.com/blog/fix-your-coding-agent-bill)

- 우선순위: High
- 확인된 사실: GitHub는 cost center별 included AI credit pool cap을 REST API로 제공한다고 밝혔다. cost center budget은 metered phase 지출을 제한하고, AI credit pool은 pooled included credits 사용량을 제한한다. GitHub는 Copilot CLI/SDK session limit preview도 제공한다. LangChain은 여러 coding agent 도구가 서로 다른 format으로 비용/활동 데이터를 남겨 feature 단위 ROI를 보기 어렵다고 분석했다.
- 해석: AI 비용 통제는 "월간 예산" 하나로 부족하다. included credits, metered overage, session-level cap, model route, tool retry loop, cost center chargeback이 모두 필요하다.
- AI 플랫폼 영향: `ai_spend_policy`는 included pool, metered budget, session cap, model price table, auto-routing discount, overage behavior, alert threshold를 함께 가져야 한다.
- 후속 확인: Copilot usage records와 cost center/budget API가 같은 trace id 또는 billing key로 join 가능한지 확인한다.

### Auto model selection and open-weight model policy

참고 링크: [Copilot CLI auto model selection](https://github.blog/changelog/2026-07-01-copilot-cli-auto-model-selection-routes-based-on-task/), [Enterprise default auto model selection](https://github.blog/changelog/2026-07-01-enterprises-can-default-to-auto-model-selection/), [Kimi K2.7 in Copilot](https://github.blog/changelog/2026-07-01-kimi-k2-7-is-now-available-in-github-copilot/)

- 우선순위: High
- 확인된 사실: Copilot CLI auto model selection은 model availability/reliability signals와 task dimensions를 고려해 모델을 고른다. 사용자는 `/model`로 수동 전환할 수 있고, admin model policies를 따른다. Enterprise는 `managed-settings.json`에서 new conversations의 기본 model을 `auto`로 설정할 수 있다. Kimi K2.7 Code는 Copilot model picker의 첫 open-weight selectable model로 GA됐고, GitHub가 Microsoft Azure에서 호스팅한다. Business/Enterprise에서는 기본 off이며 admin enable이 필요하다.
- 해석: model choice는 UX 선택이 아니라 policy-controlled routing layer가 됐다. open-weight model이 enterprise coding agent 표면에 들어오면 provenance, hosting, data governance, quality regression, cost table이 같은 설정에 묶여야 한다.
- AI 플랫폼 영향: `model_routing_policy`에 task classifier, model health, policy allowlist, open-weight flag, hosting boundary, credit rate, fallback rule, user override rule을 포함한다.
- 후속 확인: auto model selection의 routing explanation, quality/no-regression eval, admin audit log 제공 범위를 확인한다.

### Browser tools, vision, and MCP-backed issue fields

참고 링크: [Browser tools GA](https://github.blog/changelog/2026-07-01-browser-tools-for-github-copilot-in-vs-code-are-generally-available/), [Copilot vision GA](https://github.blog/changelog/2026-07-01-copilot-vision-is-generally-available/), [Issue fields MCP integration](https://github.blog/changelog/2026-07-02-issue-fields-are-now-generally-available/)

- 우선순위: High
- 확인된 사실: GitHub Copilot browser tools for VS Code가 GA되어 agents가 live web apps를 탐색하고 결과를 chat에 반영할 수 있다. Copilot vision은 images/PDFs를 chat prompt에 붙여 code와 함께 reasoning할 수 있게 GA됐다. Issue fields는 GitHub MCP server를 통해 AI tools가 issue 생성/수정 시 field values를 읽고 설정할 수 있다.
- 해석: coding agent는 code-only helper에서 live browser QA, visual artifact analysis, planning metadata update까지 다루는 IDE-embedded operator가 되고 있다.
- AI 플랫폼 영향: browser permission, domain allowlist, screenshot/artifact retention, visual input retention, MCP field write permission, issue metadata audit를 같은 tool surface policy로 관리해야 한다.
- 후속 확인: browser tools의 network/domain controls, session recording, attachment retention, MCP field write audit semantics를 확인한다.

### Memora and cost-aware long-horizon memory

참고 링크: [Microsoft Memora](https://www.microsoft.com/en-us/research/blog/memora-a-harmonic-memory-representation-balancing-abstraction-and-specificity/), [LangChain OpenWiki](https://www.langchain.com/blog/introducing-openwiki-an-open-source-agent-for-repo-documentation), [LangChain coding agent cost](https://www.langchain.com/blog/fix-your-coding-agent-bill)

- 우선순위: Medium-High
- 확인된 사실: Microsoft Research는 Memora가 long-horizon agent productivity를 높이기 위해 rich memory content와 lightweight abstraction/cue anchors를 분리한다고 설명했다. LangChain은 OpenWiki를 codebase documentation agent/CLI로 공개했고, coding agent 비용 관측 문제를 tool 간 fragmented traces 문제로 제시했다.
- 해석: 장기 agent의 병목은 context window만이 아니라 memory retrieval cost, documentation freshness, trace normalization, task-level ROI다.
- AI 플랫폼 영향: memory page, repo docs, session traces, cost ledger를 연결해 "이 agent가 왜 이 context를 썼고 얼마를 썼는가"를 설명해야 한다.
- 후속 확인: Memora의 benchmark 재현성, OpenWiki의 update lifecycle, LangSmith trace normalization의 open schema 제공 여부를 확인한다.

## 4. 주요 동향과 중요도

| 중요도 | 동향 | 확인된 사실 | 해석 / 추론 | AI 플랫폼 적용 포인트 |
| --- | --- | --- | --- | --- |
| High | Agent session observability | Copilot agent session streaming public preview. | prompt/response/tool-call stream이 enterprise audit primitive가 된다. | `agent_session_ledger`, OTel/SIEM export, privacy redaction. |
| High | AI spend governance | AI credit pools, session limits, cost center controls. | agent 비용은 seat가 아니라 session/cost center/model route 단위로 통제된다. | `ai_spend_policy`, budget vs included credits, overage rule. |
| High | Policy-controlled model routing | Copilot CLI auto model selection, enterprise default auto. | model routing은 health, task, policy, credit rate를 결합한다. | `model_routing_policy`, route explanation, fallback. |
| High | Open-weight model in enterprise agent surface | Kimi K2.7 Code in Copilot, admin opt-in. | open-weight model도 hosted enterprise product의 governance 대상이다. | provenance, hosting boundary, admin allowlist, quality eval. |
| High | Browser/vision tool surface | Browser tools and Copilot vision GA. | coding agent가 live app/browser/visual artifact까지 다룬다. | browser domain policy, attachment retention, visual trace. |
| Medium-High | MCP-backed planning metadata | GitHub issue fields accessible through MCP server. | agent가 issue metadata를 직접 쓰며 planning system을 조작한다. | MCP write permission, field-level audit, workflow policy. |
| Medium-High | Long-horizon memory efficiency | Memora, OpenWiki, LangChain cost observability. | memory and docs are cost/quality control surfaces. | memory index, repo wiki, trace-cost join, retrieval budget. |

## 5. AI Platform / Service Implications

1. `agent_session_ledger`를 product core로 만든다.
   - 모든 prompt, response, tool call, browser action, visual attachment, MCP write, model route, cost event를 session timeline에 기록한다.
   - enterprise export는 streaming endpoint, REST batch, warehouse sink, SIEM/OTel sink를 지원해야 한다.

2. `ai_spend_policy`를 seat/plan 바깥으로 확장한다.
   - included credit pool, metered budget, session cap, cost center, team/project, model price table, retry-loop breaker를 하나의 정책으로 둔다.
   - agent run이 중단된 이유가 budget cap인지, admin policy인지, model health인지 설명 가능해야 한다.

3. `model_routing_policy`와 `model_access_governance`를 합친다.
   - auto routing은 task classifier, model health, context length, tool orchestration, cost, admin allowlist, open-weight flag, hosting region을 함께 본다.
   - user override를 허용하더라도 route decision과 override reason을 감사 로그에 남긴다.

4. Browser/vision/MCP write를 `tool_surface_policy`로 관리한다.
   - live browser는 domain allowlist, credential handoff, screenshot retention, download/upload boundary가 필요하다.
   - vision inputs는 image/PDF retention, OCR leakage, customer data class를 정책화한다.
   - MCP writes는 field-level permission과 approval flow를 둔다.

5. Memory/docs/cost를 하나의 improvement loop로 연결한다.
   - repo documentation, wiki memory, session trace, eval verdict, cost ledger를 같은 task id로 연결한다.
   - long-horizon agent가 어떤 memory를 검색했고, 그 retrieval이 품질과 비용에 어떤 영향을 줬는지 측정한다.

## 6. Recommended Actions

- 지금: `agent_session_ledger` schema를 만든다. 필드는 session_id, actor, prompt, response, tool_call, browser_action, attachment, MCP_write, model_route, cost_event, policy_verdict, trace_id다.
- 지금: `ai_spend_policy` schema를 만든다. 필드는 included_credit_pool, metered_budget, session_limit, cost_center, model_price, overage_behavior, alert_threshold다.
- 지금: `model_routing_policy` schema에 task_classifier, model_health, open_weight_flag, hosting_boundary, admin_allowlist, user_override, route_explanation을 추가한다.
- 30일: Copilot session streaming, LangSmith trace model, OpenTelemetry semantic conventions를 비교해 agent trace export 표준 후보를 만든다.
- 30일: browser tools, vision, MCP field write를 묶은 tool surface risk matrix를 만든다.
- 계속: Memora/OpenWiki/Wiki Memory/SkillOpt를 memory-docs-skill lifecycle 비교로 확장한다.

## 7. Risks / Watch Items

| 리스크 | 설명 | 대응 |
| --- | --- | --- |
| Trace privacy leakage | prompt, response, tool call, visual attachment가 모두 session stream에 들어가면 민감 정보가 관측 시스템으로 이동한다. | redaction, field-level retention, tenant boundary, access log. |
| Spend policy fragmentation | included credit pool, budget, session cap, model price가 따로 관리되면 비용 원인 분석이 어렵다. | unified spend policy, per-session cost attribution, chargeback key. |
| Auto routing opacity | auto model selection이 비용 절감과 품질 유지 사이에서 왜 특정 모델을 골랐는지 설명하지 않으면 신뢰가 낮다. | route explanation, eval baseline, user/admin override log. |
| Open-weight compliance gap | open-weight model이 hosted product에 들어와도 기업은 provenance, hosting, data handling을 확인해야 한다. | open-weight flag, model card, hosted boundary, enablement approval. |
| Browser/vision tool overreach | live browser와 image/PDF attachment가 credential, customer data, UI state를 노출할 수 있다. | domain allowlist, takeover prompts, attachment retention policy, screenshot audit. |
| MCP write blast radius | issue fields처럼 planning metadata를 agent가 직접 쓰면 workflow automation과 충돌할 수 있다. | MCP write scope, approval, audit, rollback. |
| Memory-cost drift | long-horizon memory가 늘수록 검색 비용과 stale context 위험이 같이 증가한다. | memory pruning, retrieval budget, freshness score, eval-backed memory updates. |

## 8. Sources

### 왜 이걸 정리하게 되었는가

- [GitHub - Copilot agent session streaming](https://github.blog/changelog/2026-07-02-copilot-agent-session-streaming-is-now-in-public-preview/)
- [GitHub - Cost centers now support AI credit pools](https://github.blog/changelog/2026-07-02-cost-centers-now-support-included-usage-caps/)
- [GitHub - Copilot CLI auto model selection](https://github.blog/changelog/2026-07-01-copilot-cli-auto-model-selection-routes-based-on-task/)
- [GitHub - Kimi K2.7 Code in Copilot](https://github.blog/changelog/2026-07-01-kimi-k2-7-is-now-available-in-github-copilot/)
- [GitHub - Browser tools for Copilot in VS Code GA](https://github.blog/changelog/2026-07-01-browser-tools-for-github-copilot-in-vs-code-are-generally-available/)

### 딥리서치 출처

- [GitHub - Enterprises can default to auto model selection](https://github.blog/changelog/2026-07-01-enterprises-can-default-to-auto-model-selection/)
- [GitHub - Set AI credit session limits in Copilot CLI and SDK](https://github.blog/changelog/2026-07-01-set-ai-credit-session-limits-in-copilot-cli-and-sdk/)
- [GitHub - Copilot vision GA](https://github.blog/changelog/2026-07-01-copilot-vision-is-generally-available/)
- [GitHub - Issue fields GA with MCP integration](https://github.blog/changelog/2026-07-02-issue-fields-are-now-generally-available/)
- [Microsoft Research - Memora](https://www.microsoft.com/en-us/research/blog/memora-a-harmonic-memory-representation-balancing-abstraction-and-specificity/)
- [LangChain - Your coding agent bill doubled](https://www.langchain.com/blog/fix-your-coding-agent-bill)
- [LangChain - OpenWiki](https://www.langchain.com/blog/introducing-openwiki-an-open-source-agent-for-repo-documentation)
- [Google - Genkit Agents API](https://developers.googleblog.com/build-agentic-full-stack-apps-with-genkit/)
- [Google - ADK Go 2.0](https://developers.googleblog.com/announcing-adk-go-20/)
- [Anthropic - Redeploying Fable 5](https://www.anthropic.com/news/redeploying-fable-5)
- [NIST - AI Agent Standards Initiative](https://www.nist.gov/news-events/news/2026/02/announcing-ai-agent-standards-initiative-interoperable-and-secure)
- [EU AI Act Service Desk - AI agents](https://ai-act-service-desk.ec.europa.eu/en/ai-act/faq/how-are-ai-agents-addressed-within-ai-act-0)
