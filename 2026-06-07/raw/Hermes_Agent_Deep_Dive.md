# Hermes Agent Deep Dive

- 작성 시점: 2026-06-07 01:12 KST
- 조사 수준: Detailed Deep Dive
- 유형: Project / Persistent agent runtime / product benchmark
- HTML 정본: `2026-06-07/Hermes_Agent_Deep_Dive.html`

## 1. Executive Summary

참고 링크: [Hermes Docs](https://hermes-agent.nousresearch.com/docs/), [GitHub](https://github.com/NousResearch/hermes-agent), [Nous Product Page](https://nousresearch.com/hermes-agent/), [Architecture Docs](https://hermes-agent.nousresearch.com/docs/developer-guide/architecture/)

Hermes Agent는 Nous Research가 만든 open-source persistent agent framework다. 단발성 coding assistant라기보다 memory, skills, tool runtime, messaging gateway, cron, MCP, profile isolation, provider routing을 한데 묶은 개인용 agent runtime benchmark로 봐야 한다.

핵심 판단:

- Hermes Agent의 중요성은 특정 모델 성능이나 단일 기능이 아니라, agent가 장기간 기억하고, 스스로 절차를 저장하고, 여러 채널에서 응답하고, 사용자가 없는 시간에도 예약 실행되는 “persistent agent” 제품 패턴을 빠르게 압축해서 보여준다는 데 있다.
- 그대로 채택하기보다 architecture, permission matrix, skill lifecycle, scheduled autonomy, memory governance를 벤치마크해 자체 플랫폼 요구사항으로 바꾸는 것이 현실적이다.
- persistent memory, broad tool execution, MCP catalog, credential handling, channel gateway, cron이 결합되면 attack surface도 커진다. 운영형 서비스에서는 sandbox, allowlist, channel-specific approval, data retention, audit trace를 먼저 설계해야 한다.

## 2. Latest Signal and Momentum

참고 링크: [GitHub Repository](https://github.com/NousResearch/hermes-agent), [GitHub Releases](https://github.com/NousResearch/hermes-agent/releases), [Docs Landing](https://hermes-agent.nousresearch.com/docs/), [Tool Gateway](https://hermes-agent.nousresearch.com/docs/user-guide/features/tool-gateway/)

| 관찰 신호 | 확인 내용 | 해석 |
| --- | --- | --- |
| Release cadence | GitHub releases 영역에는 2026-06-06 기준 v0.16.0 “Surface Release”가 latest로 노출된다. | core loop만이 아니라 desktop app, web dashboard, TUI, CLI, model picker, onboarding 등 surface 경쟁으로 이동한다. |
| Feature density | 공식 docs는 20+ messaging platforms, 60+ built-in tools, 70+ tool registry, 28 toolsets, 18+ providers, 6 terminal backends를 설명한다. | agent platform은 “모델 + prompt”가 아니라 tool runtime, connector catalog, platform adapters, provider runtime의 조합이 된다. |
| Managed tools | Tool Gateway는 web search, image generation, TTS, cloud browser automation을 subscription 기반으로 묶는다. | 서비스 사업자는 agent가 쓸 tool billing, rate limit, credential setup을 제품화 포인트로 볼 필요가 있다. |
| Community debate | Reddit에서는 tool/skill bloat, cron의 full-agent parity, gateway reliability, profile별 tool pruning이 반복 논점이다. | 많은 기능을 붙이는 것보다 task-scoped capability selection과 운영 안정성이 더 큰 차별점이 된다. |

## 3. What It Is

참고 링크: [README Raw](https://raw.githubusercontent.com/NousResearch/hermes-agent/main/README.md), [Tools Docs](https://hermes-agent.nousresearch.com/docs/user-guide/features/tools), [Profiles Docs](https://hermes-agent.nousresearch.com/docs/user-guide/profiles/), [Memory Docs](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory/)

Hermes Agent는 terminal, CLI/TUI, messaging gateway, desktop/web dashboard, ACP editor integration, batch runner, API server 같은 여러 entry point에서 같은 core agent loop를 호출하는 구조다. core loop는 prompt builder, provider resolution, tool dispatch, session persistence, memory, skills, context compression, callbacks를 묶는다.

공식 문서에서 가장 중요한 표현은 “closed learning loop”다. Hermes는 agent-curated memory, skill creation, skill self-improvement, session search, profile state, cron delivery를 통해 한 번의 대화가 다음 실행으로 이어지도록 설계되어 있다.

| 기능 묶음 | Hermes에서 보이는 형태 | AI 플랫폼 및 서비스 관점 |
| --- | --- | --- |
| Persistent Memory | `MEMORY.md`, `USER.md`, SQLite session search, external memory providers | 개인화와 privacy boundary를 동시에 다루는 memory control plane이 필요하다 |
| Skills System | agent-created procedural memory, slash command skills, bundles, Skills Hub, external skill dirs | skill registry, provenance, eval, versioning, quarantine, activation policy가 핵심이다 |
| Gateway | Telegram, Discord, Slack, WhatsApp, Signal, Matrix, Email, SMS, Teams 등 adapter | 채널별 identity, approval, action scope, delivery policy를 분리해야 한다 |
| MCP | stdio/HTTP MCP, catalog install, tool include/exclude filtering, OAuth token cache | connector admission, tool allowlist, credential scoping, audit trace가 필요하다 |
| Cron / Event Hooks | natural language or cron expression, fresh agent sessions, skill attachment, platform delivery | 사용자 없는 실행에는 owner, expiry, budget, last-run summary, kill switch가 기본값이어야 한다 |
| Provider Runtime | OpenAI-compatible endpoint, OpenRouter, Nous Portal, many providers, fallback/provider resolution | routing은 cost만이 아니라 data policy, latency, fallback, compliance 기준으로 관리해야 한다 |

## 4. Architecture Decomposition

참고 링크: [Architecture Docs](https://hermes-agent.nousresearch.com/docs/developer-guide/architecture/), [Tools & Toolsets](https://hermes-agent.nousresearch.com/docs/user-guide/features/tools), [MCP Docs](https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp/), [Security Docs](https://hermes-agent.nousresearch.com/docs/user-guide/security/)

Hermes architecture docs의 핵심은 하나의 `AIAgent` core가 CLI, gateway, cron, ACP, batch, API server에서 재사용된다는 점이다. platform-specific logic은 entry point와 adapter에 두고, prompt assembly, provider runtime, tool registry, session persistence는 공유한다.

| 레이어 | 공식 문서 기반 확인 | 설계상 의미 |
| --- | --- | --- |
| Entry Point | CLI, Gateway, ACP, Batch Runner, API Server, Python Library | agent product는 하나의 chat UI가 아니라 여러 surface가 같은 agent state를 공유하는 구조로 간다 |
| Agent Loop | `run_agent.py`의 AIAgent가 provider selection, prompt construction, tool execution, retries, callbacks, compression, persistence를 처리 | core loop에는 observability, retry budget, permission decision, failure recovery hook이 들어가야 한다 |
| Prompt System | stable/context/volatile tiers, skills, context files, memory/profile/timestamp blocks, prompt caching | memory와 skills를 언제 system prompt에 넣고 언제 user message로 넣을지 정책화해야 한다 |
| Tool Registry | central registry, toolsets, platform presets, terminal/file/browser/web/media/delegation/memory/cron/MCP | tool description bloat를 줄이려면 registry 위에 selector/gateway layer가 필요하다 |
| Session Storage | SQLite + FTS5, session lineage, per-platform isolation, atomic writes | 대화 기록은 단순 transcript가 아니라 recall, audit, eval, compliance 자료가 된다 |
| Gateway | 20 platform adapters, authorization, slash commands, hooks, cron ticking, delivery | messaging app adapter는 conversational ingress와 action delivery를 모두 책임지므로 보안 경계가 두껍다 |
| Plugin / Memory Providers | user/project/pip entry-point plugins, memory provider plugin, context engine plugin | extensibility는 marketplace가 되기 쉽고, marketplace는 검증/서명/권한/삭제 UX가 필요하다 |
| Cron | JSON job store, scheduler, fresh AIAgent sessions, attached skills, platform delivery | scheduled agent run은 background worker이며, 일반 chat session보다 강한 owner/scope/expiration 제어가 필요하다 |

## 5. Memory, Profile, and State Model

참고 링크: [Persistent Memory](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory/), [Profiles](https://hermes-agent.nousresearch.com/docs/user-guide/profiles/), [Architecture](https://hermes-agent.nousresearch.com/docs/developer-guide/architecture/)

Hermes는 “agent state”를 여러 층으로 나눈다. memory는 prompt에 반복 주입되는 compact facts, session search는 SQLite/FTS5 기반 과거 대화 조회, profile은 config/API keys/SOUL/memories/sessions/skills/cron/state DB/gateway PID/logs를 분리하는 home directory다.

| State 종류 | Hermes 구현 신호 | 운영 설계 포인트 |
| --- | --- | --- |
| Memory facts | `MEMORY.md`와 `USER.md`가 session start에 frozen snapshot으로 system prompt에 들어감 | memory write는 즉시 저장되더라도 다음 session에 보인다는 cache-aware 모델을 UI에 설명해야 한다 |
| Session history | `~/.hermes/state.db`에 CLI/messaging session 저장, FTS5 검색, scroll/browse | 장기 recall은 on-demand search로 처리하고, 항상 prompt에 넣는 memory는 작고 검증된 사실로 제한한다 |
| User profile | preferences, communication style, timezone, workflow habits 등을 별도 target으로 저장 | 사용자 통제권이 필요하다. approve/reject/edit/export/delete UI가 없으면 personalization이 신뢰 리스크가 된다 |
| Agent profile | profile별 `HERMES_HOME`, config, API keys, memory, sessions, skills, cron, gateway state 분리 | researcher, coder, support, commerce 등 role별 agent를 만들 때 state mixing을 막는 기본 단위가 된다 |
| Working directory | profile home과 terminal cwd가 다르며, local backend는 launch directory 또는 config cwd 기준 | profile isolation은 sandbox isolation이 아니다. filesystem 권한과 cwd policy는 별도 제어해야 한다 |
| Distributable profile | SOUL, config, skills, cron, MCP connections를 package로 공유하고 credentials/memories/sessions는 per-machine 유지 | team agent template과 user-private state를 분리하는 좋은 패턴이다 |

## 6. Skills as Procedural Memory

참고 링크: [Skills System](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills), [Skills Catalog](https://hermes-agent.nousresearch.com/docs/reference/skills-catalog/), [agentskills.io](https://agentskills.io/), [Skills Loading Thread](https://www.reddit.com/r/hermesagent/comments/1tp0by4/does_hermes_actually_load_the_skills_every_single/)

Hermes의 skills는 반복 workflow를 저장하는 procedural memory다. agent가 복잡한 task를 성공하거나, 에러를 해결하거나, 사용자의 교정을 받으면 skill을 만들 수 있고, skill bundles는 여러 skill을 하나의 slash command로 묶는다.

| 기능 | 좋은 점 | 리스크 | 설계 기준 |
| --- | --- | --- | --- |
| Agent-managed skill creation | 작업 경험이 다음 실행 품질로 이어진다 | 검증되지 않은 절차가 반복 주입될 수 있다 | quarantine, provenance, eval pass, human approval 후 활성화 |
| Skill patch/edit/delete | 절차가 고정 문서가 아니라 계속 개선된다 | 외부 skill dir이 writable이면 공유 skill도 바뀔 수 있다 | filesystem permission, signed registry, team skill read-only mode |
| Bundles | 반복 task profile을 짧은 command로 호출한다 | bundle이 커지면 prompt/context overhead가 늘어난다 | bundle마다 max token budget, task class, activation threshold 지정 |
| Skills Hub | community/official skills를 빠르게 설치한다 | marketplace supply-chain risk와 prompt injection risk가 있다 | manifest scanning, setup command review, registry trust tier, rollback |
| Slash command surface | user가 특정 procedure를 명시적으로 호출할 수 있다 | 자동 활성화와 수동 호출이 섞이면 behavior 설명 가능성이 낮아진다 | auto vs explicit activation을 UI와 trace에 구분 표시 |

커뮤니티 논점: skills가 너무 많이 자동 로드되면 system prompt overhead와 tool choice noise가 커진다는 지적이 반복된다. 장기적으로는 full skill text가 아니라 compact index, search/describe/invoke, task-scoped lazy loading이 필요하다.

## 7. Gateway, MCP, Cron, and Automation Surface

참고 링크: [MCP Docs](https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp/), [Cron Docs](https://hermes-agent.nousresearch.com/docs/user-guide/features/cron/), [Tool Gateway](https://hermes-agent.nousresearch.com/docs/user-guide/features/tool-gateway/), [Gateway Internals](https://hermes-agent.nousresearch.com/docs/developer-guide/architecture/)

MCP와 cron은 Hermes가 일반 assistant에서 agent runtime으로 넘어가는 핵심 지점이다. MCP는 외부 tool ecosystem을 붙이고, cron은 user prompt가 없는 시간에도 agent run을 시작한다. messaging gateway는 이 실행 결과를 여러 채널로 전달한다.

| Surface | Hermes 신호 | 권한/운영 요구사항 |
| --- | --- | --- |
| MCP catalog | curated optional MCP catalog, install-time credential setup, tool checklist, per-server filtering | catalog admission review, source repository verification, bootstrap command inspection, default-deny mutating tools |
| MCP runtime | startup discovery, prefixed tool names, resource/prompt utility tools, dynamic tool list notification handling | capability-aware registration, tool diff audit, runtime reload trace, dangerous tool confirmation |
| MCP credential handling | stdio env filtering, explicit env only, OAuth token cache with file permissions | least-privilege tokens, per-profile credentials, token rotation, error redaction, server-scoped audit |
| Cron jobs | one-shot/recurring tasks, pause/resume/edit/run/remove, skill attachment, origin/local/platform delivery | owner, schedule, workdir, attached skills, toolsets, delivery target, budget, expiry, last-run outcome를 모두 저장 |
| Cron runtime | gateway daemon ticks every 60 seconds, loads `jobs.json`, fresh AIAgent sessions, file lock prevents overlapping ticks | idempotency key, overlapping run policy, retries, failure notification, run trace retention이 필요하다 |
| Messaging delivery | origin/local/Telegram/Discord/Slack/Email/SMS/Home Assistant/all 등 delivery targets | 채널별 home target, fan-out policy, sensitive result redaction, delivery failure handling |

## 8. Security and Trust Reading

참고 링크: [Official Security Docs](https://hermes-agent.nousresearch.com/docs/user-guide/security/), [MCP Security Model](https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp/), [CSA Research Note](https://labs.cloudsecurityalliance.org/research/csa-research-note-hermes-agent-cves-20260504-csa-styled/), [Agent Security Systems Paper](https://arxiv.org/abs/2605.18991)

공식 security docs는 command approval, sandbox/env filtering, MCP credential handling, website blocklist, SSRF protection, pre-exec scanning을 다룬다. 외부 security note는 Hermes와 유사 persistent agent frameworks의 CVE/architecture risk를 근거로 memory isolation, skill manifest validation, sandbox enforcement, prompt-layer monitoring을 강조한다.

| 위험면 | 실패 경로 | 필수 통제 |
| --- | --- | --- |
| Broad shell/file access | prompt injection이나 오인 task가 host credential/file에 접근 | Docker/remote sandbox 기본값, write-safe root, command approval, credential file read-only mount |
| MCP supply chain | catalog manifest나 bootstrap command가 악성 dependency를 설치 | manifest review, signed sources, install command diff, tool allowlist, server disable switch |
| Skill injection | skill description/setup/instruction에 prompt injection이나 exfiltration 절차가 들어감 | Skills Guard, registry trust tier, setup command blocking, manual approval, provenance trace |
| Memory poisoning | 악성 또는 잘못된 memory가 매 session system prompt에 반복 주입 | memory scanning, approval inbox, conflict detection, retention limit, rollback |
| Messaging gateway takeover | 외부 채널 auth 약점이나 DM pairing 오용으로 privileged action이 실행 | allowlist, channel-scoped toolsets, sensitive action confirmation, session/device binding |
| Cron autonomy | 오래된 schedule이 계속 실행되거나, scope 밖 action을 반복 | expiry, dry-run, pause switch, run budget, owner notification, periodic re-consent |
| Web/browser SSRF | agent가 internal URL/cloud metadata/private network를 fetch | private network block, URL revalidation on redirect, public gateway에서 private URL opt-out 금지 |

시스템 보안 해석: 모델은 신뢰할 수 없는 decision component로 보고, permission, sandbox, credential scope, action approval, audit invariant는 runtime이 강제해야 한다. prompt instruction만으로는 persistent agent의 보안 기준이 될 수 없다.

## 9. Community Risk Signals

참고 링크: [Tool/Skills Bloat](https://www.reddit.com/r/hermesagent/comments/1t34qee/hermes_agent_tool_and_skills_bloat/), [Skills Loading](https://www.reddit.com/r/hermesagent/comments/1tp0by4/does_hermes_actually_load_the_skills_every_single/), [Cron Toolset](https://www.reddit.com/r/hermesagent/comments/1sqdt6b/cron_jobs_dont_have_full_toolset_and_skills/), [Self-improving Cron](https://www.reddit.com/r/hermesagent/comments/1txeg1n/self_improving_cron_skill_via_software_house/), [Agent AI Skills Thread](https://www.reddit.com/r/Agent_AI/comments/1tq4oj7/how_im_thinking_about_hermes_agent_skills/)

아래는 공식 출처로 확인된 기능 사실과 별개로, 커뮤니티에서 반복되는 사용감/운영 리스크 신호다. 제품 판단에는 유용하지만, 수치와 개별 경험담은 검증되지 않은 신호로 취급한다.

| 논점 | 사람들이 말하는 문제 | 왜 트렌디한가 | 후속 확인 |
| --- | --- | --- | --- |
| Tool/skill bloat | 많은 tool schema와 skill text가 first turn context를 잠식하고 tool selection noise를 만든다는 지적 | MCP/skills를 많이 붙인 agent일수록 capability routing이 core UX가 된다 | schema token overhead, wrong-tool rate, lazy loading 효과를 자체 bench로 측정 |
| Cron parity | cron run이 interactive agent와 같은 tool/skill context를 갖는지 혼란이 있다 | 무인 실행 agent가 실제 업무 자동화로 쓰이려면 deterministic context가 필요하다 | cron job별 toolset/skill/workdir/config snapshot을 명시하는 UI 설계 |
| Self-improving skills | 야간 cron으로 skills를 review/trim/archive하게 만드는 실험이 공유됨 | agent가 자신의 capability repository를 유지보수하는 흐름이 등장하고 있다 | self-edit에는 approval gate와 regression eval을 붙이지 않으면 drift가 생긴다 |
| Profile setup | profile별 tool/skill config를 따로 pruning하지 않으면 같은 bloat가 반복된다는 의견 | multi-agent profiles가 늘수록 role-specific configuration UX가 중요해진다 | profile template, inherited config diff, per-role tool policy를 설계 |
| Gateway reliability | 메시징 gateway, cron, profile이 섞일 때 reproducibility가 떨어진다는 사용 경험이 있다 | agent는 chat product가 아니라 background service가 되면서 ops 품질이 경쟁력이 된다 | daemon health, delivery retry, scheduler trace, run replay를 요구사항화 |

## 10. AI 플랫폼 및 서비스 적용 방향

참고 링크: [Tools Docs](https://hermes-agent.nousresearch.com/docs/user-guide/features/tools), [Profiles Docs](https://hermes-agent.nousresearch.com/docs/user-guide/profiles/), [Cron Docs](https://hermes-agent.nousresearch.com/docs/user-guide/features/cron/), [MCP Docs](https://hermes-agent.nousresearch.com/docs/user-guide/features/mcp/)

가져갈 것:

- profile-based state isolation
- memory/session search split
- explicit skill lifecycle
- channel gateway
- scheduled work lifecycle
- MCP per-server filtering
- managed tool gateway billing model

그대로 가져오면 안 되는 것:

- 모든 채널에 동일 tool 권한
- 자동 skill 활성화
- 검증 없는 self-improvement
- 영구 schedule
- hidden memory write
- provider routing without data policy

| 적용 영역 | Hermes에서 배울 점 | 제품 요구사항 |
| --- | --- | --- |
| 개인 agent / 업무 agent | profile 단위로 config, memory, skills, sessions, cron을 나눔 | role별 agent template, private state, enterprise policy, export/delete controls |
| 대화형 서비스 | messaging gateway로 channel surface를 확장 | 채널별 action card, approval card, sensitive result masking, fan-out policy |
| 커머스/예약/결제 workflow | cron, webhook, tool delivery를 통해 agent가 사용자 대신 흐름을 계속 진행 | mandate, expiry, human-present/not-present 구분, dispute evidence, final confirmation |
| 사내/파트너 connector | MCP catalog와 per-server filtering | connector manifest, allowed operations, read/write tier, token scope, audit log |
| 스킬 플랫폼 | skills as procedural memory, bundles, hub, self-improvement | skill registry, eval score, version compatibility, owner, provenance, rollback |
| 운영/관측 | session DB, cron state, gateway daemon, tool trace | run ledger, tool call trace, delivery log, policy decision log, replayable failure report |

## 11. Evaluation and PoC Plan

참고 링크: [Architecture Docs](https://hermes-agent.nousresearch.com/docs/developer-guide/architecture/), [Memory Docs](https://hermes-agent.nousresearch.com/docs/user-guide/features/memory/), [Skills Docs](https://hermes-agent.nousresearch.com/docs/user-guide/features/skills), [Cron Docs](https://hermes-agent.nousresearch.com/docs/user-guide/features/cron/)

Hermes를 benchmark로 볼 때 평가는 기능 수가 아니라 “persistent agent가 실제 서비스 환경에서 안전하고 예측 가능하게 일하는가”를 측정해야 한다.

| PoC task | 검증 질문 | 측정 지표 |
| --- | --- | --- |
| Recurring research digest | cron이 owner/scope/expiry를 지키며 매 실행 결과를 추적 가능한가 | missed run rate, duplicate run rate, cost/run, source freshness, notification latency |
| Cross-channel approval | messaging에서 요청한 action이 web/desktop 승인 UI와 일관되게 연결되는가 | approval completion rate, wrong-channel block rate, sensitive action false negative |
| Memory correction | 잘못된 memory가 수정/삭제/rollback되고 다음 session에 반영되는가 | memory precision, correction latency, stale memory recurrence, user trust score |
| Skill lifecycle | 새 skill 생성 후 quarantine, eval, activation, rollback이 가능한가 | skill utility delta, token overhead, wrong activation rate, regression count |
| MCP connector policy | read/write tool filtering과 credential scope가 runtime에서 강제되는가 | blocked dangerous calls, policy decision coverage, connector audit completeness |
| Failure recovery | tool failure, provider failure, delivery failure 후 trace와 다음 조치가 명확한가 | recovery success, retry budget adherence, user-visible explanation quality |

2주 PoC 제안:

- 1주차: memory/session/search, skill registry, MCP allowlist, cron owner/scope schema 구현.
- 2주차: recurring digest, cross-channel approval, memory correction, connector policy, skill quarantine 시나리오를 실제 user journey로 검증.

## 12. Recommended Actions

1. Hermes를 기능 비교표가 아니라 persistent agent architecture checklist로 벤치마크한다.
2. memory, skill, tool, channel, scheduler, provider를 하나의 permission matrix로 묶어 설계한다.
3. 메신저/웹/desktop/channel별로 승인 UI와 tool 권한을 분리한다.
4. tool/skill bloat 방지를 위해 task intent classification, lazy schema delivery, search/describe/invoke router를 실험한다.
5. cron/webhook agent run에는 owner, scope, workdir, attached skills, delivery target, expiry, budget cap, last-run summary, disable switch를 둔다.
6. agent-created skills는 quarantine에서 시작하고, eval과 human approval 이후 활성화한다.
7. MCP connector catalog에는 manifest review, bootstrap command inspection, per-server allowlist, token scope, runtime audit를 기본값으로 둔다.
8. memory write에는 security scanning, approval inbox, conflict detection, retention policy, rollback을 적용한다.
9. provider routing은 data sensitivity, retention, cost, latency, fallback, failure mode를 기준으로 정책화한다.
10. Hermes의 latest surface 강화 신호를 추적하면서 desktop/web dashboard/agent status UX를 별도 benchmark로 남긴다.
