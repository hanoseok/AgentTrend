# Agent Trend 전체 인덱스

- 최종 갱신: 2026-06-07 09:00 KST
- HTML 정본: `index.html`
- raw 원본: `2026-06-06/raw/index.md`
- 정기 업데이트: 00:00 / 08:00 / 16:00 KST

## 1. Quick Read

- 지금 가장 중요한 축: Agent runtime/control plane. Google AX, Copilot Agent Tasks API, Cline SDK, Kimi Code CLI가 agent를 durable task, event log, policy, scheduling, worktree isolation으로 운영하는 방향을 보여준다.
- AI 플랫폼 사업 연결 축: UCP + AP2 + AG-UI + A2-UI. 대화형 AI 서비스/커머스/쇼핑/예약/결제를 agentic commerce로 묶으려면 commerce, payment, UI event, declarative UI가 함께 필요하다.
- 플랫폼 품질 축: SkillNet + SWE-Skills-Bench + COLLEAGUE.SKILL. 스킬은 많이 모으는 것이 아니라 task 개선, token cost, version compatibility, governance까지 검증해야 한다.

브리핑 시작점:

1. `2026-06-07/0900_agent_trend_brief.html`
2. `2026-06-06/AI_Platform_Service_Applicability.html`
3. `2026-06-07/Hermes_Agent_Deep_Dive.html`
4. `2026-06-07/AgentBound_Deep_Dive.html`
5. `2026-06-07/SkillNet_Deep_Dive.html`

## 2. Protocol / Platform Map

| 레이어 | 관련 자료 | 무엇을 해결하나 | AI 플랫폼 관점 |
| --- | --- | --- | --- |
| Agent-to-Agent | `2026-06-06/A2A_Deep_Dive.html` | agent discovery, task delegation, Agent Card, task lifecycle, artifact 교환 | 내부/외부 agent registry와 orchestrator의 표준 후보 |
| Commerce | `2026-06-06/UCP_Deep_Dive.html` | 상품 탐색, cart, checkout, order, identity linking, merchant capability discovery | 커머스, 쇼핑, 예약, 지도/로컬, 채널 파트너를 agent-readable commerce로 전환 |
| Payment Trust | `2026-06-06/AP2_Deep_Dive.html` | 결제 위임, 사용자 의도 증명, mandate, trusted surface, dispute evidence | 결제 플랫폼 agent 결제의 승인 UX, 감사, 분쟁 대응 기준 |
| Agent UI Events | `2026-06-06/AG_UI_Deep_Dive.html` | agent run lifecycle, text streaming, tool call, state delta, interrupt, custom event | 대화형 AI 서비스/CS/커머스 agent UX의 공통 event schema |
| Generative UI | `2026-06-06/A2_UI_Deep_Dive.html` | agent가 안전한 선언형 UI payload를 만들고 client가 native UI로 렌더링 | 채팅 UI 안의 상품 카드, 예약 form, 승인 summary, CS action UI |
| Security / Skills / Runtime | `2026-06-06/Selected_Agent_Project_Paper_Deep_Dive.html`, `2026-06-07/AgentBound_Deep_Dive.html`, `2026-06-07/SWE_Skills_Bench_Deep_Dive.html`, `2026-06-07/SkillNet_Deep_Dive.html` | Dynamic Workflows, AgentBound, SWE-Skills-Bench, COLLEAGUE.SKILL, Hermes Agent, SkillNet을 각각 분리 정리 | workflow runtime, MCP permission boundary, skill registry/eval, persistent agent governance |

## 3. Reading Paths

### 브리핑 / 전략

1. `2026-06-06/AI_Platform_Service_Applicability.html`
2. `2026-06-07/0900_agent_trend_brief.html`
3. `2026-06-06/2000_agent_trend_brief.html`
4. `2026-06-06/Selected_Agent_Project_Paper_Deep_Dive.html`
5. `2026-06-07/Claude_Code_Dynamic_Workflows_Deep_Dive.html`
6. `2026-06-07/AgentBound_Deep_Dive.html`
7. `2026-06-06/UCP_Deep_Dive.html`

### Agent Platform 설계

1. `2026-06-06/AI_Platform_Service_Applicability.html`
2. `2026-06-06/A2A_Deep_Dive.html`
3. `2026-06-07/Claude_Code_Dynamic_Workflows_Deep_Dive.html`
4. `2026-06-07/AgentBound_Deep_Dive.html`
5. `2026-06-07/Hermes_Agent_Deep_Dive.html`
6. `2026-06-07/SkillNet_Deep_Dive.html`
7. `2026-06-07/SWE_Skills_Bench_Deep_Dive.html`
8. `2026-06-06/AG_UI_Deep_Dive.html`

### 커머스 / 결제

1. `2026-06-06/UCP_Deep_Dive.html`
2. `2026-06-06/AP2_Deep_Dive.html`
3. `2026-06-06/A2_UI_Deep_Dive.html`

### UI / Product UX

1. `2026-06-06/AG_UI_Deep_Dive.html`
2. `2026-06-06/A2_UI_Deep_Dive.html`
3. `2026-06-06/UCP_Deep_Dive.html`

## 4. Reports

현재 공개된 HTML 산출물을 최신 업데이트순으로 정렬했다. 각 문서의 상세 업데이트 이력은 `DOCUMENT_HISTORY.html`의 History anchor에서 확인한다.

| 최신 업데이트 | 문서 | 유형 | 핵심 용도 | History |
| --- | --- | --- | --- | --- |
| 2026-06-07 09:00 KST | `DOCUMENT_HISTORY.html` | Operations | 각 HTML 정본의 생성/업데이트 타임라인과 Reports 정렬 기준 | `DOCUMENT_HISTORY.html#history-document-history` |
| 2026-06-07 09:00 KST | `index.html` | Hub | 전체 문서 탐색, 최신 업데이트순 Reports, source catalog, raw 링크 허브 | `DOCUMENT_HISTORY.html#history-index` |
| 2026-06-07 09:00 KST | `RESEARCH_LOG.html` | Operations | 날짜별 조사 주제, 조사 수준, 산출물 링크를 관리하는 이력 문서 | `DOCUMENT_HISTORY.html#history-research-log` |
| 2026-06-07 09:00 KST | `SOURCE_WATCHLIST.html` | Operations | 정기 확인할 공식 문서, 커뮤니티, 논문, 뉴스 source 목록. AX, Kimi Code, Cline, Copilot agent platform, Colab CLI, Codex Sites를 보조 소스로 추가 | `DOCUMENT_HISTORY.html#history-source-watchlist` |
| 2026-06-07 09:00 KST | `2026-06-07/0900_agent_trend_brief.html` | 8시간 Brief | AX, Kimi Code CLI, Cline SDK, Copilot Agent Tasks API, Colab CLI, Codex Sites 중심의 runtime/coding agent 업데이트 | `DOCUMENT_HISTORY.html#history-0900-brief` |
| 2026-06-07 01:12 KST | `2026-06-07/Hermes_Agent_Deep_Dive.html` | Detailed Deep Dive | persistent agent runtime, architecture, memory/profile state model, skills lifecycle, gateway/MCP/cron, security controls, PoC metrics | `DOCUMENT_HISTORY.html#history-hermes-agent` |
| 2026-06-07 00:37 KST | `2026-06-07/Claude_Code_Dynamic_Workflows_Deep_Dive.html` | Expanded Deep Dive | workflow runtime, subagent orchestration, task-specific harness, execution mechanics, evaluation protocol, platform blueprint | `DOCUMENT_HISTORY.html#history-dynamic-workflows` |
| 2026-06-07 00:37 KST | `2026-06-07/AgentBound_Deep_Dive.html` | Expanded Deep Dive | MCP server permission manifest, threat model, connector admission flow, evidence caveats, runtime audit schema | `DOCUMENT_HISTORY.html#history-agentbound` |
| 2026-06-07 00:37 KST | `2026-06-07/SWE_Skills_Bench_Deep_Dive.html` | Expanded Deep Dive | agent skill marginal utility, paired evaluation method, skill contract, token cost, internal eval playbook | `DOCUMENT_HISTORY.html#history-swe-skills-bench` |
| 2026-06-07 00:37 KST | `2026-06-07/COLLEAGUE_SKILL_Deep_Dive.html` | Expanded Deep Dive | trace-to-skill lifecycle, expert skill artifact contract, governance, role-task evaluation, consent/privacy controls | `DOCUMENT_HISTORY.html#history-colleague-skill` |
| 2026-06-07 00:37 KST | `2026-06-07/SkillNet_Deep_Dive.html` | Expanded Deep Dive | skill supply pipeline, repository, ontology, graph, 5-D evaluation, skill data model, research agenda | `DOCUMENT_HISTORY.html#history-skillnet` |
| 2026-06-07 00:24 KST | `2026-06-06/AI_Platform_Service_Applicability.html` | Synthesis | 조사한 프로토콜/프로젝트를 AI 플랫폼 및 서비스 적용 관점으로 재정리 | `DOCUMENT_HISTORY.html#history-ai-platform-applicability` |
| 2026-06-07 00:24 KST | `2026-06-06/A2A_Deep_Dive.html` | Deep Dive | agent-to-agent 표준과 Agent Registry/orchestrator 설계 | `DOCUMENT_HISTORY.html#history-a2a` |
| 2026-06-07 00:24 KST | `2026-06-06/Agent_Trend_Scout.html` | Scout | GeekNews, MarkTechPost, GitHub Blog, Reddit 기반 딥다이브 후보 발굴 | `DOCUMENT_HISTORY.html#history-agent-trend-scout` |
| 2026-06-07 00:24 KST | `2026-06-06/2000_agent_trend_brief.html` | 8시간 Brief | 정기 브리프 형식의 트렌드/업데이트 요약 | `DOCUMENT_HISTORY.html#history-2000-brief` |
| 2026-06-07 00:24 KST | `2026-06-06/Selected_Agent_Project_Paper_Deep_Dive.html` | Overview | Dynamic Workflows, AgentBound, skills, commerce stack, Hermes Agent, SkillNet 우선순위 매트릭스. 개별 딥다이브는 2026-06-07 문서로 분리 | `DOCUMENT_HISTORY.html#history-selected-overview` |
| 2026-06-07 00:24 KST | `2026-06-06/UCP_Deep_Dive.html` | Deep Dive | agentic commerce capability/discovery/checkout/order 표준 | `DOCUMENT_HISTORY.html#history-ucp` |
| 2026-06-07 00:24 KST | `2026-06-06/AP2_Deep_Dive.html` | Deep Dive | agent 결제 위임, mandate, trusted surface, dispute evidence | `DOCUMENT_HISTORY.html#history-ap2` |
| 2026-06-07 00:24 KST | `2026-06-06/AG_UI_Deep_Dive.html` | Deep Dive | agent frontend event protocol, interrupt, state delta, tool call UX | `DOCUMENT_HISTORY.html#history-ag-ui` |
| 2026-06-07 00:24 KST | `2026-06-06/A2_UI_Deep_Dive.html` | Deep Dive | agent-generated declarative UI payload/spec와 AI 플랫폼 component catalog 설계 | `DOCUMENT_HISTORY.html#history-a2-ui` |
| 2026-06-07 00:24 KST | `README.html` | Operations | 아카이브 구조, 운영 방식, 문서 접근 경로 안내 | `DOCUMENT_HISTORY.html#history-readme` |
| 2026-06-07 00:24 KST | `REPORTING_STANDARD.html` | Operations | 브리핑 작성 원칙, 출처 표기, 문서 품질 기준 | `DOCUMENT_HISTORY.html#history-reporting-standard` |

## 5. Source Catalog

이 영역은 전체 소스 탐색용 카탈로그다. 개별 딥다이브의 근거 링크는 마지막에 몰아두지 않고, 관련 요약과 판단이 나오는 섹션 바로 아래에 `참고 링크:`로 붙인다.

### Watch Sources / Communities

- [GeekNews](https://news.hada.io/)
- [MarkTechPost](https://www.marktechpost.com/)
- [GitHub Blog](https://github.blog/)
- [r/Agent_AI](https://www.reddit.com/r/Agent_AI/)
- [r/AI_Agents](https://www.reddit.com/r/AI_Agents/)
- [r/ClaudeCode](https://www.reddit.com/r/ClaudeCode/)
- [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/)
- [r/claudeskills](https://www.reddit.com/r/claudeskills/)
- [r/vibecoding](https://www.reddit.com/r/vibecoding/)
- [r/mcp](https://www.reddit.com/r/mcp/)

### A2A / Agent Interoperability

- [Google A2A](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/)
- [Linux Foundation launch](https://www.linuxfoundation.org/press/linux-foundation-launches-the-agent2agent-protocol-project-to-enable-secure-intelligent-communication-between-ai-agents)
- [Linux Foundation update](https://www.linuxfoundation.org/press/a2a-protocol-surpasses-150-organizations-lands-in-major-cloud-platforms-and-sees-enterprise-production-use-in-first-year)
- [A2A docs](https://a2a-protocol.org/latest/)
- [A2A v1.0](https://a2a-protocol.org/latest/announcing-1.0/)
- [Key Concepts](https://a2a-protocol.org/latest/topics/key-concepts/)
- [Agent Discovery](https://a2a-protocol.org/latest/topics/agent-discovery/)
- [Specification](https://a2a-protocol.org/dev/specification/)
- [Enterprise Ready](https://a2a-protocol.org/latest/topics/enterprise-ready/)
- [A2A and MCP](https://a2a-protocol.org/latest/topics/a2a-and-mcp/)
- [Agent protocol guide](https://developers.googleblog.com/developers-guide-to-ai-agent-protocols/)

### MCP / Agents SDK / Runtime Docs

- [MCP Docs](https://modelcontextprotocol.io/)
- [MCP Blog](https://blog.modelcontextprotocol.io/)
- [OpenAI Agents SDK](https://openai.github.io/openai-agents-python/)
- [OpenAI News](https://openai.com/news/)
- [Codex Sites](https://developers.openai.com/codex/sites)
- [Claude Blog](https://claude.com/blog)
- [Claude Code Docs](https://code.claude.com/docs)
- [Dynamic Workflows](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code)
- [open-dynamic-workflows](https://github.com/imsai-sh/open-dynamic-workflows)
- [Hermes Docs Home](https://hermes-agent.nousresearch.com/docs/)
- [Hermes Docs](https://hermes-agent.nousresearch.com/docs/user-guide/skills/bundled/autonomous-ai-agents/autonomous-ai-agents-hermes-agent)
- [Hermes Tools](https://hermes-agent.nousresearch.com/docs/user-guide/features/tools)
- [Hermes Product Page](https://nousresearch.com/hermes-agent/)
- [Hermes GitHub](https://github.com/NousResearch/hermes-agent)
- [Google AX Site](https://agentexecutor.io/)
- [Google AX GitHub](https://github.com/google/ax)
- [Cline SDK](https://docs.cline.bot/sdk/overview)
- [Cline GitHub](https://github.com/cline/cline)

### Security / Permission Boundary / Skills

- [AgentBound PDF](https://programming-group.com/assets/pdf/papers/2026_AgentBound-Securing-Execution-Boundaries-of-AI-Agents.pdf)
- [AgentBound FSE](https://conf.researchr.org/details/fse-2026/fse-2026-research-papers/14/AgentBound-Securing-Execution-Boundaries-of-AI-Agents)
- [AgentBound arXiv](https://arxiv.org/abs/2510.21236)
- [AgentBound replication](https://zenodo.org/records/19468201)
- [OpenFGA MCP authorization](https://openfga.dev/docs/modeling/agents/mcp-authorization)
- [MCP permission analysis](https://mcpblog.dev/blog/2026-03-21-chmod-ai-agents-mcp-permissions)
- [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401)
- [SWE-Skills-Bench GitHub](https://github.com/GeniusHTX/SWE-Skills-Bench)
- [COLLEAGUE.SKILL](https://arxiv.org/abs/2605.31264)
- [COLLEAGUE.SKILL HF](https://huggingface.co/papers/2605.31264)
- [Skill trace auditing](https://arxiv.org/abs/2605.11946)
- [SkillNet paper](https://arxiv.org/abs/2603.04448)
- [SkillNet GitHub](https://github.com/zjunlp/SkillNet)
- [SkillNet site](https://skillnet.openkg.cn)
- [SkillNet HF blog](https://huggingface.co/blog/xzwnlp/skillnet)

### UCP / Agentic Commerce

- [UCP Docs](https://ucp.dev/)
- [UCP Core Concepts](https://ucp.dev/documentation/core-concepts/)
- [UCP Specification](https://ucp.dev/latest/specification/overview/)
- [UCP Schema Reference](https://ucp.dev/2026-04-08/specification/reference/)
- [UCP and AP2](https://ucp.dev/documentation/ucp-and-ap2/)
- [UCP Versioning](https://ucp.dev/versioning/)
- [UCP GitHub](https://github.com/Universal-Commerce-Protocol/ucp)
- [UCP Discussions](https://github.com/Universal-Commerce-Protocol/ucp/discussions)
- [Under the Hood UCP](https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/)
- [Agentic shopping](https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/)
- [Shopify Engineering UCP](https://shopify.engineering/UCP)
- [Shopify UCP](https://www.shopify.com/ucp)
- [UCP updates](https://blog.google/products-and-platforms/products/shopping/ucp-updates/)
- [Universal Cart](https://blog.google/products-and-platforms/products/shopping/shopping-updates-google-marketing-live/)
- [Axios coverage](https://www.axios.com/2026/01/11/google-shopify-ai-shopping-standard-nrf-2026)
- [InfoQ coverage](https://www.infoq.com/news/2026/01/google-agentic-commerce-ucp/)
- [Strabo paper](https://arxiv.org/abs/2606.05043)

### AP2 / Agent Payments

- [AP2 Docs](https://ap2-protocol.org/)
- [AP2 Specification](https://ap2-protocol.org/ap2/specification/)
- [Google agentic commerce GitHub](https://github.com/google-agentic-commerce)
- [AP2 GitHub](https://github.com/google-agentic-commerce/AP2)
- [AP2 docs repo](https://github.com/google-agentic-commerce/AP2/tree/main/docs)
- [AP2 code samples](https://github.com/google-agentic-commerce/AP2/tree/main/code/samples)
- [Human-present sample](https://github.com/google-agentic-commerce/AP2/tree/main/code/samples/python/scenarios/a2a/human-present/cards)
- [Human-not-present sample](https://github.com/google-agentic-commerce/AP2/tree/main/code/samples/python/scenarios/a2a/human-not-present/cards)
- [x402 sample](https://github.com/google-agentic-commerce/AP2/tree/main/code/samples/python/scenarios/a2a/human-not-present/x402)
- [Android credential sample](https://github.com/google-agentic-commerce/AP2/tree/main/code/samples/android/scenarios/digital-payment-credentials)
- [Google Cloud AP2](https://cloud.google.com/blog/products/ai-machine-learning/announcing-agents-to-payments-ap2-protocol)
- [AP2 and FIDO](https://blog.google/products-and-platforms/platforms/google-pay/agent-payments-protocol-fido-alliance/)
- [FIDO standards](https://fidoalliance.org/fido-alliance-to-develop-standards-for-trusted-ai-agent-interactions/)
- [Zero-trust payment paper](https://arxiv.org/abs/2602.06345)
- [x402 Extended paper](https://arxiv.org/abs/2601.22569)

### AG-UI / A2-UI / Generative UI

- [AG-UI Docs](https://docs.ag-ui.com/)
- [AG-UI Introduction](https://docs.ag-ui.com/introduction)
- [AG-UI Events](https://docs.ag-ui.com/concepts/events)
- [AG-UI State](https://docs.ag-ui.com/concepts/state.md)
- [AG-UI Interrupts](https://docs.ag-ui.com/concepts/interrupts)
- [AG-UI Tools](https://docs.ag-ui.com/concepts/tools)
- [AG-UI Generative UI](https://docs.ag-ui.com/concepts/generative-ui-specs.md)
- [AG-UI Reasoning](https://docs.ag-ui.com/concepts/reasoning)
- [AG-UI TypeScript SDK](https://docs.ag-ui.com/sdk/js/core/overview)
- [AG-UI Python SDK](https://docs.ag-ui.com/sdk/python/core/overview)
- [AG-UI GitHub](https://github.com/ag-ui-protocol/ag-ui)
- [AG-UI Dojo](https://dojo.ag-ui.com/)
- [A2UI Docs](https://a2ui.org/)
- [What is A2UI](https://a2ui.org/introduction/what-is-a2ui/)
- [A2UI Core Concepts](https://a2ui.org/concepts/overview/)
- [A2UI v0.8](https://a2ui.org/specification/v0.8-a2ui/)
- [A2UI v0.9](https://a2ui.org/specification/v0_9/docs/a2ui_protocol/)
- [A2UI v0.10](https://a2ui.org/specification/v0_10/docs/a2ui_protocol/)
- [A2UI GitHub](https://github.com/a2ui-project/a2ui)
- [A2UI spec folder](https://github.com/a2ui-project/a2ui/tree/main/specification)
- [AG2 A2UI docs](https://docs.ag2.ai/latest/docs/user-guide/a2a/a2ui/)
- [AG2 A2UI agent](https://docs.ag2.ai/latest/docs/user-guide/reference-agents/a2uiagent/)
- [A2UI Composer](https://a2ui-composer.ag-ui.com/)
- [AG-UI/A2UI explainer](https://www.copilotkit.ai/docs/AG-UI-and-A2UI-Explained.pdf)
- [A2UI walkthrough](https://www.copilotkit.ai/blog/build-with-googles-new-a2ui-spec-agent-user-interfaces-with-a2ui-ag-ui)
- [Macaron-A2UI](https://arxiv.org/abs/2605.24830)

### GitHub / Sandbox / Agent Web Signals

- [GitHub Copilot App](https://github.blog/news-insights/product-news/github-copilot-app-the-agent-native-desktop-experience/)
- [Copilot coding agent](https://github.blog/ai-and-ml/github-copilot/whats-new-with-github-copilot-coding-agent/)
- [Copilot App preview](https://github.blog/changelog/2026-06-02-expanded-technical-preview-availability-for-the-github-copilot-app/)
- [Agent Tasks API](https://github.blog/changelog/2026-06-04-agent-tasks-rest-api-now-available-for-copilot-pro-pro-and-max/)
- [Copilot CLI scheduling](https://github.blog/changelog/2026-06-02-copilot-cli-improved-ui-rubber-duck-prompt-scheduling-and-voice-input/)
- [Enterprise plugins](https://github.blog/changelog/2026-06-05-enterprise-managed-plugins-in-vs-code-in-public-preview/)
- [Kimi Code CLI](https://github.com/MoonshotAI/kimi-code)
- [Kimi Code Docs](https://moonshotai.github.io/kimi-code/)
- [Google Colab CLI](https://developers.googleblog.com/introducing-the-google-colab-cli/)
- [gh skill](https://github.blog/changelog/2026-04-16-manage-agent-skills-with-github-cli/)
- [Copilot VS Code release](https://github.blog/changelog/2026-03-06-github-copilot-in-visual-studio-code-v1-110-february-release/)
- [Copilot CLI GA](https://github.blog/changelog/2026-02-25-github-copilot-cli-is-now-generally-available/)
- [AIO Sandbox article](https://www.marktechpost.com/2026/03/29/agent-infra-releases-aio-sandbox-an-all-in-one-runtime-for-ai-agents-with-browser-shell-shared-filesystem-and-mcp/)
- [AIO Sandbox GitHub](https://github.com/AgentInfra-AI/aio-sandbox)
- [WebMCP article](https://www.marktechpost.com/2026/02/14/google-ai-introduces-the-webmcp-to-enable-direct-and-structured-website-interactions-for-new-ai-agents/)
- [A-Evolve article](https://www.marktechpost.com/2026/03/29/meet-a-evolve-the-pytorch-moment-for-agentic-ai-systems-replacing-manual-tuning-with-automated-state-mutation-and-self-correction/)
- [MarkTechPost UCP](https://www.marktechpost.com/2026/01/12/google-ai-releases-universal-commerce-protocol-ucp-an-open-source-standard-designed-to-power-the-next-generation-of-agentic-commerce/)

### Community Threads / Discussion Signals

- [r/ClaudeCode Dynamic Workflows](https://www.reddit.com/r/ClaudeCode/comments/1tq9pge/introducing_dynamic_workflows_in_claude_code/)
- [r/ClaudeAI Dynamic Workflows](https://www.reddit.com/r/ClaudeAI/comments/1tq9ofy/introducing_dynamic_workflows_in_claude_code/)
- [r/vibecoding Dynamic Workflows](https://www.reddit.com/r/vibecoding/comments/1tqe2yp/anthropic_just_introduced_dynamic_workflows_in/)
- [Claude Code workflows thread](https://www.reddit.com/r/ClaudeCode/comments/1tkjy4u/claude_code_dropped_workflows/)
- [Production MCP thread](https://www.reddit.com/r/ClaudeAI/comments/1tuqqpn/i_ship_ai_agents_in_production_the_mess_is_mcp/)
- [MCP server thread](https://www.reddit.com/r/ClaudeAI/comments/1s4gz18/built_an_mcp_server_that_turns_claude_code_into_a/)
- [Agent runtime thread](https://www.reddit.com/r/AI_Agents/comments/1tgwlh1/your_vibe_coded_repo_is_rotting_i_built_an_open/)
- [Hermes tool/skill bloat](https://www.reddit.com/r/hermesagent/comments/1t34qee/hermes_agent_tool_and_skills_bloat/)
- [Hermes skills loading](https://www.reddit.com/r/hermesagent/comments/1tp0by4/does_hermes_actually_load_the_skills_every_single/)
- [Hermes cron monitoring](https://www.reddit.com/r/hermesagent/comments/1t9gz2f/the_cron_job_every_serious_hermes_agent_user/)
- [GeekNews topic 25327](https://news.hada.io/topic?id=25327)
- [GeekNews topic 27108](https://news.hada.io/topic?id=27108)
- [GeekNews topic 27530](https://news.hada.io/topic?id=27530)
- [GeekNews topic 27636](https://news.hada.io/topic?id=27636)
- [GeekNews topic 29171](https://news.hada.io/topic?id=29171)
- [GeekNews topic 29493](https://news.hada.io/topic?id=29493)
- [GeekNews topic 30028](https://news.hada.io/topic?id=30028)

## 6. Raw Sources

- `2026-06-06/raw/A2A_Deep_Dive.md`
- `2026-06-07/raw/0900_agent_trend_brief.md`
- `2026-06-06/raw/AI_Platform_Service_Applicability.md`
- `2026-06-06/raw/Agent_Trend_Scout.md`
- `2026-06-06/raw/2000_agent_trend_brief.md`
- `2026-06-06/raw/Selected_Agent_Project_Paper_Deep_Dive.md`
- `2026-06-07/raw/Claude_Code_Dynamic_Workflows_Deep_Dive.md`
- `2026-06-07/raw/AgentBound_Deep_Dive.md`
- `2026-06-07/raw/SWE_Skills_Bench_Deep_Dive.md`
- `2026-06-07/raw/COLLEAGUE_SKILL_Deep_Dive.md`
- `2026-06-07/raw/Hermes_Agent_Deep_Dive.md`
- `2026-06-07/raw/SkillNet_Deep_Dive.md`
- `2026-06-06/raw/UCP_Deep_Dive.md`
- `2026-06-06/raw/AP2_Deep_Dive.md`
- `2026-06-06/raw/AG_UI_Deep_Dive.md`
- `2026-06-06/raw/A2_UI_Deep_Dive.md`
- `2026-06-07/raw/DOCUMENT_HISTORY.md`
- `2026-06-06/raw/RESEARCH_LOG.md`
- `2026-06-07/raw/SOURCE_WATCHLIST.md`
- `2026-06-06/raw/REPORTING_STANDARD.md`
- `2026-06-06/raw/README.md`

## 7. Operations

- `DOCUMENT_HISTORY.html`: 각 HTML 정본의 업데이트 히스토리.
- `RESEARCH_LOG.html`: 날짜별 조사 이력.
- `SOURCE_WATCHLIST.html`: 정기 확인 소스 목록.
- `REPORTING_STANDARD.html`: 브리핑과 내부 공유 작성 원칙.
