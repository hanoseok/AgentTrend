# Google Agent Executor / AX Deep Dive

- 작성 시각: 2026-06-07 11:17 KST
- HTML 정본: `report/google-ax-deep-dive/`
- 조사 수준: Detailed Project Deep Dive
- 유형: Agent runtime / control plane / Kubernetes execution substrate

## 1. Executive Summary

참고 링크: [AX Site](https://agentexecutor.io/), [GitHub](https://github.com/google/ax), [v0.1.0 release](https://github.com/google/ax/releases/tag/v0.1.0), [README Raw](https://raw.githubusercontent.com/google/ax/main/README.md), [ax.proto](https://raw.githubusercontent.com/google/ax/main/proto/ax.proto)

Google Agent Executor, 줄여서 AX는 Google이 공개한 open-source distributed agent runtime이다. 핵심은 agent를 단순 chat loop가 아니라 durable execution, event log, resumable stream, isolated actor, registry, Kubernetes deployment unit으로 다루는 데 있다.

핵심 판단:

- AX는 지금 당장 stable platform으로 채택할 제품이라기보다, 앞으로 AI 플랫폼 및 서비스가 갖춰야 할 agent runtime primitive를 선명하게 보여주는 reference signal이다.
- 가장 중요한 primitive는 durable event log, conversation-level sequence, pending execution resume, forkable trajectory, registry-based agent invocation, Kubernetes actor lifecycle이다.
- production core로 바로 넣기에는 이르다. README와 proto, open issues가 모두 interface churn을 보여준다.
- 다만 internal agent job API, event ledger, approval stream, fork/replay, sandbox lifecycle, connector permission model의 요구사항을 정의하는 benchmark로는 즉시 활용할 가치가 높다.

## 2. Source Coverage

| 소스 | 확인 내용 | 신뢰도 | 해석 |
| --- | --- | --- | --- |
| [AX official site](https://agentexecutor.io/) | reliability, safety, customizability, efficiency, Kubernetes-native, model/harness agnostic, MCP/A2A support, auditing/policy를 핵심 메시지로 제시 | 1차 | 제품 포지셔닝은 "distributed runtime under the agent"다 |
| [GitHub README](https://github.com/google/ax), [README Raw](https://raw.githubusercontent.com/google/ax/main/README.md) | active early development, breaking changes, external PR pause, single-writer/event log/resumption, custom remote/A2A/ADK/Colab agents, roadmap 확인 | 1차 | 안정성보다 설계 검증 단계다. 문서의 promise와 code surface를 분리해야 한다 |
| [ax.proto](https://raw.githubusercontent.com/google/ax/main/proto/ax.proto), [controller.go](https://raw.githubusercontent.com/google/ax/main/internal/controller/controller.go), [eventlog.go](https://raw.githubusercontent.com/google/ax/main/internal/controller/executor/eventlog.go), [sqlite.go](https://raw.githubusercontent.com/google/ax/main/internal/controller/executor/sqlite.go) | ControllerService, ConversationService, AgentService, HarnessService, SQLite event log, pending/completed state, last_seq replay, confirmation handling 확인 | 1차 | runtime primitive는 코드로 존재한다. 단, TODO와 open issue가 많다 |
| [v0.1.0](https://github.com/google/ax/releases/tag/v0.1.0), [Issue #50](https://github.com/google/ax/issues/50), [Issue #27](https://github.com/google/ax/issues/27), [Issue #13](https://github.com/google/ax/issues/13) | initial release, event log update, transport decision, OpenTelemetry, trajectory exposition, Exec RPC revisit가 open | 1차 | control plane API가 아직 변하는 중이다 |
| [Agent Substrate](https://github.com/agent-substrate/substrate), [Go Packages](https://pkg.go.dev/github.com/agent-substrate/substrate), [GKE Agent Sandbox](https://docs.cloud.google.com/kubernetes-engine/docs/how-to/agent-sandbox), [Kubernetes Blog](https://kubernetes.io/blog/2026/03/20/running-agents-on-kubernetes-with-agent-sandbox/) | actor-to-worker multiplexing, suspend/resume, warm pool, SandboxTemplate/SandboxWarmPool, gVisor/Kata isolation, stable identity 확인 | 1차/공식 | AX의 production story는 Kubernetes substrate와 함께 봐야 한다 |
| [r/LLMDevs comparison](https://www.reddit.com/r/LLMDevs/comments/1tukc23/%D1%81ompared_agent_platforms_cloudflare_agents_aws/), [r/AI_Agents Agyn thread](https://www.reddit.com/r/AI_Agents/comments/1tnmp48/agyn_opensource_distributed_agent_runtime_on/) | 커뮤니티는 durable execution, A2A interop, self-hosted runtime 장점을 언급하면서 preview status, credential isolation, MCP runtime 실체를 논점으로 둔다 | 커뮤니티 신호 | 미확인 신호로 분리하되 security/credential boundary 논쟁은 PoC 체크리스트에 반영한다 |

## 3. What AX Is and Is Not

참고 링크: [README: What AX is NOT](https://github.com/google/ax#what-ax-is-not), [Initial Release](https://github.com/google/ax/releases/tag/v0.1.0)

| 구분 | AX의 공식 포지션 | 실무 해석 |
| --- | --- | --- |
| Runtime | agentic loop를 coordinate하고 local/remote actors와 통신하는 distributed runtime | LangGraph 같은 graph framework보다 아래 레이어다. "agent가 어디서, 어떤 상태로, 어떻게 재개되는가"를 담당한다 |
| Control plane | single controller가 user/agentic calls, skill/tool/agent calls를 조정하고 audit 가능하게 만든다 | agent 실행 ledger와 policy enforcement point를 한 곳에 둔다는 뜻이다 |
| Self-hosted | managed service가 아니며 Kubernetes clusters에서 배포/운영하기 쉽게 만드는 것이 목표 | 운영 책임은 도입 조직에 있다. storage, secrets, network policy, observability, authn/authz를 직접 설계해야 한다 |
| Framework-agnostic | agent framework가 아니며 ADK 같은 framework와 integration을 제공하려 한다 | agent logic은 외부 harness/agent가 갖고, AX는 serving/resumption/isolation layer를 제공하는 모델이다 |
| Harness-agnostic | 특정 coding agent harness가 아니며, BYOH와 built-in harness를 roadmap으로 제시 | 현재는 Gemini planner와 일부 examples가 중심이다. production coding-agent harness는 직접 연결해야 한다 |
| Model-agnostic | model-specific controller가 아니라고 명시 | 현재 config와 planner default는 Gemini 중심이다. OpenAI-compatible endpoint는 issue로 열려 있다 |

## 4. Architecture: Controller, Registry, Event Log, Actor

참고 링크: [README Architecture](https://raw.githubusercontent.com/google/ax/main/README.md), [controller.go](https://raw.githubusercontent.com/google/ax/main/internal/controller/controller.go), [registry.go](https://raw.githubusercontent.com/google/ax/main/internal/controller/registry.go), [config.go](https://raw.githubusercontent.com/google/ax/main/internal/config/config.go)

AX의 public diagram은 Client, Router, AX Controller, Remote Agent, Tool(MCP server), Environment with skills/tools를 연결한다. code 기준 핵심 객체는 `Controller`, `Registry`, `EventLog`, `Agent`이며 controller가 registry를 clone해서 planner와 Gemini agent를 더한 뒤 실행한다.

| 구성요소 | 코드/문서에서 확인된 역할 | AI 플랫폼 설계로 번역 |
| --- | --- | --- |
| Client | `ax exec`는 local built-in controller 또는 remote gRPC server에 `ExecRequest`를 보낸다 | 서비스 UI/업무 시스템은 agent job client가 되고 run id/conversation id/last seen seq를 관리해야 한다 |
| Router | README diagram과 Kubernetes deployment에서 router가 resumable stream을 controller/actor로 보낸다 | channel별 ingress는 stateless gateway가 아니라 conversation-aware router가 필요하다 |
| Controller | single-writer orchestrator. pending execution을 찾고 history를 replay하고 new execution을 event log에 append한다 | central execution authority가 있어야 audit, replay, policy, concurrency control이 가능하다 |
| Registry | local, remote AX protocol, A2A protocol, Colab, Substrate agents를 등록한다 | registry는 protocol, auth, metadata, health, capability, trust tier를 가져야 한다 |
| Event Log | conversation log와 execution log가 SQLite에 append된다. conversation seq는 per conversation monotonic하게 계산된다 | agent output은 transcript가 아니라 ordered, durable, typed event stream이어야 한다 |
| Remote Agent | native `AgentService.Connect`, A2A bridge, ADK Python agent, Colab agent examples가 있다 | runtime은 특정 framework가 아니라 protocol boundary로 외부 agent를 품어야 한다 |
| Environment / Tool | README는 skills/tools/MCP server를 actor로 표시하지만 repo 검색 기준 MCP runtime 구현은 제한적이다 | MCP support는 connector runtime, permission, auth, audit까지 구현해야 실제 platform feature가 된다 |

## 5. Execution, Resumption, Fork, Trace

참고 링크: [exec.go](https://raw.githubusercontent.com/google/ax/main/cmd/ax/exec.go), [controller.go](https://raw.githubusercontent.com/google/ax/main/internal/controller/controller.go), [executor.go](https://raw.githubusercontent.com/google/ax/main/internal/controller/executor/executor.go), [sqlite.go](https://raw.githubusercontent.com/google/ax/main/internal/controller/executor/sqlite.go), [trace.go](https://raw.githubusercontent.com/google/ax/main/cmd/ax/trace.go)

AX의 가장 중요한 기술 단위는 event log 기반 resumption이다. `ExecRequest`에는 `conversation_id`, `inputs`, `last_seq`, `agent_id`, `agent_config`가 들어가고, `ExecResponse`는 output messages와 seq를 stream으로 반환한다.

| 기능 | 동작 방식 | 강점 | 한계 / open point |
| --- | --- | --- | --- |
| Conversation continuation | conversation id가 있으면 기존 events를 읽고 history를 재구성한다 | long-running 작업의 continuity를 conversation id에 묶을 수 있다 | conversation은 마지막 execution이 완료/실패되기 전에는 이어가기 어렵다는 proto comment가 있다 |
| last_seq backfill | client가 마지막으로 본 seq를 넘기면 이후 conversation events를 다시 stream한다 | client disconnect 후 놓친 output을 catch-up할 수 있다 | [Issue #7](https://github.com/google/ax/issues/7)은 현재 backfill만으로는 in-flight 상황에서 충분하지 않다고 지적한다 |
| Pending execution resume | conversation event 중 pending exec id를 찾고 execution log의 pending event를 기준으로 `execute`를 다시 호출한다 | agent가 중단된 뒤 state를 event log로 재구성하려는 구조다 | side effect idempotency, tool replay, external API 중복 호출은 별도 설계가 필요하다 |
| Confirmation | 마지막 message가 confirmation question이면 user approval/decline input이 올 때까지 pending 상태로 둔다 | human-in-the-loop primitive가 content model에 포함된다 | subagent tool approval은 roadmap이며 현재 approval policy language는 얇다 |
| Fork | source conversation과 seq를 지정해 새 conversation log를 만든다 | trajectory branching, alternative plan/eval, rollback test에 유용하다 | proto TODO는 snapshot_id 전환을 언급한다 |
| Trace | SQLite event log에서 conversation/execution events를 읽어 local web UI로 시각화한다 | debugging과 audit의 최소 기준을 제공한다 | trace.go TODO는 graph execution 표시가 부정확하고 executor가 linear model로 바뀔 예정이라고 설명한다 |

## 6. API and Protocol Surface

참고 링크: [ax.proto](https://raw.githubusercontent.com/google/ax/main/proto/ax.proto), [content.proto](https://raw.githubusercontent.com/google/ax/main/proto/content.proto), [Transport Issue](https://github.com/google/ax/issues/27), [HarnessService Issue](https://github.com/google/ax/issues/53)

| Service / Message | 현재 역할 | 중요 포인트 |
| --- | --- | --- |
| `ControllerService.Exec` | agentic task 실행 또는 기존 conversation resume. server-streaming `ExecResponse` 반환 | agent job API의 core shape다. async broker-like API는 Issue #7에서 재검토 중이다 |
| `ConversationService` | conversation delete와 fork 제공 | agent execution을 mutable session이 아니라 lifecycle 관리 대상 resource로 본다 |
| `AgentService.Connect` | controller가 remote agent에 `AgentRequest`를 보내고 `AgentResponse` stream을 받는다 | remote agent boundary다. protocol resumption은 significant changes가 예고되어 있다 |
| `HarnessService.Connect` | bidi streaming harness protocol placeholder | BYOH의 핵심이 될 수 있지만 아직 안정 인터페이스가 아니다 |
| `Content` | text, thought, tool_call, tool_result, confirmation, image/audio/document/video를 oneof로 표현 | UI event protocol은 아니지만 typed execution content schema로 발전할 수 있다 |
| `ConversationEvent` | conversation id, seq, exec id, messages, state | client-visible ordered event stream의 기본 단위다 |
| `ExecutionEvent` | exec id, agent id/config, inputs/outputs, state, timestamp | audit와 trace의 기본 단위다. [Issue #50](https://github.com/google/ax/issues/50)은 event log mechanism 변경을 예고한다 |

해석: AX의 proto는 "agent job stream"과 "remote agent stream"을 분리하고 있다. AI 플랫폼 및 서비스에서 그대로 복사하기보다 API compatibility layer를 두고 내부 event schema는 독자적으로 versioning하는 편이 안전하다.

## 7. Interoperability: A2A, ADK, Colab, MCP, Harness

참고 링크: [A2A Example](https://github.com/google/ax/tree/main/examples/a2a_agent), [ADK Example](https://github.com/google/ax/tree/main/examples/adk_agent), [Colab Example](https://github.com/google/ax/tree/main/examples/colab_agent), [ax.yaml](https://raw.githubusercontent.com/google/ax/main/ax.yaml), [OpenAI-compatible Issue](https://github.com/google/ax/issues/28)

| 연결 대상 | 확인된 상태 | AI 플랫폼 및 서비스 의미 |
| --- | --- | --- |
| A2A | registry는 remote agent protocol을 `axp` 또는 `a2a`로 받으며 A2A AgentCard에서 metadata를 채운다 | A2A-compliant agent를 internal runtime에 등록하는 bridge pattern이 가능하다 |
| ADK | examples/adk_agent와 README에서 ADK Python agent를 remote agent로 실행하는 경로를 제공한다 | agent framework를 runtime 아래에 붙이는 reference pattern이다 |
| Colab | config에는 local/Drive notebook, accelerator, requirements, input flag, output image, output Drive path가 있다 | remote compute를 agent registry item으로 다루는 방향. data boundary와 cost control이 필수다 |
| MCP | official site와 README diagram은 MCP support를 말하지만 repo 검색 기준 runtime code surface는 제한적이다 | MCP connector platform을 만들려면 permission, auth, tool catalog, approval, audit를 별도 구현해야 한다 |
| BYOH | HarnessService, controller2, internal/ax2.yaml, Antigravity harness scaffold가 빠르게 변경 중이다 | BYOH는 전략적으로 중요하지만 현재는 interface churn 영역이다 |
| Model providers | default planner는 Gemini. OpenAI-compatible endpoints는 open issue로 논의 중이다 | model agnostic claim은 방향성으로 보고 실제 multi-provider abstraction은 따로 검증해야 한다 |

## 8. Kubernetes, Agent Substrate, Agent Sandbox

참고 링크: [AX manifests README](https://raw.githubusercontent.com/google/ax/main/manifests/README.md), [Agent Substrate GitHub](https://github.com/agent-substrate/substrate), [Agent Substrate Go Packages](https://pkg.go.dev/github.com/agent-substrate/substrate), [GKE Blog](https://cloud.google.com/blog/products/containers-kubernetes/agentic-ai-on-kubernetes-and-gke), [GKE Agent Sandbox Docs](https://docs.cloud.google.com/kubernetes-engine/docs/how-to/agent-sandbox), [Kubernetes Blog](https://kubernetes.io/blog/2026/03/20/running-agents-on-kubernetes-with-agent-sandbox/)

AX의 production deployment story는 Agent Substrate와 Agent Sandbox를 함께 봐야 이해된다. Agent Substrate는 많은 stateful actors를 더 적은 ready workers에 multiplex하고 actor lifecycle(create/destroy/suspend/resume)과 routing을 제공한다. Agent Sandbox는 isolated stateful singleton workload를 Kubernetes CRD와 warm pool로 다룬다.

| Layer | 역할 | AX와 결합했을 때의 의미 |
| --- | --- | --- |
| AX controller | agent execution, event log, registry, stream response | conversation state와 execution ledger의 논리적 owner |
| Agent Substrate | actors를 workers에 할당하고 suspend/resume, traffic routing을 담당 | long-running mostly-idle agent session을 효율적으로 운영하는 runtime substrate |
| Agent Sandbox | SandboxTemplate, SandboxWarmPool, gVisor/Kata isolation, stable identity | untrusted code/tool execution을 Kubernetes-native isolated environment로 운영 |
| Pod Snapshots | running pod checkpoint/restore, idle sandbox snapshot/suspend | agent workspace cold start와 idle cost를 줄이는 infrastructure primitive |
| GCS snapshot path | AX manifests는 workers를 live-snapshot하고 새 conversation 시작 시 GCS에서 restore한다고 설명 | stateful workspace는 storage locality, retention, encryption, restore validation이 필요하다 |
| Warm standby actors | AX manifests는 `WorkerPool`, `ActorTemplate`, router port-forward, `kubectl ate` diagnostics를 사용 | agent pool health, standby capacity, actor leak, suspend failure를 관찰해야 한다 |

## 9. Security and Governance Reading

참고 링크: [exec confirmation flow](https://raw.githubusercontent.com/google/ax/main/cmd/ax/exec.go), [confirmations.go](https://raw.githubusercontent.com/google/ax/main/internal/historyutil/confirmations.go), [GKE Agent Sandbox](https://docs.cloud.google.com/kubernetes-engine/docs/how-to/agent-sandbox), [OpenTelemetry Issue](https://github.com/google/ax/issues/13), [Skills/Bash Issue](https://github.com/google/ax/issues/32), [Community Comparison](https://www.reddit.com/r/LLMDevs/comments/1tukc23/%D1%81ompared_agent_platforms_cloudflare_agents_aws/)

| 영역 | 현재 신호 | 판단 | 필수 보강 |
| --- | --- | --- | --- |
| Human approval | bash tool execution은 confirmation flow를 요구한다고 README가 설명하고 content proto에 confirmation decision이 있다 | 승인 primitive가 있다 | tool risk tier, policy language, subagent approval, channel-specific approval UI |
| Execution isolation | Agent Sandbox는 gVisor/Kata isolation, non-root, service account token automount off 등을 문서화한다 | compute sandbox story는 강하다 | network egress, filesystem mount, secret access, artifact exfiltration policy |
| Credential isolation | AX repo 자체에는 vaulted credential proxy나 per-tool secret boundary가 명확하지 않다 | 가장 큰 gap이다 | credential broker, per-tool identity, mTLS, secret never visible to model, redaction |
| Observability | event log와 trace CLI가 있지만 OpenTelemetry는 open issue다 | debuggability의 시작점은 있다 | OTLP traces/logs/metrics, span context propagation, eval hooks, SLO dashboard |
| MCP security | MCP는 architecture-level claim으로 보이며 runtime implementation과 permission model은 아직 약하다 | MCP connector governance는 별도 과제다 | tool allowlist/filter, manifest review, OAuth scope, result redaction, prompt injection detection |
| Concurrency control | server는 conversation in-flight map으로 같은 conversation 동시 실행을 막는다 | 기본 race guard가 있다 | distributed controller 환경에서는 external lock/lease가 필요할 수 있다 |

핵심 보안 판단: AX의 event log와 sandbox substrate는 production agent의 두 축을 보여준다. 그러나 credential isolation, MCP permission, policy enforcement, enterprise audit는 AX만으로 완결되지 않는다. 도입 시 security control plane을 별도로 설계해야 한다.

## 10. Why This Is Trendy

참고 링크: [GeekNews AX Topic](https://news.hada.io/topic?id=29439), [GitHub Stars](https://github.com/google/ax), [Platform Comparison](https://www.reddit.com/r/LLMDevs/comments/1tukc23/%D1%81ompared_agent_platforms_cloudflare_agents_aws/), [Agyn Discussion](https://www.reddit.com/r/AI_Agents/comments/1tnmp48/agyn_opensource_distributed_agent_runtime_on/), [Copilot Agent Tasks API](https://github.blog/changelog/2026-06-04-agent-tasks-rest-api-now-available-for-copilot-pro-pro-and-max/)

| 트렌드 신호 | AX가 보여주는 방향 | 연결되는 다른 흐름 |
| --- | --- | --- |
| Agent job API | conversation id, exec id, stream seq, resume/fork/delete가 agent run을 resource로 만든다 | GitHub Copilot Agent Tasks API, Cline scheduled agents, Codex Sites deploy loop |
| Runtime durability | event log와 pending execution replay는 agent가 끊겨도 이어지는 구조를 목표로 한다 | Temporal-style workflow, durable execution, long-running coding agents |
| Kubernetes-native agents | Agent Substrate/Sandbox와 함께 stateful isolated singleton workload를 Kubernetes API로 다룬다 | Agyn, kagent, Cloudflare Agents, Bedrock AgentCore 등 runtime 경쟁 |
| Protocol interoperability | A2A bridge, remote AgentService, future harness interface를 통해 framework/harness boundary를 둔다 | A2A, MCP, AG-UI/A2-UI처럼 agent stack이 protocolized되는 흐름 |
| Audit and trajectory | trace CLI와 trajectory exposition issue는 agent outcome보다 process evidence가 중요해지는 신호다 | AgentTrace, AgentRx, AgentBound, SWE-Skills-Bench 같은 eval/observability 흐름 |

## 11. AI Platform / Service Implications

| 플랫폼 기능 | AX에서 배울 점 | 권장 설계 |
| --- | --- | --- |
| Agent run ledger | conversation event와 execution event를 분리한다 | 모든 agent run에 run id, parent run id, input, output, tool call, approval, state, timestamp, policy decision을 남긴다 |
| Resumable UX | last_seq backfill과 resume command를 제공한다 | UI는 마지막으로 본 event를 저장하고 reconnect 시 backfill과 current in-flight status를 보여준다 |
| Fork / replay | conversation fork는 trajectory branching의 시작점이다 | 고위험 task는 forked dry-run, alternate plan, verifier run을 기본 workflow로 둔다 |
| Agent registry | remote/A2A/Colab agents를 registry item으로 등록한다 | agent card, protocol, endpoint, auth, owner, risk tier, allowed tools, eval status, SLA를 registry schema에 넣는다 |
| Execution substrate | Agent Substrate/Sandbox는 agent workspace를 stateful actor로 운영한다 | private data task와 code execution task는 sandbox class를 분리하고 warm pool과 snapshot 정책을 둔다 |
| Approval and policy | confirmation content가 stream에 들어간다 | approval은 UI modal이 아니라 event stream의 first-class content여야 한다 |
| Observability | trace CLI는 초기 형태이며 OTLP는 open issue다 | 처음부터 OpenTelemetry, eval score, run cost, retry, side effect count를 표준 telemetry로 설계한다 |

## 12. Recommended PoC Plan

참고 링크: [AX GitHub](https://github.com/google/ax), [Kubernetes manifests](https://raw.githubusercontent.com/google/ax/main/manifests/README.md), [AX Issues](https://github.com/google/ax/issues)

| 기간 | 실험 | 성공 기준 | 중단 기준 |
| --- | --- | --- | --- |
| 1주 | local AX CLI + SQLite event log로 simple agent run, interrupt, resume, fork, trace를 재현 | event log 구조와 last_seq/replay semantics를 내부 문서로 확정 | core command가 반복적으로 실패하거나 issue와 README가 불일치하면 중단 |
| 2주 | A2A sample agent와 remote AgentService sample을 붙여 registry/protocol boundary를 검증 | agent id, endpoint, auth, metadata, health, output stream이 registry schema로 추상화됨 | A2A bridge가 실제 task lifecycle/artifact를 충분히 보존하지 못하면 자체 adapter 필요 |
| 3주 | Agent Substrate 또는 Agent Sandbox 기반 isolated runtime을 별도 sandbox cluster에서 테스트 | suspend/resume, warm pool latency, workspace cleanup, network/secret boundary가 측정됨 | credential isolation 또는 egress control을 보장할 수 없으면 production data 금지 |
| 4주 | 내부 agent job API 초안을 AX primitive와 비교: create, status, stream, cancel, resume, fork, delete, artifacts, audit | AX에서 채택할 primitive와 자체 구현할 control이 분리됨 | AX interface churn이 커서 integration cost가 과도하면 reference-only로 전환 |

PoC metrics:

- resume success rate: forced interrupt 후 동일 conversation이 중복 side effect 없이 이어지는 비율
- event completeness: output, tool call, approval, state transition, timestamp가 감사에 충분한지
- in-flight behavior: client disconnect, server restart, concurrent request에서 state가 깨지지 않는지
- sandbox cold/warm latency: actor restore, first token, first tool call, first artifact까지의 시간
- security boundary: model-visible secrets, tool-visible secrets, network egress, filesystem mount surface

## 13. Risks / Watch Items

| 우선순위 | 리스크 | 근거 | 추적 액션 |
| --- | --- | --- | --- |
| High | API instability | [README warning](https://github.com/google/ax), [Issue #7](https://github.com/google/ax/issues/7), [Issue #27](https://github.com/google/ax/issues/27), [Issue #53](https://github.com/google/ax/issues/53). README와 proto가 breaking changes를 예고하고 Exec RPC/transport/HarnessService가 open issue | stable release 전 production dependency 금지. adapter layer로 격리 |
| High | Credential and connector security gap | [Community comparison](https://www.reddit.com/r/LLMDevs/comments/1tukc23/%D1%81ompared_agent_platforms_cloudflare_agents_aws/), [GitHub](https://github.com/google/ax). community는 credential isolation이 AX repo 자체에 명확하지 않다고 논의. 공식 repo에서도 vaulted secret boundary는 주요 기능으로 보이지 않음 | secret broker, per-tool identity, result redaction, connector policy를 자체 설계 |
| Medium | MCP support ambiguity | [AX Site](https://agentexecutor.io/), [GitHub](https://github.com/google/ax). site와 diagram은 MCP를 언급하지만 repo code surface는 A2A/AgentService 중심 | MCP runtime readiness를 매 회차 확인하고 MCP는 별도 gateway로 설계 |
| Medium | Event log semantics churn | [Issue #50](https://github.com/google/ax/issues/50), [trace.go TODO](https://raw.githubusercontent.com/google/ax/main/cmd/ax/trace.go). event log mechanism과 trace visualization이 바뀔 예정 | internal ledger schema는 AX schema에 직접 종속시키지 않는다 |
| Medium | Substrate dependency | [AX manifests](https://raw.githubusercontent.com/google/ax/main/manifests/README.md), [Agent Substrate](https://pkg.go.dev/github.com/agent-substrate/substrate). Kubernetes deployment story는 Agent Substrate 설치와 운영을 전제 | local/dev, K8s/sandbox, managed infra 대안을 비교한다 |
| Low | Model/provider agnosticism gap | [Issue #28](https://github.com/google/ax/issues/28), [ax.yaml](https://raw.githubusercontent.com/google/ax/main/ax.yaml). default planner는 Gemini이며 OpenAI-compatible endpoint는 논의 중 | provider abstraction은 internal layer로 유지하고 AX adapter를 붙인다 |

## 14. Decision Notes

- 바로 배울 것: event ledger, seq-based streaming, resume/fork, registry, sandbox actor lifecycle, trace UI, confirmation content model.
- 바로 쓰지 말 것: unstable proto에 직접 종속, production credential handling을 AX에 위임, MCP support를 완성 기능으로 가정, BYOH interface를 고정 계약으로 가정.
- 다음 딥다이브 후보: Agent Substrate와 Agent Sandbox를 함께 묶어 "agent execution substrate" 관점으로 별도 분석할 가치가 높다.
- 최종 판단: AX는 agent runtime 경쟁의 방향을 보여주는 강한 신호다. 도입 전략은 reference architecture + limited PoC + adapter isolation이 적합하다.

## 출처

### 왜 이걸 정리하게 되었는가

- [AX Site](https://agentexecutor.io/)
- [GitHub release v0.1.0](https://github.com/google/ax/releases/tag/v0.1.0)
- [GeekNews AX Topic](https://news.hada.io/topic?id=29439)
- [Platform comparison](https://www.reddit.com/r/LLMDevs/comments/1tukc23/%D1%81ompared_agent_platforms_cloudflare_agents_aws/)
- [Copilot Agent Tasks API](https://github.blog/changelog/2026-06-04-agent-tasks-rest-api-now-available-for-copilot-pro-pro-and-max/)

### 딥리서치 출처

- [Google AX GitHub](https://github.com/google/ax)
- [README Raw](https://raw.githubusercontent.com/google/ax/main/README.md)
- [ax.proto](https://raw.githubusercontent.com/google/ax/main/proto/ax.proto)
- [eventlog.go](https://raw.githubusercontent.com/google/ax/main/internal/controller/executor/eventlog.go)
- [AX manifests README](https://raw.githubusercontent.com/google/ax/main/manifests/README.md)
- [Agent Substrate](https://github.com/agent-substrate/substrate)
- [GKE Agent Sandbox](https://docs.cloud.google.com/kubernetes-engine/docs/how-to/agent-sandbox)
