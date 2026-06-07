# 0900 Agent Trend Brief

- 작성 시점: 2026-06-07 09:00 KST
- 범위: Agent Runtime / MCP / Skills / Cloud Execution / Agent UX
- HTML: `2026-06-07/0900_agent_trend_brief.html`

## 1. Executive Summary

이번 정기 브리프의 핵심은 agent 경쟁축이 대화 UI에서 실행 runtime/control plane으로 이동하고 있다는 점이다. Google Agent Executor(AX), Kimi Code CLI, Cline SDK/runtime, GitHub Copilot Agent Tasks API는 모두 agent를 durable task, event log, worktree/session isolation, scheduled/background execution, policy/audit 대상으로 다룬다.

Sources: [Google AX](https://github.com/google/ax), [Kimi Code CLI](https://github.com/MoonshotAI/kimi-code), [Cline SDK](https://docs.cline.bot/sdk/overview), [Copilot App](https://github.blog/changelog/2026-06-02-expanded-technical-preview-availability-for-the-github-copilot-app/), [Agent Tasks API](https://github.blog/changelog/2026-06-04-agent-tasks-rest-api-now-available-for-copilot-pro-pro-and-max/)

핵심 판단:

1. Runtime layer가 본격 경쟁축이다. AX, Cline SDK, Copilot cloud agent API는 resumable execution, event log, worktree isolation, API-triggered background jobs를 공통 방향으로 보여준다.
2. Terminal coding agent 표준 기능이 수렴한다. Kimi Code CLI, Cline, GitHub Copilot CLI 모두 MCP, skills, subagents, hooks, scheduling, approval 흐름을 핵심 기능으로 내세운다.
3. Skills는 축적보다 관리가 문제다. r/claudeskills에서는 Task Observer, claude-spellbook, skill libraries가 계속 공유되며 다음 병목은 검증, token overhead, drift, ownership, rollback이다.

이번 회차 결론: AI 플랫폼 및 서비스가 지금부터 설계해야 할 공통 layer는 agent execution ledger, MCP/tool permission matrix, skill registry/eval, scheduled run policy, channel-specific approval, artifact/canvas review surface다.

## 2. Source Coverage

| 소스 | 확인 여부 | 유의미한 변화 | 후속 액션 |
|---|---|---|---|
| GeekNews | Checked | Agent Executor, Codex Sites, Anthropic recursive self-improvement, AI-native startup 운영 모델, harness engineering 글이 상위권에 노출. | AX와 Codex Sites는 공식 출처로 교차 확인. AX는 딥다이브 후보. |
| MarkTechPost | Checked | Kimi Code CLI가 2026-06-06 신규 릴리스로 노출. Colab CLI, Cline SDK, local/server inference orchestration도 agentic infrastructure 흐름으로 확인. | Kimi Code CLI는 공식 GitHub와 함께 Project Alert에 올림. |
| GitHub Blog / Changelog | Checked | Copilot App technical preview 확대, canvases, cloud automations, Copilot CLI scheduling/voice/rubber duck, Agent tasks REST API, enterprise-managed plugins, 1M token context가 확인됨. | 기존 agent-native developer platform 판단을 강화. |
| Reddit r/Agent_AI | Checked | Hermes workflow 실사용, Agent Arena leaderboard, AI shopping agent trust, portfolio automation 등 persistent agent와 trust 논점이 반복. | 커뮤니티 신호로 분리. 공식 출처 없는 성능/수익 주장은 미확인 처리. |
| Reddit r/ClaudeCode | Checked | dynamic workflows 이후 비용/속도/검증 가능성, Opus 4.8 속도와 안정성 논쟁이 지속. | workflow runtime에는 cost cap, inspectable trace, checkpoint가 필요하다는 기존 판단 유지. |
| Reddit r/claudeskills | Checked | Task Observer, claude-spellbook, skills-for-humanity, second-brain fragmentation 문제가 활발. | Task Observer를 skill lifecycle 후보로 추적. |
| Reddit r/vibecoding | Checked | 이번 회차에는 공식 출처로 교차 확인 가능한 신규 핵심 프로젝트는 낮음. 기존의 quality/craft/maintenance 논쟁은 지속. | 다음 회차 계속 확인. |

## 3. Project / Paper Alert

### Google Agent Executor (AX)

Sources: [AX Site](https://agentexecutor.io/), [GitHub](https://github.com/google/ax), [GeekNews](https://news.hada.io/)

- 우선순위: High
- 왜 이슈인가: Google이 공개한 early-preview distributed agent runtime. long-running workers, resumable streams, event log, isolated actors, MCP/A2A support, audit/policy를 한 레이어에 묶는다.
- 사람들이 하는 말/논쟁: GeekNews에서 주목받고, r/AI_Agents의 Kubernetes-native agent runtime 논의에서도 AX가 비교 기준으로 등장한다.
- 기술 핵심: single controller, durable event log, isolated agents/tools/skills, Kubernetes target, MCP/A2A protocol support.
- 검증 필요점: early development, breaking changes 예정, 외부 PR 일시 중단. 실제 API 안정성, security boundary, policy language 확인 필요.
- 딥다이브 추천: Yes. runtime/control-plane 관점에서 최우선.

### Kimi Code CLI

Sources: [GitHub](https://github.com/MoonshotAI/kimi-code), [MarkTechPost](https://www.marktechpost.com/2026/06/06/moonshot-ai-releases-kimi-code-cli-a-terminal-ai-coding-agent-built-in-typescript-for-next-gen-agents/), [Docs](https://moonshotai.github.io/kimi-code/)

- 우선순위: High
- 왜 이슈인가: Moonshot AI가 terminal-first coding agent를 MIT license로 공개. TypeScript, single-binary install, MCP conversational config, plugin marketplace, subagents, lifecycle hooks, ACP integration을 묶는다.
- 사람들이 하는 말/논쟁: terminal coding agent 시장이 Claude Code/Codex/Gemini/Cline 중심에서 더 넓어지는 신호. 모델 경쟁보다 agent harness UX 경쟁으로 해석된다.
- 기술 핵심: read/edit/run/search/fetch loop, approval flow, built-in coder/explore/plan subagents, hooks for risky tool calls, ACP editor integration.
- 검증 필요점: provider compatibility, plugin trust model, MCP auth handling, subagent isolation, approval bypass/yolo mode 안전성 검증 필요.
- 딥다이브 추천: Yes. terminal agent UX와 open-source harness 관점.

### Cline SDK / Cline runtime

Sources: [SDK Docs](https://docs.cline.bot/sdk/overview), [GitHub](https://github.com/cline/cline)

- 우선순위: Medium
- 왜 이슈인가: Cline이 IDE extension을 넘어 SDK, CLI, Kanban, scheduled agents, MCP, subagents를 같은 runtime으로 묶는다.
- 사람들이 하는 말/논쟁: coding agent가 plugin architecture와 product-embedded SDK로 넘어가는 신호. agent를 제품 안에 심는 플랫폼 관점에서 중요하다.
- 기술 핵심: `@cline/core`, `@cline/agents`, `@cline/llms`, scheduled agents, worktree-based Kanban, checkpointed edits.
- 검증 필요점: SDK stability, pricing/API key model, MCP permission, schedule audit, benchmark 재현성.
- 딥다이브 추천: Yes. Kimi/AX 이후 비교 딥다이브 권장.

### Task Observer

Sources: [GitHub](https://github.com/rebelytics/one-skill-to-rule-them-all), [r/claudeskills](https://www.reddit.com/r/claudeskills/)

- 우선순위: Medium
- 왜 이슈인가: skill을 자동으로 관찰, 개선 제안, 신규 skill 후보로 바꾸는 meta-skill. GitHub에서 661 stars로 확인되고 커뮤니티 확산이 있다.
- 사람들이 하는 말/논쟁: skills가 많아지는 문제에서 skills가 스스로 개선되는 문제로 논점이 이동. Hermes/OpenClaw 통합 경험도 커뮤니티에서 언급된다.
- 기술 핵심: work session observation log, correction/gap detection, skill updates proposal, cross-cutting principles log, human review before applying.
- 검증 필요점: self-improvement가 drift나 prompt injection을 키우지 않는지, observation log가 privacy와 IP 리스크를 만들지 않는지 확인 필요.
- 딥다이브 추천: Conditional. skill registry/eval 연구와 묶어 볼 가치 있음.

## 4. 신규 사항

| 중요도 | 주제 | 새로 확인한 내용 | 왜 중요한가 | AI 플랫폼 및 서비스 영향 |
|---|---|---|---|---|
| High | Copilot Agent Tasks REST API | [GitHub](https://github.blog/changelog/2026-06-04-agent-tasks-rest-api-now-available-for-copilot-pro-pro-and-max/) Copilot cloud agent task를 API로 시작/추적할 수 있는 public preview가 열림. | agent를 사람이 클릭하는 UI가 아니라 내부 시스템이 호출하는 background worker로 다룬다. | agent job API, task status API, artifact/PR output, budget, auth token scope를 공통화해야 한다. |
| High | Copilot App canvases / cloud automations | [GitHub](https://github.blog/changelog/2026-06-02-expanded-technical-preview-availability-for-the-github-copilot-app/) agent work가 canvas라는 bidirectional work surface로 이동. | agent 결과가 transcript에 묻히지 않고 work object에 남아야 한다는 방향. | reviewable canvas, action card, diff, approval, replay trace가 필요하다. |
| Medium | Copilot CLI scheduling / rubber duck / local voice | [GitHub](https://github.blog/changelog/2026-06-02-copilot-cli-improved-ui-rubber-duck-prompt-scheduling-and-voice-input/) `/every`, `/after`, reviewer-like rubber duck agent, local speech-to-text voice input 확인. | CLI agent가 recurring automation + critic agent + multimodal input으로 확장된다. | scheduled agent metadata와 traceable critic output이 필요하다. |
| Medium | Enterprise-managed plugins | [GitHub](https://github.blog/changelog/2026-06-05-enterprise-managed-plugins-in-vs-code-in-public-preview/) admins가 plugins, custom agents, skills, hooks, MCP configurations를 자동 배포할 수 있는 preview. | agent 확장 생태계가 개인 설정에서 enterprise governance 대상으로 이동. | plugin marketplace, allowlist, forced hooks, MCP policy, rollout/rollback이 필요하다. |
| Medium | Google Colab CLI | [Google Developers](https://developers.googleblog.com/introducing-the-google-colab-cli/) terminal에서 remote Colab runtime, GPU/TPU, remote exec, artifact download, REPL/console 제공. | agent가 local shell에서 remote accelerator를 tool처럼 호출하는 패턴이 강해진다. | remote compute broker, quota, artifact recovery, replayable notebook/log, data boundary 설계가 필요하다. |
| Medium | Codex Sites | [OpenAI Developers](https://developers.openai.com/codex/sites), [GeekNews](https://news.hada.io/) Codex가 Sites plugin으로 hosted website/web app/game을 만들고 저장/배포/inspect 가능. | agent가 코드 생성에서 배포와 접근 제어까지 이어지는 product shipping loop를 갖는다. | review-before-publish, audience control, secrets management, versioned deployment가 필요하다. |

## 5. 기존 조사 업데이트

| 기존 주제 | 변경점 | 기존 판단 | 후속 확인 |
|---|---|---|---|
| Hermes Agent | [r/Agent_AI](https://www.reddit.com/r/Agent_AI/), [r/hermesagent](https://www.reddit.com/r/hermesagent/) 커뮤니티에서 Hermes workflow, cron, Kanban, reusable skill 실험이 계속 보임. | persistent agent benchmark라는 판단 유지. 자동화는 quality 기준을 먼저 세운 뒤 반복해야 한다. | Hermes vs OpenClaw, Memory OS, gateway reliability 관련 공식/커뮤니티 자료 추적. |
| Dynamic Workflows | [Reddit thread](https://www.reddit.com/r/ClaudeAI/comments/1tq9ofy/introducing_dynamic_workflows_in_claude_code/), [Official blog](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code) 비용과 token burn, 자동 활성화 느낌, 결과 검증 가능성 논쟁 지속. | workflow runtime에는 cost cap, checkpoint, inspectable trace, verifier, human approval가 필요하다. | 실제 프로젝트 migration 사례의 재현성과 failure report 추적. |
| AgentBound / MCP permission | [GitHub plugins](https://github.blog/changelog/2026-06-05-enterprise-managed-plugins-in-vs-code-in-public-preview/), [OpenAI Agents SDK MCP](https://openai.github.io/openai-agents-python/mcp/) enterprise-managed plugins와 hosted/local MCP approval/filtering 문서가 같은 방향을 가리킨다. | MCP는 연결 표준이지만 운영 표준은 별도 permission layer가 필요하다. | tool filter, approval callback, connector token scope를 구현 기준으로 계속 비교. |
| Skills / SkillNet / SWE-Skills-Bench | [Task Observer](https://github.com/rebelytics/one-skill-to-rule-them-all), [claude-spellbook](https://github.com/kid-sid/claude-spellbook) skill marketplace와 skill auto-improvement 실험이 늘어남. | skill은 수량보다 measured utility, activation precision, lifecycle governance가 중요하다. | Task Observer를 skill eval/registry 모델과 연결해 별도 후보로 추적. |

## 6. Technical Detail

Sources: [AX](https://github.com/google/ax), [Cline SDK](https://docs.cline.bot/sdk/overview), [Kimi Code CLI](https://github.com/MoonshotAI/kimi-code), [OpenAI Agents SDK Changelog](https://openai.github.io/openai-agents-python/release/)

- Execution ledger: AX의 event log, Copilot Agent Tasks API의 task tracking, Cline Kanban의 worktree/task board는 agent run을 durable object로 취급한다.
- Isolation: AX는 actors/tools/skills isolation을, Copilot App은 worktree/branch/session isolation을, Cline Kanban은 card별 worktree를 강조한다.
- Tool governance: Kimi Code CLI는 lifecycle hooks와 conversational MCP config, OpenAI Agents SDK는 hosted/local MCP approval/filtering, GitHub는 enterprise plugin governance를 제공한다.
- Scheduling: Copilot CLI, Cline, Hermes 모두 scheduled run을 지원한다. 공통 위험은 stale intent, hidden cost, repeated side effect다.
- Review surface: Copilot canvases, Codex Sites review-before-deploy, Cline checkpoints는 agent work가 visible artifact로 검증되어야 한다는 방향이다.

## 7. AI Platform / Service Implications

| 영역 | 적용 포인트 | 필수 통제 |
|---|---|---|
| 공통 agent runtime | agent task를 API로 생성/추적하고, event log와 resumable state를 저장. | owner, scope, timeout, budget, checkpoint, replay, audit. |
| 서비스 agent UX | chat transcript 밖에 canvas/action card/diff/deployment candidate를 둔다. | 승인 전 상태와 승인 후 상태 분리, rollback, evidence of completion. |
| MCP / connector platform | tool catalog와 hosted/local connector를 제공하되 task별 filter를 적용. | manifest review, token scope, approval callback, sensitive result redaction. |
| Skill platform | skill creation, skill observation, skill update를 lifecycle로 관리. | quarantine, eval score, activation threshold, owner, provenance, version pin. |
| Cloud execution | Colab CLI 같은 remote compute를 agent tool로 연결. | quota, data movement, artifact recovery, environment reproducibility, cost guardrail. |
| Agent deployment | Codex Sites처럼 agent가 build/save/deploy/access-control까지 처리. | review-before-publish, environment secrets, audience control, deployment history. |

## 8. Recommended Actions

1. 지금: Google AX, Kimi Code CLI, Cline SDK를 별도 딥다이브 후보로 등록하고, runtime primitive 비교표를 만든다.
2. 지금: scheduled agent run의 최소 metadata를 정의한다: owner, prompt, toolset, skillset, channel, workdir, schedule, expiry, budget, last result.
3. 30일: internal agent job API prototype을 설계한다. 최소 기능은 create/run/status/cancel/replay/artifacts/audit.
4. 30일: skill registry에 quarantine/eval/activation/rollback 상태를 넣고 Task Observer류 self-improvement를 sandbox에서만 실험한다.
5. 계속: Copilot App canvases, Codex Sites, AG-UI/A2-UI를 비교해 agent-visible work object UX 기준을 만든다.

## 9. Risks / Watch Items

가장 큰 리스크: agent가 더 오래 실행되고 더 많은 도구를 호출할수록 failure mode가 prompt 품질이 아니라 runtime policy, credential scope, scheduled intent drift, hidden cost, artifact 검증 실패로 이동한다.

| 리스크 | 설명 | 대응 |
|---|---|---|
| Cost explosion | dynamic workflows와 scheduled tasks는 사용자 체감 없이 token/credit을 소모할 수 있다. | per-run budget, dry-run, approval threshold, usage alert. |
| Tool/skill bloat | MCP/skills/plugin이 많아질수록 wrong-tool, context noise, slow planning 가능성이 커진다. | task-scoped lazy loading, tool search, skill activation threshold. |
| Supply-chain risk | plugin marketplace, skill repo, MCP server bootstrap이 새로운 attack surface가 된다. | signed sources, install review, sandbox, forced enterprise policies. |
| State drift | memory/skills/scheduled jobs가 사용자의 최신 의도와 어긋날 수 있다. | expiration, periodic re-consent, human-readable run summary, rollback. |
| Cloud execution data boundary | remote GPU/hosted deployment/agent task API는 code/data/artifacts를 외부 runtime으로 보낸다. | data classification, provider policy, artifact retention, audit. |

## 10. Sources / Inline Evidence

주요 근거 링크는 각 요약과 판단 바로 아래 source pill로 배치했다. 이번 회차에서 신규 보조 소스로 추가한 항목은 `SOURCE_WATCHLIST.html`의 관련 보조 소스에 반영했다.

- [SOURCE_WATCHLIST](../../SOURCE_WATCHLIST.html)
- [Source Catalog](../../index.html#references)
- [HTML](../0900_agent_trend_brief.html)

## 출처

### 왜 이걸 정리하게 되었는가

- [Source Watchlist](../SOURCE_WATCHLIST.html)
- [GeekNews](https://news.hada.io/)
- [MarkTechPost](https://www.marktechpost.com/)
- [GitHub Blog](https://github.blog/)
- [r/Agent_AI](https://www.reddit.com/r/Agent_AI/)
- [r/hermesagent](https://www.reddit.com/r/hermesagent/)

### 딥리서치 출처

- [Google AX](https://github.com/google/ax)
- [Kimi Code CLI](https://github.com/MoonshotAI/kimi-code)
- [Cline SDK](https://docs.cline.bot/sdk/overview)
- [Agent Tasks API](https://github.blog/changelog/2026-06-04-agent-tasks-rest-api-now-available-for-copilot-pro-pro-and-max/)
- [Google Colab CLI](https://developers.googleblog.com/introducing-the-google-colab-cli/)
- [Codex Sites](https://developers.openai.com/codex/sites)
- [OpenAI Agents SDK Changelog](https://openai.github.io/openai-agents-python/release/)
