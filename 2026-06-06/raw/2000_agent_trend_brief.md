# 2000 Agent Trend Brief

- 작성 시점: 2026-06-06 20:00 KST
- 실행 유형: 수동 업데이트
- 조사 범위: Source Watchlist 기반 에이전트 트렌드, 프로젝트/논문 알림, AI 플랫폼 및 서비스 영향
- 저장 경로: `2026-06-06/2000_agent_trend_brief.html`

## 1. Executive Summary

참고 링크: [Anthropic](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code), [MCP Blog](https://blog.modelcontextprotocol.io/), [AgentBound](https://programming-group.com/assets/pdf/papers/2026_AgentBound-Securing-Execution-Boundaries-of-AI-Agents.pdf), [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401)

1. 이번 업데이트의 가장 강한 신호는 Claude Code Dynamic Workflows다. Anthropic 공식 발표와 Reddit 반응이 동시에 강하고, 병렬 subagent, workflow-as-code, 장시간 코드베이스 작업, token burn 논쟁이 모두 붙어 있다. 조직 내부 개발/운영 에이전트 플랫폼을 설계할 때 workflow orchestration, cost guardrail, subagent trace, approval gate를 반드시 포함해야 한다.

2. MCP는 단순 tool 연결 표준에서 production-grade agent platform 표준으로 확장 중이다. 2026-07-28 MCP Specification RC는 stateless core, MCP Apps, Tasks extension, OAuth/OIDC 정렬, deprecation policy를 포함하는 큰 변화다. AI 플랫폼이 MCP registry를 만든다면 단순 서버 목록이 아니라 task lifecycle, UI rendering, authorization, version/deprecation까지 포함해야 한다.

3. Project / Paper Alert 쪽에서는 AgentBound, SWE-Skills-Bench, COLLEAGUE.SKILL을 우선 추적해야 한다. AgentBound는 MCP 서버 접근통제 논문이고, SWE-Skills-Bench는 skill adoption 과열에 제동을 거는 벤치마크이며, COLLEAGUE.SKILL은 expert trace를 skill로 증류하는 흐름이다. AI 플랫폼에서 skill marketplace를 만들 경우 "많이 설치"가 아니라 eval과 권한, 버전 호환성을 기준으로 관리해야 한다.

## 2. Source Coverage

| 소스 | 확인 여부 | 유의미한 변화 | 후속 액션 |
| --- | --- | --- | --- |
| GeekNews | Checked | AI 에이전트, CLI 회귀, Context Mode, MCP 논쟁, 로컬 AI/비용 이슈가 계속 이어짐 | MCP governance와 coding agent 비용 이슈 계속 추적 |
| MarkTechPost | Checked | Agent-Infra AIO Sandbox, UCP, OpenSpace, Cloudflare Agents SDK 등 agent runtime/commerce/skills 관련 자료 확인 | AIO Sandbox와 UCP는 딥다이브 후보 |
| GitHub Blog | Checked | Copilot App 기술 프리뷰 확대, gh skill, usage-based billing, Copilot CLI/agent UX 신호 확인 | GitHub Agent-Native UX 딥다이브 후보 유지 |
| Reddit r/Agent_AI | Checked | 프로젝트/운영 후기성 신호는 보조적으로 확인 | 반복 언급 프로젝트만 선별 |
| Reddit r/ClaudeCode | Checked | Dynamic Workflows가 가장 큰 이슈. token burn, subagent orchestration, workflow 의미 논쟁 | 최우선 딥다이브 후보 |
| Reddit r/claudeskills | Checked | Skills/CLAUDE.md/skill marketplace 흐름 유지 | SWE-Skills-Bench와 함께 검증 중심으로 추적 |
| Reddit r/vibecoding | Checked | Dynamic Workflows와 agent coding 실사용 반응 확인 | 비용/품질/검토 부담 관점 추적 |
| 보조: MCP Blog/Docs | Checked | 2026-07-28 MCP Specification RC 확인 | 별도 딥다이브 후보 |
| 보조: OpenAI Agents SDK Docs | Checked | sandbox agents, handoffs, guardrails, HITL, tracing, realtime/voice agents 구조 확인 | AI 플랫폼 SDK 비교 기준으로 사용 |
| 보조: UCP/AP2/AG-UI Docs | Checked | agentic commerce와 user-facing agent UI protocol stack 확인 | AI 플랫폼 쇼핑/커머스/결제/로컬 예약 관점 딥다이브 후보 |

## 3. Project / Paper Alert

참고 링크: [Dynamic Workflows](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code), [AgentBound](https://programming-group.com/assets/pdf/papers/2026_AgentBound-Securing-Execution-Boundaries-of-AI-Agents.pdf), [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401), [COLLEAGUE.SKILL](https://arxiv.org/abs/2605.31264)

| 구분 | 프로젝트/논문 | 왜 이슈인가 | 사람들이 하는 말 | AI 플랫폼/서비스 영향 | 딥다이브 추천 |
| --- | --- | --- | --- | --- | --- |
| Project | Claude Code Dynamic Workflows / ultracode | Anthropic 공식 발표와 Reddit 강한 반응. 수십~수백 병렬 subagent, 장시간 workflow, Bun port 사례 | 긍정: 코드베이스 규모 작업의 전환점. 우려: token burn, workflow bias, 비용 폭증, 제어권 문제 | 내부 개발/운영 에이전트, 보안 감사, 대규모 마이그레이션에 직접 연결 | Yes, 최우선 |
| Spec | MCP 2026-07-28 Specification RC | MCP 출시 이후 최대급 개정. stateless core, MCP Apps, Tasks, OAuth/OIDC alignment | production MCP의 stateful transport, UI, long-running task, auth 문제에 대한 해법으로 보임 | MCP registry/agent platform의 핵심 계약 변경 가능성 | Yes |
| Paper | AgentBound: Securing Execution Boundaries of AI Agents | MCP 서버 보안/접근통제를 정면으로 다룬 논문. 296개 인기 MCP 서버 데이터셋, 자동 policy 생성 | MCP 서버가 host system에 과권한 접근한다는 문제의식 | MCP 서버 검증, permission tier, sandbox policy, 보안 baseline에 중요 | Yes |
| Paper | SWE-Skills-Bench | Agent Skills 효과를 실증 검증. 49개 public SWE skills 중 39개가 pass-rate 개선 없음, 평균 +1.2% | skills hype에 대한 반례. token overhead와 version mismatch가 위험 | skill marketplace를 만들 때 eval/compatibility가 필수 | Yes |
| Paper/Project | COLLEAGUE.SKILL | expert traces를 portable, correctable skill로 증류하는 방향. 최근 arXiv 제출 | personal/role-grounded skills 가능성을 보여주지만 주장의 검증 필요 | AI 플랫폼 업무 지식/운영 절차/CS 대응 skill화에 연결 | Maybe |
| Product | GitHub Copilot App + gh skill | agent-native desktop, MCP, reusable skills, scheduled automation, voice, cloud session 결합 | 개발자가 코드를 쓰는 사람이 아니라 agent output manager가 되는 UX | 조직 내부 개발 플랫폼의 task board/approval/review UX 참고 | Yes |
| Protocol Stack | UCP / AP2 / AG-UI | agentic commerce와 user-facing agent UI가 프로토콜 스택화 | A2A/MCP만으로는 결제/승인/UI/감사까지 해결 불가 | 쇼핑/커머스/결제/로컬 예약에 전략적 | Yes |

## 4. 신규 사항

| 중요도 | 주제 | 새로 확인한 내용 | 왜 중요한가 | AI 플랫폼/서비스 영향 | 출처 |
| --- | --- | --- | --- | --- | --- |
| High | Claude Code Dynamic Workflows | 2026-05-28 Anthropic 발표. Claude가 orchestration script를 만들고 병렬 subagent를 실행하며 결과를 검증 | coding agent가 단일 agent에서 workflow runtime으로 이동 | 대규모 리팩토링/감사/마이그레이션 자동화 플랫폼 필요 | Anthropic, Reddit |
| High | MCP 2026-07-28 RC | MCP Blog가 May 21에 release candidate 공개. stateless core, MCP Apps, Tasks extension, OAuth/OIDC, deprecation policy 포함 | MCP가 enterprise production protocol로 성숙 중 | MCP registry와 tool governance 설계 업데이트 필요 | MCP Blog |
| High | AgentBound | MCP 서버 접근통제 프레임워크 논문. 296개 MCP 서버 분석, 80.9% 정책 자동 생성 정확도 | MCP 보안의 실무 공백을 정량적으로 제시 | AI 플랫폼 MCP 서버 인증/인가/sandbox policy baseline 후보 | AgentBound paper |
| High | SWE-Skills-Bench | skills 효과를 벤치마크로 검증. 평균 개선 +1.2%, 일부 skill은 성능 저하 | skill ecosystem의 hype를 eval로 걸러야 함 | skill marketplace 운영 기준 필요 | arXiv |
| Medium | COLLEAGUE.SKILL | expert knowledge를 skill로 증류하는 자동화 시스템 | memory와 skill의 경계가 업무 지식 패키징으로 이동 | AI 플랫폼 사내 업무/CS/운영 정책 skill화 가능성 | arXiv |
| Medium | GitHub Copilot App preview 확대 | 2026-06-02 기존 Copilot Pro/Business/Enterprise 고객으로 preview 확대. MCP, reusable skills, scheduled automation, voice, cloud sessions 포함 | agent-native control center UX가 구체화 | 조직 내부 agent workspace UX 참고 | GitHub Blog |
| Medium | UCP/AP2/AG-UI | UCP가 commerce lifecycle, AP2가 결제 승인/audit, AG-UI가 user-facing interaction protocol 담당 | agent service stack이 도구/에이전트/사용자/결제 계층으로 분화 | AI 플랫폼 커머스/결제/로컬 예약 전략에 중요 | UCP/AP2/AG-UI docs |

## 5. 기존 조사 업데이트

| 기존 주제 | 변경점 | 기존 판단 유지/수정 | AI 플랫폼/서비스 영향 | 후속 확인 |
| --- | --- | --- | --- | --- |
| A2A | AP2와 UCP 공식 문서에서 A2A가 commerce/payment stack의 일부로 연결됨 | 유지: A2A는 agent-to-agent 표준 후보 | A2A 단독이 아니라 MCP/UCP/AP2/AG-UI와 함께 설계해야 함 | A2A+UCP+AP2 통합 플로우 딥다이브 |
| MCP Production Mess | MCP RC와 AgentBound가 production pain을 공식/연구 레벨에서 뒷받침 | 강화: tool 연결보다 governance가 중요 | registry, auth, policy, risk annotation, Tasks extension 설계 필요 | MCP 2026-07-28 RC 딥다이브 |
| Skills Marketplace | GitHub gh skill과 SWE-Skills-Bench가 동시에 등장 | 수정: skill은 중요하지만 무조건 효과적이지 않음 | skill eval, version pinning, supply chain integrity, compatibility 테스트 필요 | SWE-Skills-Bench 딥다이브 |
| Agentic Commerce | UCP/AP2 문서 확인으로 중요도 상승 | 강화: 쇼핑 추천이 아니라 결제 승인/audit 문제 | AI 플랫폼결제/커머스/쇼핑/로컬 예약의 공통 agent commerce contract 필요 | UCP/AP2/AG-UI 딥다이브 |

## 6. Technical Detail

참고 링크: [Workflow runtime](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code), [MCP Docs](https://modelcontextprotocol.io/), [UCP](https://ucp.dev/), [AP2](https://ap2-protocol.org/), [AG-UI](https://docs.ag-ui.com/)

### 6.1 Dynamic workflows

Anthropic은 dynamic workflows를 research preview로 공개했다. 핵심은 Claude가 workflow orchestration script를 만들고, 이를 통해 병렬 subagents를 실행하며, 결과를 검증하고, 중단된 장시간 작업을 재개할 수 있다는 점이다. 공식 사례로 Bun의 Zig-to-Rust port가 언급되며, 약 750,000 lines, 11 days, 99.8% test suite passing이라는 수치가 제시됐다.

AI 플랫폼 관점의 핵심은 "많은 agent를 돌린다"가 아니다. 필요한 것은 workflow policy, budget limit, per-task model selection, subagent trace, approval gate, resumability, artifact review다. Reddit 반응에서도 token burn과 제어권 문제가 반복된다.

### 6.2 MCP 2026-07-28 RC

MCP Blog는 2026-07-28 release candidate를 가장 큰 revision이라고 설명한다. 키워드는 stateless core, ordinary HTTP infrastructure, MCP Apps, Tasks extension, OAuth/OIDC alignment, formal deprecation policy다. 이는 MCP가 local tool protocol에서 enterprise deployable protocol로 가고 있음을 뜻한다.

### 6.3 Skills need eval

GitHub는 `gh skill`로 skills discovery/install/publish를 CLI에 넣었다. 동시에 SWE-Skills-Bench는 대부분의 public SWE skills가 pass-rate 개선을 만들지 못하거나 token overhead만 늘릴 수 있음을 보여준다. 따라서 AI 플랫폼이 skill marketplace를 만든다면 install count가 아니라 task-specific eval, compatibility matrix, version pinning, supply-chain integrity를 핵심으로 둬야 한다.

### 6.4 Agentic commerce protocol stack

UCP는 discovery-to-checkout-to-order lifecycle을 다루고, AP2는 human-present/human-not-present payment mandates와 verifiable digital credentials를 다루며, AG-UI는 user-facing agent interaction을 event-based protocol로 다룬다. A2A/MCP만으로는 commerce UX, 결제 승인, UI, audit trail을 완결하기 어렵다.

## 7. AI Platform Implications

- AI 플랫폼: Agent Registry와 MCP Registry를 통합하되, Task lifecycle, tool risk, app/UI rendering, permission, deprecation을 같이 관리해야 한다.
- 대화형 AI 서비스 / 대화형 AI 서비스: AG-UI류의 streaming, interrupt, approval, shared state, user steering이 필요하다.
- 쇼핑/커머스/결제/로컬 예약: UCP/AP2 스타일의 checkout mandate, payment mandate, identity linking, order lifecycle을 검토해야 한다.
- 사내 개발 플랫폼: GitHub Copilot App과 Claude Dynamic Workflows를 참고해 agent task board, review queue, trace view, budget guardrail을 설계해야 한다.
- Skills: AI 플랫폼 정책/CS/운영 지식을 skill화할 수 있지만, SWE-Skills-Bench 관점의 eval 없이 배포하면 token 낭비와 잘못된 guidance가 생길 수 있다.

## 8. Recommended Actions

- 지금 할 일: `Claude Code Dynamic Workflows`와 `MCP 2026-07-28 RC` 중 하나를 바로 딥다이브한다.
- 30일 내 할 일: AI 플랫폼 Agent/MCP Registry 설계안에 risk annotation, permission tier, trace, version/deprecation, task lifecycle을 포함한다.
- 계속 추적할 일: AgentBound, SWE-Skills-Bench, COLLEAGUE.SKILL, UCP/AP2/AG-UI의 실제 구현체와 커뮤니티 채택 신호.

## 9. Risks / Watch Items

- 비용 리스크: multi-agent workflows는 token 사용량이 급증할 수 있다.
- 보안 리스크: MCP 서버와 sandbox agent는 host/resource 접근권한을 명시적으로 통제해야 한다.
- 품질 리스크: skills는 task에 맞지 않으면 효과가 없거나 오히려 성능을 떨어뜨릴 수 있다.
- 제품 리스크: commerce agent는 추천보다 결제 승인, 책임, 분쟁 처리, audit trail이 더 어렵다.

## 10. Sources

- Anthropic, Dynamic workflows: https://claude.com/blog/introducing-dynamic-workflows-in-claude-code
- Reddit r/ClaudeCode, Dynamic Workflows: https://www.reddit.com/r/ClaudeCode/comments/1tq9pge/introducing_dynamic_workflows_in_claude_code/
- Reddit r/ClaudeAI, Dynamic Workflows: https://www.reddit.com/r/ClaudeAI/comments/1tq9ofy/introducing_dynamic_workflows_in_claude_code/
- MCP Blog, 2026-07-28 RC: https://blog.modelcontextprotocol.io/
- MCP Docs: https://modelcontextprotocol.io/
- AgentBound PDF: https://programming-group.com/assets/pdf/papers/2026_AgentBound-Securing-Execution-Boundaries-of-AI-Agents.pdf
- SWE-Skills-Bench: https://arxiv.org/abs/2603.15401
- COLLEAGUE.SKILL: https://arxiv.org/abs/2605.31264
- GitHub Copilot App preview: https://github.blog/changelog/2026-06-02-expanded-technical-preview-availability-for-the-github-copilot-app/
- GitHub gh skill: https://github.blog/changelog/2026-04-16-manage-agent-skills-with-github-cli/
- GitHub usage-based billing: https://github.blog/news-insights/company-news/github-copilot-is-moving-to-usage-based-billing/
- MarkTechPost, Agent-Infra AIO Sandbox: https://www.marktechpost.com/2026/03/29/agent-infra-releases-aio-sandbox-an-all-in-one-runtime-for-ai-agents-with-browser-shell-shared-filesystem-and-mcp/
- MarkTechPost, UCP: https://www.marktechpost.com/2026/01/12/google-ai-releases-universal-commerce-protocol-ucp-an-open-source-standard-designed-to-power-the-next-generation-of-agentic-commerce/
- UCP Docs: https://ucp.dev/
- AP2 Docs: https://ap2-protocol.org/
- AG-UI Docs: https://docs.ag-ui.com/
- OpenAI Agents SDK: https://openai.github.io/openai-agents-python/
