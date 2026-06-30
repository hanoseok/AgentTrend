# Agent Trend Source Watchlist

- 최종 갱신: 2026-07-01 07:00 KST
- 정기 확인: 매일 07:00 KST
- 목적: 에이전트 트렌드 정기 조사에서 매번 순회할 소스 목록 관리
- HTML: `source/`

## 1. 필수 순회 소스

| 소스 | URL | 확인할 신호 | 딥다이브 후보 판단 기준 |
| --- | --- | --- | --- |
| GeekNews | https://news.hada.io/ | 한국 개발자 커뮤니티에서 주목받는 에이전트, MCP, Claude Code, 프로토콜, 개발 워크플로 이슈 | 댓글/포인트가 붙고, 국내 개발자 관점에서 논쟁이 생기거나 실무 적용성이 보이면 후보화 |
| MarkTechPost | https://www.marktechpost.com/ | Agentic AI 논문/프레임워크/프로토콜/런타임/상거래/온디바이스 에이전트 소개 | 공식 repo, 논문, 벤치마크, 구현체가 있고 AI 플랫폼 아키텍처에 연결되면 후보화 |
| GitHub Blog | https://github.blog/ | Copilot coding agent, Copilot CLI, SDK, custom agents, skills, MCP, code review, sandbox, enterprise controls | 개발자 agent UX, agent control center, review/security/sandbox 기능 변화가 있으면 후보화 |
| Reddit r/Agent_AI | https://www.reddit.com/r/Agent_AI/ | 에이전트 커뮤니티에서 많이 공유되는 프로젝트, MCP 서버, 프롬프트/스킬/런타임 논쟁 | 반복 언급, 구체적 프로젝트, 사용 후기, 운영 실패담이 있으면 후보화 |
| Reddit r/ClaudeCode | https://www.reddit.com/r/ClaudeCode/ | Claude Code 기능 변화, dynamic workflows, subagents, MCP, workflows, CLI/desktop 사용 패턴 | 공식 발표와 커뮤니티 반응이 동시에 강하거나 실제 workflow 변화가 있으면 후보화 |
| Reddit r/claudeskills | https://www.reddit.com/r/claudeskills/ | Claude Skills, CLAUDE.md, project memory, 업무 지식 패키징, skill marketplace 흐름 | 반복 가능한 업무 지식/검증 절차를 skill화한 사례가 있으면 후보화 |
| Reddit r/vibecoding | https://www.reddit.com/r/vibecoding/ | AI coding 실전 사용, 실패담, 생산성 도구, agent runtime, 보안/품질 문제 | 개인/소규모 팀의 실제 사용 문제가 플랫폼 공통 병목으로 보이면 후보화 |

## 2. 관련 보조 소스

| 소스 | URL | 활용 방식 |
| --- | --- | --- |
| Reddit r/AI_Agents | https://www.reddit.com/r/AI_Agents/ | 에이전트 프로젝트, MCP, observability, runtime 실패담을 보조 확인 |
| Reddit r/ClaudeAI | https://www.reddit.com/r/ClaudeAI/ | Claude Code 외 Claude 생태계의 agent memory, MCP gateway, multi-agent 운영 사례 확인 |
| Reddit r/mcp | https://www.reddit.com/r/mcp/ | MCP 서버, tool governance, agent-ready web, security 이슈를 보조 확인 |
| Reddit r/hermesagent | https://www.reddit.com/r/hermesagent/ | persistent agent, memory, skills, gateway, cron/webhook, tool bloat, 운영 실패담을 보조 확인 |
| Google Agent Executor / AX | https://agentexecutor.io/, https://github.com/google/ax, https://github.com/google/ax/releases/tag/v0.1.0 | 추가 이유: distributed agent runtime, durable event log, resumable execution, isolation, MCP/A2A support가 AI 플랫폼 runtime 설계와 직접 연결됨. 관찰할 신호: API 안정화, event log 변경, transport/HarnessService, security boundary, Kubernetes target, policy/audit, release cadence |
| Agent Substrate / GKE Agent Sandbox | https://github.com/agent-substrate/substrate, https://pkg.go.dev/github.com/agent-substrate/substrate, https://docs.cloud.google.com/kubernetes-engine/docs/how-to/agent-sandbox, https://kubernetes.io/blog/2026/03/20/running-agents-on-kubernetes-with-agent-sandbox/ | 추가 이유: AX의 production deployment story가 stateful actor lifecycle, suspend/resume, warm pool, pod snapshot, gVisor/Kata isolation과 직접 연결됨. 관찰할 신호: CRD/API 안정화, WorkerPool/ActorTemplate 변화, sandbox isolation defaults, snapshot/restore latency, cost model, security boundary |
| Kimi Code CLI | https://github.com/MoonshotAI/kimi-code, https://moonshotai.github.io/kimi-code/ | 추가 이유: terminal coding agent의 MCP config, plugin marketplace, subagents, hooks, approval flow가 open-source harness 기준으로 유용함. 관찰할 신호: provider compatibility, plugin trust model, MCP auth, subagent isolation |
| Cline SDK / Cline runtime | https://docs.cline.bot/sdk/overview, https://github.com/cline/cline | 추가 이유: IDE extension에서 product-embedded SDK, scheduled agents, Kanban/worktree, MCP, subagents로 확장 중. 관찰할 신호: SDK stability, schedule audit, checkpoint, MCP permission, enterprise controls |
| AI Agent Usage / Adoption Benchmarks | https://www.langchain.com/state-of-agent-engineering, https://www.langchain.com/stateofaiagents, https://openai.com/index/how-people-are-using-chatgpt/, https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai, https://www.deloitte.com/us/en/what-we-do/capabilities/applied-artificial-intelligence/content/state-of-ai-in-the-enterprise.html, https://www.pwc.com/us/en/tech-effect/ai-analytics/ai-agent-survey.html, https://survey.stackoverflow.co/2025/ai | 추가 이유: agent runtime/protocol 조사와 별도로 실제 사용 분야, production deployment, 기업 기능별 adoption, 개발자 agent 사용률을 추적하기 위한 benchmark 묶음. 관찰할 신호: customer service vs research/data analysis 비중, internal productivity 변화, IT/security agent 확산, agent 정의 변화, survey 표본 편향 |
| Agent Work / Discovery / Governance / Evals | https://openai.com/index/how-agents-are-transforming-work/, https://developers.openai.com/blog/connect-private-mcp-servers-to-openai-products, https://openai.com/index/personal-finance-chatgpt/, https://www.anthropic.com/research/economic-index-june-2026-report, https://www.anthropic.com/research/claude-code-expertise, https://developers.googleblog.com/announcing-the-agentic-resource-discovery-specification/, https://github.blog/changelog/2026-06-17-agent-finder-for-github-copilot-now-available/, https://github.blog/changelog/label/copilot/, https://learn.microsoft.com/en-us/visualstudio/releases/2026/release-notes, https://blogs.microsoft.com/blog/2026/06/02/microsoft-build-2026-be-yourself-at-work/, https://www.nccoe.nist.gov/sites/default/files/2026-02/accelerating-the-adoption-of-software-and-ai-agent-identity-and-authorization-concept-paper.pdf, https://www.prnewswire.com/news-releases/patronus-ai-raises-50-million-series-b-and-unveils-first-digital-world-models-for-ai-agent-training-and-simulation-302811248.html, https://developers.googleblog.com/measuring-what-matters-with-jules/, https://www.langchain.com/blog/langchain-langgraph-1dot0 | 추가 이유: agent가 delegated work, IDE/terminal operations, discovery/registry, MCP trust, identity/authorization, simulation/eval, graph runtime 안정화로 이동하는 흐름을 추적하기 위한 공식/준공식 소스 묶음. 관찰할 신호: ARD 채택, agent inventory/governance 제품화, agent identity standard, task horizon metric, MCP trust validation, model routing/lifecycle, sensitive-domain consent/evidence/revocation, digital world eval, framework production stability |
| Scientific Workbench / Eval / Skill Memory | https://www.anthropic.com/news/claude-science-ai-workbench, https://www.anthropic.com/news/claude-sonnet-5, https://openai.com/index/introducing-genebench-pro/, https://developers.googleblog.com/announcing-adk-go-20/, https://developers.googleblog.com/driving-the-agent-quality-flywheel-from-your-coding-agent/, https://www.langchain.com/blog/running-untrusted-agent-code-without-a-sandbox, https://www.langchain.com/blog/unified-stack-for-evaluating-agents, https://www.langchain.com/blog/wiki-memory, https://www.langchain.com/blog/introducing-dynamic-subagents-in-deep-agents, https://www.microsoft.com/en-us/research/blog/skillopt-agent-skills-as-trainable-parameters/ | 추가 이유: agent가 domain-specific workbench, graph runtime, AutoRater/eval flywheel, untrusted-code boundary, skill/memory optimization으로 구체화되는 흐름을 추적하기 위한 공식/연구 소스 묶음. 관찰할 신호: scientific agent adoption, graph/HITL runtime stability, eval runner integration, failure clustering, skill transfer, wiki memory review workflow |
| arXiv / Hugging Face Papers | https://arxiv.org/, https://huggingface.co/papers | agent security, skill eval, trace-to-skill, memory, workflow, multi-agent 논문과 커뮤니티 반응을 확인 |
| Google Developers Blog | https://developers.googleblog.com/ | A2A, UCP, AP2, A2UI, AG-UI, ADK 등 공식 프로토콜/SDK 업데이트 교차 확인 |
| Anthropic / Claude Blog | https://claude.com/blog | Claude Code, Skills, Managed Agents, MCP, memory, workflow 기능의 공식 발표 확인 |
| OpenAI News / Docs | https://openai.com/news/ | OpenAI Agents, Codex, Responses/Agents SDK, MCP, agent commerce 관련 공식 업데이트 확인 |
| OpenAI Codex Sites | https://developers.openai.com/codex/sites | 추가 이유: agent가 build/save/deploy/access-control까지 처리하는 product shipping loop의 공식 문서. 관찰할 신호: review-before-publish, deployment history, environment/secrets handling, hosted app permissions |
| Model Context Protocol Docs / Blog | https://modelcontextprotocol.io/, https://blog.modelcontextprotocol.io/ | MCP specification, registry, apps, tasks, risk annotations, auth/OAuth, governance 변경을 공식 출처로 확인 |
| MCP Authorization / Permission Analysis | https://openfga.dev/docs/modeling/agents/mcp-authorization, https://mcpblog.dev/blog/2026-03-21-chmod-ai-agents-mcp-permissions | MCP server 권한 모델, tool-level authorization, policy enforcement, audit trail 설계 사례를 확인 |
| A2A Protocol Docs | https://a2a-protocol.org/latest/ | A2A specification, Agent Card, task lifecycle, SDK, release notes, enterprise feature 변경 확인 |
| OpenAI Agents SDK Docs | https://openai.github.io/openai-agents-python/ | agents, handoffs, sandbox agents, guardrails, MCP, sessions, human-in-the-loop, tracing 구조 변화 확인 |
| Claude Code Docs | https://code.claude.com/docs | Claude Code workflows, skills, MCP, hooks, memory, sandbox/managed agents 등 실사용 기능 변화 확인 |
| AG-UI Docs | https://docs.ag-ui.com/ | Agent-user interaction, streaming events, generative UI, frontend/backend agent protocol 변화 확인 |
| AP2 / Google Agentic Commerce | https://ap2-protocol.org/, https://github.com/google-agentic-commerce | agent payments, checkout/payment mandate, commerce authorization, samples, UCP/AP2 연결 변화 확인 |
| GitHub Copilot Agent Platform Updates | https://github.blog/changelog/2026-06-04-agent-tasks-rest-api-now-available-for-copilot-pro-pro-and-max/, https://github.blog/changelog/2026-06-02-expanded-technical-preview-availability-for-the-github-copilot-app/, https://github.blog/changelog/2026-06-05-enterprise-managed-plugins-in-vs-code-in-public-preview/ | 추가 이유: background agent task API, canvas UX, cloud automations, enterprise plugin governance가 함께 움직임. 관찰할 신호: API lifecycle, enterprise policy, worktree/session isolation, artifact review surface |
| Google Colab CLI | https://developers.googleblog.com/introducing-the-google-colab-cli/ | 추가 이유: terminal/agent가 remote GPU/TPU runtime을 tool처럼 호출하는 cloud execution 패턴. 관찰할 신호: quota, data boundary, artifact recovery, reproducible environment, agent orchestration hooks |

## 3. 자동 소스 추가 기준

- 링크를 돌다가 지속 확인 가치가 높은 사이트를 발견하면 사용자 승인 없이 watchlist에 추가한다.
- 추가할 때는 추가 이유와 관찰할 신호를 함께 남긴다.
- 추가 기준은 공식 문서/스펙/릴리스, 고품질 기술 분석, 커뮤니티 실사용 후기, 오픈소스 프로젝트, AI 플랫폼/서비스 적용 가능성이다.
- 노이즈가 크거나 업데이트가 끊기거나 홍보성 자료만 반복되면 보조 소스로 낮추거나 제거 후보로 표시한다.

## 4. 매회 체크 방식

- 브리프마다 어떤 소스를 확인했는지, 유의미한 업데이트가 있었는지, 업데이트가 없으면 "특이사항 없음"으로 남긴다.
- 신호는 공식 발표, 커뮤니티 열기, 오픈소스 프로젝트, 실패담/운영 이슈, 보안 이슈, AI 플랫폼/서비스 적용 가능성으로 분류한다.
- 주제가 깊게 볼 만하면 발견 소스, 사람들이 하는 말, 트렌디한 이유, AI 플랫폼/서비스에 중요한 이유를 정리하고 사용자에게 먼저 묻는다.

## 5. 딥다이브 후보 점수 기준

- 공식 발표 또는 스펙/릴리스/논문이 있는가
- 커뮤니티에서 반복적으로 언급되거나 실제 사용 후기가 있는가
- 프로젝트, SDK, 오픈소스 repo, 벤치마크가 있는가
- 대화형 AI 서비스, 쇼핑/커머스, 로컬/지도, 결제, 사내 개발 플랫폼과 연결되는가
- 보안, 권한, 감사, observability, 비용, UX 같은 플랫폼 의사결정에 영향을 주는가

## 6. Project / Paper Alert 기준

- 프로젝트 신호: GitHub repo, SDK, MCP server, agent framework, benchmark tool, sandbox/runtime, skill marketplace가 갑자기 많이 언급되거나 실제 사용 후기가 붙으면 우선 보고한다.
- 논문 신호: arXiv, 연구소 블로그, 벤치마크 리더보드에서 agent architecture, memory, eval, tool use, multi-agent, security 관련 논문이 이슈가 되면 우선 보고한다.
- 보고 방식: 프로젝트/논문명, 링크, 왜 이슈인지, 사람들이 하는 말, 기술 핵심, 검증 필요점, AI 플랫폼/서비스 적용 가능성, 딥다이브 추천 여부를 함께 쓴다.
