# 0701 Agent Trend Brief

- 작성 시점: 2026-07-05 07:01 KST
- 범위: agent accountability layer, usage telemetry, org-billed automation, model lifecycle, hosted secure runtimes, regulated-market guardrails, framework fragmentation
- HTML: `report/agent-trend-brief/2026-07-05-0701/`

## 1. Executive Summary

참고 링크: [GitHub Copilot usage metrics coverage](https://github.blog/changelog/2026-07-02-improved-accuracy-and-coverage-in-copilot-usage-metrics-reports/), [GitHub Copilot CLI in Actions without PAT](https://github.blog/changelog/2026-07-02-copilot-cli-no-longer-needs-a-personal-access-token-in-github-actions/), [GitHub Gemini deprecation](https://github.blog/changelog/2026-07-02-upcoming-deprecation-of-gemini-2-5-pro-and-gemini-3-flash/), [Google Cloud monthly AI update](https://cloud.google.com/blog/products/ai-machine-learning/what-google-cloud-announced-in-ai-this-month), [Bank of England Sarah Breeden speech](https://www.bankofengland.co.uk/speech/2026/june/sarah-breeden-panel-at-the-european-central-bank-forum-on-central-banking-2026), [OpenAI AgentKit update](https://openai.com/index/introducing-agentkit/), [Anthropic Claude Sonnet 5](https://www.anthropic.com/news/claude-sonnet-5)

오늘 핵심은 AI agent 시장이 "기능과 모델 성능"에서 "운영 책임성"으로 더 강하게 이동한다는 점이다. GitHub는 Copilot usage metrics의 coverage와 credit attribution을 정정했고, Copilot CLI가 GitHub Actions에서 장기 PAT 없이 `GITHUB_TOKEN`으로 실행되도록 했다. 이는 agent 실행 로그, 비용 귀속, org billing, workflow token scope가 한 묶음으로 관리되어야 함을 보여준다. 동시에 GitHub의 Gemini 모델 deprecation 공지는 agent route가 model lifecycle calendar와 fallback policy를 가져야 한다는 신호다. Google Cloud는 Managed Agents API와 CodeMender를 Agent Platform 맥락으로 다시 전면화했고, Bank of England는 agentic AI가 금융시장, 결제, 사이버 리스크를 바꿀 수 있어 kill switch와 circuit breaker식 guardrail 논의가 필요하다고 봤다.

핵심 판단:

1. Agent 운영 단위는 session trace에서 accountability plane으로 확장된다. usage coverage, credit attribution, org-billed automation, token scope, model lifecycle을 한 ledger에 묶어야 한다.
2. CI/CD 안에서 agent CLI가 기본 토큰으로 실행되는 패턴은 자동화 접근성을 높이지만, user budget이 아니라 org budget과 workflow permission을 기준으로 통제해야 하는 영역을 만든다.
3. Model deprecation은 성능 문제가 아니라 운영 연속성 문제다. agent policy에는 deprecation date, fallback model, admin enablement, regression eval이 필수 필드가 된다.
4. 금융권과 보안 영역에서는 autonomous agent의 목표 함수, 시장 동조, 결제/거래 실행, cyber response가 public-policy guardrail과 연결된다.
5. OpenAI Agent Builder/Evals wind-down과 framework roundup은 agent builder 경험이 code-first SDK, hosted natural-language agent, open-source graph/runtime framework로 분화되고 있음을 보여준다.

이번 회차 결론: AI 플랫폼은 `agent_accountability_plane`, `agent_ci_policy`, `model_lifecycle_calendar`, `regulated_agent_guardrail_profile`, `framework_portability_index`, `authorized_action_policy`를 정기 운영 객체로 다뤄야 한다.

## 2. Source Coverage

| 소스 | 확인 여부 | 유의미한 변화 | 신뢰도 |
| --- | --- | --- | --- |
| GitHub Changelog | Checked | Copilot usage metrics coverage/credit attribution, Copilot CLI Actions token model, Gemini model deprecation이 같은 운영 책임성 이슈로 연결됨. | Official / Confirmed |
| Google Cloud | Checked | Managed Agents API와 CodeMender가 Agent Platform의 secure hosted runtime과 security-agent use case를 강화. | Official / Confirmed |
| Bank of England | Checked | Sarah Breeden 부총재가 agentic AI의 finance/cyber/markets/payments 영향과 kill switch, circuit breaker형 guardrail 필요성을 언급. | Official policy speech / Confirmed |
| OpenAI | Checked | Agent Builder와 Evals 제품 wind-down 공지로 code-first Agents SDK와 Workspace Agents로 사용 경로가 재정렬됨. | Official / Confirmed |
| Anthropic | Checked | Claude Sonnet 5를 agentic model로 포지셔닝하고 browser/terminal/autonomous work를 강조. | Official / Confirmed |
| Media / Community | Checked | Arcade authorization funding, Gradial marketing agents, KDnuggets framework roundup, UN AI for Good Commission은 시장/커뮤니티 보조 신호로만 사용. | Media/community / Contextual |

## 3. Project / Product / Policy Alerts

### GitHub: usage metrics coverage and credit attribution

참고 링크: [Improved accuracy and coverage in Copilot usage metrics](https://github.blog/changelog/2026-07-02-improved-accuracy-and-coverage-in-copilot-usage-metrics-reports/)

- 우선순위: High
- 확인된 사실: GitHub는 Copilot usage metrics reports에서 CLI suggested lines, IDE/plugin versions, AI credit consumption attribution coverage를 개선했다고 공지했다. GitHub는 이 변경으로 일부 credit totals가 증가할 수 있다고 설명했다.
- 해석: agent analytics는 고정된 진실이 아니라 telemetry coverage의 함수다. 운영 대시보드에는 "측정 범위가 바뀌어서 숫자가 변했다"는 metadata가 필요하다.
- AI 플랫폼 영향: `agent_session_ledger`에 `telemetry_source`, `coverage_version`, `credit_attribution_status`, `measurement_change_reason`을 추가한다.
- 후속 확인: Copilot REST API field schema, CLI version별 line attribution, org billing export와 session event join key.

### GitHub: Copilot CLI in GitHub Actions without PAT

참고 링크: [Copilot CLI no longer needs a personal access token in GitHub Actions](https://github.blog/changelog/2026-07-02-copilot-cli-no-longer-needs-a-personal-access-token-in-github-actions/)

- 우선순위: High
- 확인된 사실: GitHub는 Actions workflow에서 장기 personal access token 없이 built-in `GITHUB_TOKEN`으로 Copilot CLI를 실행할 수 있게 했다. 조직 소유 repo에서는 AI credits가 조직에 직접 청구되며, workflow permission에 `copilot-requests: write`가 필요하다.
- 해석: agent automation이 CI/CD 표준 권한 모델 안으로 들어오고 있다. 그러나 사용자 단위 예산이 아니라 조직/비용센터 단위 통제가 핵심이 되며, workflow 권한 설정이 agent 보안 경계가 된다.
- AI 플랫폼 영향: `agent_ci_policy`에 repo, workflow, token_type, permission_scope, org_billed, session_limit, cost_center, approval_required를 기록한다.
- 후속 확인: Actions billing export, session limit enforcement, branch protection과 Copilot CLI 권한의 상호작용.

### GitHub: Gemini model deprecation

참고 링크: [Upcoming deprecation of Gemini 2.5 Pro and Gemini 3 Flash](https://github.blog/changelog/2026-07-02-upcoming-deprecation-of-gemini-2-5-pro-and-gemini-3-flash/)

- 우선순위: High
- 확인된 사실: GitHub는 Gemini 2.5 Pro와 Gemini 3 Flash를 2026-07-31부터 모든 Copilot experiences에서 deprecated 처리한다고 공지했다. 대체 모델로 Gemini 3.1 Pro와 Gemini 3.5 Flash를 제시했고, 관리자 model policy 조정이 필요할 수 있다고 안내했다.
- 해석: multi-model agent route는 vendor catalog 변화에 취약하다. 모델 deprecation은 product UX, eval baseline, compliance allowlist, cost forecast를 동시에 흔든다.
- AI 플랫폼 영향: `model_lifecycle_calendar`에 deprecation_date, fallback_model, admin_policy_status, eval_required, user_notice, route_owner를 넣는다.
- 후속 확인: GitHub model policy API, fallback 모델의 tool-use regression, enterprise admin enablement 절차.

### Google Cloud: Managed Agents API and CodeMender

참고 링크: [Google Cloud AI monthly update](https://cloud.google.com/blog/products/ai-machine-learning/what-google-cloud-announced-in-ai-this-month), [Gemini Enterprise Agent Platform](https://cloud.google.com/blog/products/ai-machine-learning/introducing-gemini-enterprise-agent-platform)

- 우선순위: Medium-High
- 확인된 사실: Google Cloud는 Agent Platform의 Managed Agents API를 통해 개발자가 secure Google-hosted environment에서 custom agents를 build/run할 수 있다고 설명했고, CodeMender를 vulnerability 탐지/수정 agent로 소개했다.
- 해석: hosted agent runtime은 편의 기능이 아니라 isolation, identity, eval, observability, security remediation을 묶는 managed control plane으로 포지셔닝된다.
- AI 플랫폼 영향: `hosted_agent_runtime_profile`을 만들어 runtime isolation, identity, registry, gateway, memory, eval, security action, audit export를 묶는다.
- 후속 확인: Managed Agents API pricing, identity boundary, CodeMender action authority, enterprise audit export.

### Bank of England: agentic finance risk and kill-switch style guardrails

참고 링크: [Bank of England - Agents of change speech](https://www.bankofengland.co.uk/speech/2026/june/sarah-breeden-panel-at-the-european-central-bank-forum-on-central-banking-2026)

- 우선순위: High
- 확인된 사실: Sarah Breeden 부총재는 AI가 finance를 재편하고 agentic AI가 cyber risk, markets, payments에 영향을 줄 수 있다고 말했다. 또한 AI agents가 쓰이는 시장의 resilience, objective functions의 public policy objective 포함 가능성, circuit breakers 또는 kill switches에 준하는 guardrails가 필요한지 검토해야 한다고 언급했다.
- 해석: agent governance는 enterprise productivity만의 문제가 아니다. trading, payments, cyber defense처럼 외부성이 큰 영역에서는 kill switch, rate limit, human escalation, market-wide coordination이 제품 요구사항이 된다.
- AI 플랫폼 영향: `regulated_agent_guardrail_profile`에 circuit_breaker_threshold, kill_switch_owner, trading/payment authorization tier, cyber escalation, public_policy_constraint, audit_replay를 둔다.
- 후속 확인: BoE/FCA/PRA 후속 문서, EU/US 금융 감독기관의 autonomous trading/payment agent guidance.

### OpenAI AgentKit migration and Anthropic Sonnet 5

참고 링크: [OpenAI AgentKit update](https://openai.com/index/introducing-agentkit/), [Anthropic Claude Sonnet 5](https://www.anthropic.com/news/claude-sonnet-5)

- 우선순위: Medium-High
- 확인된 사실: OpenAI는 Agent Builder와 Evals 제품을 2026-11-30 이후 사용할 수 없게 되며 code workflows는 Agents SDK, natural-language prompting은 Workspace Agents를 권장한다고 업데이트했다. Anthropic은 Claude Sonnet 5를 browser와 terminal을 쓰고 autonomous work를 수행하는 agentic model로 소개했다.
- 해석: agent builder UX는 no-code visual builder 중심에서 code-first SDK, workspace-hosted natural language agent, model-native tool/browser/terminal execution으로 분화된다.
- AI 플랫폼 영향: `framework_portability_index`에 SDK/runtime, hosted builder, eval migration, workflow export, session persistence, tool permission model을 비교한다.
- 후속 확인: OpenAI Evals migration path, Workspace Agents governance, Sonnet 5 tool-use eval과 pricing 변화.

## 4. 주요 동향과 중요도

| 중요도 | 동향 | 확인된 사실 | 해석 / 추론 | AI 플랫폼 적용 포인트 |
| --- | --- | --- | --- | --- |
| High | Accountability plane for agents | GitHub metrics coverage, credit attribution, Actions token, model deprecation. | agent 운영의 최소 단위가 trace/cost/model/token lifecycle 전체로 확장된다. | `agent_accountability_plane`, `agent_session_ledger`. |
| High | Org-billed CI agents | Copilot CLI가 Actions에서 `GITHUB_TOKEN`으로 실행되고 org credits에 청구될 수 있음. | user budget보다 workflow permission, cost center, session cap이 중요해진다. | `agent_ci_policy`, `cost_center_guardrail`. |
| High | Model lifecycle pressure | GitHub Copilot에서 Gemini 모델 deprecation 일정 공지. | agent route는 deprecation calendar와 fallback eval 없이는 안정적이지 않다. | `model_lifecycle_calendar`, `fallback_eval_suite`. |
| High | Regulated agent guardrails | BoE speech가 finance/cyber/markets/payments agent risk를 공식 agenda로 제시. | autonomous execution은 kill switch, circuit breaker, public-policy constraint가 필요하다. | `regulated_agent_guardrail_profile`. |
| Medium-High | Hosted secure agent runtime | Google Managed Agents API와 CodeMender. | hosted runtime과 security remediation agent가 platform feature가 된다. | `hosted_agent_runtime_profile`. |
| Medium | Framework fragmentation | OpenAI migration guidance와 framework roundup. | platform은 특정 framework 종속보다 portability와 eval continuity를 관리해야 한다. | `framework_portability_index`. |
| Medium | Authorization as investment thesis | Arcade funding, Gradial workflow claims. | enterprise agent 도입의 병목은 tool action authorization과 workflow compliance다. | `authorized_action_policy`, `workflow_compliance_rules`. |

## 5. AI Platform / Service Implications

1. `agent_accountability_plane`을 만든다.
   - session event, usage metric, credit attribution, billing scope, token type, model route, deprecation status, workflow permission을 하나의 운영 객체로 묶는다.

2. CI/CD agent policy를 별도 관리한다.
   - repo/workflow 단위로 `GITHUB_TOKEN` scope, `copilot-requests: write`, org-billed credit, branch protection, approval gate, session limit을 추적한다.

3. 모델 라우팅에 lifecycle calendar를 붙인다.
   - deprecation date, admin enablement, fallback model, regression eval, user notice, cost delta, compliance allowlist를 기본 필드로 둔다.

4. 금융/결제/보안 agent에는 regulated guardrail profile을 적용한다.
   - kill switch owner, circuit breaker threshold, human escalation SLA, transaction authorization tier, audit replay, public-policy constraint를 명시한다.

5. Agent framework 선택을 portability 기준으로 재정렬한다.
   - Agents SDK, Workspace Agents, Google ADK/Agent Platform, LangGraph, CrewAI, Mastra 등은 "개발 생산성"만이 아니라 eval portability, session trace, tool permission, deployment boundary로 비교한다.

6. Authorization layer를 제품 핵심 기능으로 둔다.
   - agent가 어떤 app/database/tool action을 수행할 수 있는지, 어떤 evidence와 approval이 필요한지, 어떤 budget과 scope에서 실행되는지를 policy로 관리한다.

## 6. Recommended Actions

- 지금: `agent_session_ledger`에 `telemetry_source`, `coverage_version`, `org_billed`, `workflow_token_type`, `credit_attribution_status`, `model_deprecation_date`, `fallback_model`을 추가한다.
- 지금: `agent_ci_policy` 초안을 만든다. GitHub Actions에서 agent CLI를 실행하는 workflow의 permission, billing, session cap, approval gate를 inventory화한다.
- 지금: `model_lifecycle_calendar`를 운영한다. 2026-07-31 Gemini deprecation을 첫 test case로 삼아 fallback eval과 admin policy 변경 절차를 점검한다.
- 30일: `regulated_agent_guardrail_profile`을 finance, payments, cyber response use case에 적용 가능한 최소 schema로 만든다.
- 30일: `framework_portability_index`를 만들어 code-first SDK, hosted agent, graph framework, open-source runtime의 migration/eval/trace portability를 비교한다.
- 계속: BoE/financial regulator follow-up, GitHub billing API, Google Managed Agents API details, OpenAI migration guidance, agent authorization vendors를 추적한다.

## 7. Risks / Watch Items

| 리스크 | 설명 | 대응 |
| --- | --- | --- |
| Telemetry drift | metric coverage가 개선되면 과거 대비 사용량/비용 추세가 갑자기 변할 수 있다. | coverage_version과 measurement_change_reason 기록. |
| Budget bypass by org billing | CI agent가 사용자 예산을 거치지 않고 조직 credit에 직접 청구될 수 있다. | cost center cap, session limit, workflow allowlist. |
| Model deprecation outage | deprecated model에 묶인 agent route가 실패하거나 성능이 달라질 수 있다. | lifecycle calendar, fallback eval, admin policy review. |
| Overpowered workflow token | `GITHUB_TOKEN` 권한이 agent action과 결합되면 blast radius가 커진다. | least-privilege permission, branch protection, approval gate. |
| Financial market feedback loops | trading/payment/cyber agents가 서로 반응해 시장 동조나 cascade를 만들 수 있다. | circuit breaker, kill switch, rate limit, human escalation. |
| Framework lock-in | visual builder, SDK, hosted runtime, graph framework 간 trace/eval/session model이 달라진다. | portability index, export contract, framework-neutral eval. |
| Media-claim overread | funding/adoption 기사에는 vendor claim과 selective case study가 섞일 수 있다. | official source 우선, 고객 수치 재확인, 추론 표시. |

## 8. Sources

### 왜 이걸 정리하게 되었는가

- [GitHub - Improved accuracy and coverage in Copilot usage metrics reports](https://github.blog/changelog/2026-07-02-improved-accuracy-and-coverage-in-copilot-usage-metrics-reports/)
- [GitHub - Copilot CLI no longer needs a PAT in GitHub Actions](https://github.blog/changelog/2026-07-02-copilot-cli-no-longer-needs-a-personal-access-token-in-github-actions/)
- [GitHub - Upcoming deprecation of Gemini 2.5 Pro and Gemini 3 Flash](https://github.blog/changelog/2026-07-02-upcoming-deprecation-of-gemini-2-5-pro-and-gemini-3-flash/)
- [Google Cloud - What Google Cloud announced in AI this month](https://cloud.google.com/blog/products/ai-machine-learning/what-google-cloud-announced-in-ai-this-month)
- [Bank of England - Agents of change](https://www.bankofengland.co.uk/speech/2026/june/sarah-breeden-panel-at-the-european-central-bank-forum-on-central-banking-2026)

### 딥리서치 출처

- [OpenAI - Introducing AgentKit](https://openai.com/index/introducing-agentkit/)
- [Anthropic - Claude Sonnet 5](https://www.anthropic.com/news/claude-sonnet-5)
- [KDnuggets - 10 agentic AI frameworks you should know in 2026](https://www.kdnuggets.com/10-agentic-ai-frameworks-you-should-know-in-2026)
- [ITU AI for Good Summit 2026 programme](https://aiforgood.itu.int/summit26/programme/)
- [Axios - UN-backed AI commission context](https://www.axios.com/2026/07/01/un-ai-commission-ceos-world-leaders)
- [WSJ - Arcade.dev funding context](https://www.wsj.com/cio-journal/arcade-dev-raises-60-million-to-secure-ai-agents-5d07eff4)
- [Axios - Gradial agentic marketing funding context](https://www.axios.com/2026/06/18/gradial-ai-agents-marketing)
