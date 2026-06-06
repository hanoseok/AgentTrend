# Agent Trend Scout

- 작성 시점: 2026-06-06 19:29 KST
- 목적: A2A 외에도 최근 사람들이 많이 보고 있고 딥다이브 후보가 될 만한 에이전트 트렌드/프로젝트를 스카우팅
- 확인 소스: GeekNews, MarkTechPost, GitHub Blog, Reddit r/Agent_AI, r/ClaudeCode, r/claudeskills, r/vibecoding 및 관련 Reddit/공식 링크
- 조사 수준: Scout, 딥다이브 후보 발굴

## Executive Summary

이번 스캔에서 가장 강한 신호는 세 가지다.

1. 코딩 에이전트가 단일 채팅/단일 에이전트에서 벗어나 병렬 subagent, dynamic workflow, agent-native control center로 이동 중이다.
2. MCP는 계속 중요하지만, "그냥 많이 연결하면 된다"는 흐름은 꺾이고 있다. production에서는 tool bloat, token bloat, 권한/감사/관찰 가능성이 핵심 병목으로 떠오른다.
3. agent-ready web, runtime sandbox, skills marketplace, agentic commerce protocol stack은 카카오 AI 서비스/에이전트 플랫폼에서 제품화 가능성이 높은 주변 인프라 트렌드다.

## 딥다이브 후보

| 우선순위 | 후보 주제 | 왜 지금 트렌디한가 | 딥다이브 필요성 |
| --- | --- | --- | --- |
| 1 | Claude Code Dynamic Workflows / ultracode | Anthropic 공식 발표와 Reddit 반응이 동시에 강함. 병렬 subagent, 장시간 작업, workflow-as-code가 핵심 | 카카오 사내 개발/운영 에이전트, 대규모 리팩토링, 보안 감사 자동화에 직접 연결 |
| 2 | MCP Production Mess / Tool & Token Governance | GeekNews와 Reddit 모두 MCP의 실전 문제를 다룸. tool schema bloat, 잘못된 tool selection, context 낭비가 반복 이슈 | 카카오 플랫폼에서 MCP를 무작정 붙이면 같은 문제가 생김. registry, policy, routing, eval이 필요 |
| 3 | GitHub Agent-Native Developer Experience | GitHub Copilot App, Canvas, sandbox, code review, SDK가 "agent control center"로 수렴 | 카카오 내부 AI 개발 플랫폼 UX와 운영 모델 설계에 참고 가치가 큼 |
| 4 | Agent Runtime Sandbox | MarkTechPost의 AIO Sandbox 등 agent가 실행할 브라우저/쉘/파일시스템 환경이 별도 계층으로 부상 | 에이전트가 실제 일을 하려면 격리 실행 환경, 세션 상태, 권한 통제가 필요 |
| 5 | Skills / CLAUDE.md / On-demand Skill Marketplace | r/claudeskills와 GitHub Copilot Skills 흐름이 강함. 도구보다 작은 단위의 reusable know-how가 중요해짐 | 카카오 서비스별 업무 지식, 정책, 운영 절차를 skill화할 수 있음 |
| 6 | Agentic Web / Browser MCP / WebMCP | Vibium, WebMCP, webclaw 등 웹을 agent-ready하게 만드는 시도가 늘어남 | 카카오 웹/모바일 서비스가 에이전트에게 어떻게 노출될지 설계해야 함 |
| 7 | Agentic Commerce Protocol Stack: UCP/AP2/A2UI/AG-UI | Google protocol guide가 MCP, A2A, UCP, AP2, A2UI, AG-UI를 하나의 stack으로 묶음 | 카카오 쇼핑/선물하기/결제/예약에 전략적으로 중요 |
| 8 | Agent Observability / Memory / Audit Trail | Repowise, Cortex, Octopoda, Hermes 등 커뮤니티 프로젝트들이 "agent가 뭘 했는지 보자"에 집중 | 카카오 플랫폼의 trace, memory, approval, audit baseline에 필요 |

## 후보별 메모

### 1. Claude Code Dynamic Workflows / ultracode

- 소스: Anthropic 공식 블로그, Reddit r/ClaudeCode
- 확인 내용: Claude가 orchestration script를 직접 만들고, tens to hundreds of parallel subagents를 한 세션에서 실행하며, 결과 검증 후 사용자에게 전달하는 research preview.
- 커뮤니티에서 나온 말: 기존 main-agent orchestration은 subagent 결과가 다시 main context로 들어오며 token tax를 만든다는 지적. Reddit에서는 control flow는 code가 맡고, judgment는 model이 맡는 구조가 흥미롭다는 반응이 있음.
- 트렌드 판단: "AI가 코드를 쓴다"에서 "AI가 장시간 병렬 작업 시스템을 운영한다"로 이동하는 신호.
- 카카오 의미: 대규모 코드 마이그레이션, 보안 감사, 사내 문서/서비스 변경 추적, 장애 원인 분석에 적용 가능.

### 2. MCP Production Mess / Tool & Token Governance

- 소스: GeekNews "MCP는 죽었나?", "MCP는 죽었다; MCP 만세", "Context Mode", Reddit "I ship AI agents in production. The mess is MCP."
- 확인 내용: 많은 MCP 서버와 tool schema가 붙으면 첫 프롬프트 전부터 context가 소모되고, tool description 때문에 잘못된 tool selection이 발생한다는 실전 문제가 반복됨.
- 커뮤니티에서 나온 말: "MCP in production is messy"라는 방향의 문제 제기. Skills나 CLI wrapper가 더 효율적이라는 주장도 등장.
- 트렌드 판단: MCP는 표준으로 살아남되, 핵심은 MCP 자체가 아니라 tool governance, token budget, scoped tool loading, policy, observability로 이동 중.
- 카카오 의미: 내부 MCP registry에 tool quality score, token cost, permission tier, eval score, usage trace를 붙여야 함.

### 3. GitHub Agent-Native Developer Experience

- 소스: GitHub Blog, Copilot App, Copilot CLI, Copilot coding agent 업데이트
- 확인 내용: GitHub는 agent-native desktop, Canvas, sandbox, code review, SDK, scheduled tasks, memory를 하나의 개발자 workflow로 묶고 있음.
- 트렌드 판단: IDE 보조 기능이 아니라 "agent control center"가 등장 중. 여러 agent가 병렬로 일하고, 인간은 상태를 보고 지시/검토/승인하는 UX가 중요해짐.
- 카카오 의미: 카카오 내부 개발 플랫폼에도 agent task board, trace view, approval queue, sandbox result, code review integration이 필요할 가능성이 큼.

### 4. Agent Runtime Sandbox

- 소스: MarkTechPost Agent-Infra AIO Sandbox
- 확인 내용: agent 실행 병목이 모델 추론에서 browser, shell, shared filesystem, MCP, IDE/Jupyter 관찰 환경을 갖춘 실행 runtime으로 이동.
- 트렌드 판단: agent platform은 LLM wrapper가 아니라 격리 실행 환경, 상태 공유, artifact 관리, resource limit을 제공하는 runtime platform이 되어야 함.
- 카카오 의미: 사내/서비스 agent가 실제 API, 브라우저, 파일, 코드 실행을 할 때 sandbox isolation과 policy enforcement가 필수.

### 5. Skills / CLAUDE.md / On-demand Skill Marketplace

- 소스: r/claudeskills, GitHub Copilot CLI/VS Code agent skills
- 확인 내용: book-to-skill, CLAUDE.md generator, persistent project memory skill, frontend animation skill 등 domain know-how를 skill로 패키징하는 흐름이 큼.
- 트렌드 판단: 도구는 action을 제공하고, skill은 판단 절차/업무 지식/검증 기준을 제공한다. 에이전트 품질이 prompt보다 reusable skill asset으로 이동 중.
- 카카오 의미: 카카오 서비스 운영 정책, CS 대응 절차, 결제/예약 승인 규칙, 장애 대응 runbook을 skill asset으로 만들 수 있음.

### 6. Agentic Web / Browser MCP / WebMCP

- 소스: GeekNews Vibium, MarkTechPost WebMCP, Reddit webclaw
- 확인 내용: 웹페이지를 사람이 보는 pixel/scraping 대상이 아니라 agent가 호출할 structured toolkit으로 바꾸려는 시도가 늘어남.
- 트렌드 판단: agent-ready web은 기존 SEO/앱스토어/오픈API와 비슷한 새 노출 계층이 될 수 있음.
- 카카오 의미: 카카오 서비스가 외부/내부 agent에게 어떤 capability를 어떤 권한으로 노출할지 설계해야 함.

### 7. Agentic Commerce Protocol Stack: UCP/AP2/A2UI/AG-UI

- 소스: Google Developer's Guide to AI Agent Protocols, GeekNews AI 에이전트 프로토콜 개발자 가이드, MarkTechPost UCP
- 확인 내용: MCP는 데이터/도구, A2A는 에이전트 협업, UCP는 상거래, AP2는 결제 승인/audit, A2UI는 UI composition, AG-UI는 streaming UI로 역할 분담.
- 트렌드 판단: commerce agent는 추천에서 결제/승인/사후처리로 넘어가고 있으며, 프로토콜이 stack화되고 있음.
- 카카오 의미: 쇼핑/선물하기/페이/로컬/예약을 하나의 agentic commerce 흐름으로 묶을 때 핵심 참고 구조.

### 8. Agent Observability / Memory / Audit Trail

- 소스: Reddit Repowise, Cortex, Octopoda, Hermes Agent
- 확인 내용: codebase intelligence, task lifecycle, review/approval gate, loop detection, persistent memory, durable Kanban, rollback/checkpoint 같은 프로젝트가 반복 등장.
- 트렌드 판단: agent output보다 agent operation을 관리하는 계층이 뜨고 있음.
- 카카오 의미: 플랫폼의 traceId/contextId, approval state, audit replay, agent memory, evaluation gate 설계에 직접 연결.

## 제안

딥다이브 우선순위는 다음 순서가 좋다.

1. Claude Code Dynamic Workflows / ultracode
2. MCP Production Mess / Tool & Token Governance
3. Agentic Commerce Protocol Stack: UCP/AP2/A2UI/AG-UI
4. GitHub Agent-Native Developer Experience
5. Agent Runtime Sandbox

카카오 AI 서비스와 에이전트 플랫폼 관점에서 1번과 2번은 내부 플랫폼 설계에 바로 영향을 준다. 3번은 카카오 쇼핑/선물하기/페이/예약 전략에 중요하다.

## 출처

- GeekNews, Vibium: https://news.hada.io/topic?id=25327
- GeekNews, AI 에이전트 프로토콜 개발자 가이드: https://news.hada.io/topic?id=27636
- GeekNews, Context Mode: https://news.hada.io/topic?id=27108
- GeekNews, MCP는 죽었다; MCP 만세: https://news.hada.io/topic?id=27530
- GeekNews, MCP는 죽었나?: https://news.hada.io/topic?id=30028
- GeekNews, Code w/ Claude에서 발표한 모든 것들: https://news.hada.io/topic?id=29493
- GeekNews, 에이전트 경제의 블루오션 기회: https://news.hada.io/topic?id=29171
- MarkTechPost, WebMCP: https://www.marktechpost.com/2026/02/14/google-ai-introduces-the-webmcp-to-enable-direct-and-structured-website-interactions-for-new-ai-agents/
- MarkTechPost, AIO Sandbox: https://www.marktechpost.com/2026/03/29/agent-infra-releases-aio-sandbox-an-all-in-one-runtime-for-ai-agents-with-browser-shell-shared-filesystem-and-mcp/
- MarkTechPost, A-Evolve: https://www.marktechpost.com/2026/03/29/meet-a-evolve-the-pytorch-moment-for-agentic-ai-systems-replacing-manual-tuning-with-automated-state-mutation-and-self-correction/
- MarkTechPost, UCP: https://www.marktechpost.com/2026/01/12/google-ai-releases-universal-commerce-protocol-ucp-an-open-source-standard-designed-to-power-the-next-generation-of-agentic-commerce/
- GitHub Blog, Copilot app: https://github.blog/news-insights/product-news/github-copilot-app-the-agent-native-desktop-experience/
- GitHub Blog, Copilot CLI GA: https://github.blog/changelog/2026-02-25-github-copilot-cli-is-now-generally-available/
- GitHub Blog, Copilot coding agent update: https://github.blog/ai-and-ml/github-copilot/whats-new-with-github-copilot-coding-agent/
- GitHub Blog, VS Code Copilot v1.110: https://github.blog/changelog/2026-03-06-github-copilot-in-visual-studio-code-v1-110-february-release/
- Anthropic, Dynamic workflows: https://claude.com/blog/introducing-dynamic-workflows-in-claude-code
- Google Developers Blog, AI Agent Protocols: https://developers.googleblog.com/developers-guide-to-ai-agent-protocols/
- Reddit r/Agent_AI: https://www.reddit.com/r/Agent_AI/
- Reddit r/ClaudeCode dynamic workflows: https://www.reddit.com/r/ClaudeCode/comments/1tq9pge/introducing_dynamic_workflows_in_claude_code/
- Reddit r/ClaudeCode /workflows discussion: https://www.reddit.com/r/ClaudeCode/comments/1tkjy4u/claude_code_dropped_workflows/
- Reddit r/claudeskills: https://www.reddit.com/r/claudeskills/
- Reddit r/AI_Agents Repowise: https://www.reddit.com/r/AI_Agents/comments/1tgwlh1/your_vibe_coded_repo_is_rotting_i_built_an_open/
- Reddit r/ClaudeAI MCP production mess: https://www.reddit.com/r/ClaudeAI/comments/1tuqqpn/i_ship_ai_agents_in_production_the_mess_is_mcp/
- Reddit r/ClaudeAI Octopoda: https://www.reddit.com/r/ClaudeAI/comments/1s4gz18/built_an_mcp_server_that_turns_claude_code_into_a/
