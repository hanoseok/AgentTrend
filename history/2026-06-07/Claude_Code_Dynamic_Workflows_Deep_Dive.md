# Claude Code Dynamic Workflows / ultracode Deep Dive

- 작성 시점: 2026-06-07 00:27 KST
- 조사 수준: Expanded Deep Dive
- 유형: Agent runtime / orchestration
- HTML 정본: `report/claude-code-dynamic-workflows/`

## 1. Executive Summary

참고 링크: [Anthropic Blog](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code), [open-dynamic-workflows](https://github.com/imsai-sh/open-dynamic-workflows), [r/ClaudeCode](https://www.reddit.com/r/ClaudeCode/comments/1tq9pge/introducing_dynamic_workflows_in_claude_code/)

핵심 판단: Dynamic Workflows는 모델 기능이라기보다 agent runtime 요구사항이다. AI 플랫폼은 subtask graph, checkpoint, verifier, cost cap, human approval을 하나의 control plane으로 설계해야 한다.

- 대규모 코드 마이그레이션, 보안 audit, 운영 점검처럼 장시간 병렬성이 필요한 작업을 agent가 처리할 수 있게 한다.
- “모델이 답한다”에서 “모델이 실행 계획과 workflow를 만들고 외부 runtime이 수행한다”로 agent 제품의 중심이 이동한다.

## 2. What It Is

Anthropic이 2026-05-28 공개한 Claude Code 기능이다. Claude가 큰 작업을 subtasks로 나누고, 동적으로 orchestration script를 작성한 뒤, 여러 subagent를 병렬 실행하는 research preview 성격의 기능으로 소개됐다.

공식 발표는 Claude Code CLI, Desktop, VS Code extension, API, Bedrock, Vertex AI, Microsoft Foundry 등에서 사용 가능한 흐름을 설명한다. 커뮤니티에서는 ultracode/open-dynamic-workflows 같은 재현 프로젝트와 함께 token burn, review burden, 실패 제어권에 대한 논의가 이어졌다.

## 3. Trend Signals

참고 링크: [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/comments/1tq9ofy/introducing_dynamic_workflows_in_claude_code/), [r/vibecoding](https://www.reddit.com/r/vibecoding/comments/1tqe2yp/anthropic_just_introduced_dynamic_workflows_in/)

| 신호 | 의미 | 판단 |
| --- | --- | --- |
| 공식 발표 | Claude가 workflow code와 subagent orchestration을 직접 생성 | agent runtime이 제품의 전면 기능으로 올라왔다 |
| Bun Zig-to-Rust 사례 | 공식 발표는 대규모 코드 포팅 사례를 제시 | 사례 수치는 별도 재현 검증이 필요하지만 방향성은 강하다 |
| 오픈소스 재현 | open-dynamic-workflows 같은 구현이 등장 | 기능의 핵심이 특정 UI보다 orchestration pattern에 있음을 보여준다 |
| Reddit 반응 | token cost, human review, 실패 경계가 주요 쟁점 | 제품화에는 cost/approval/tracing이 필수다 |

## 4. Technical Model

| 구성요소 | 역할 | 플랫폼 설계 포인트 |
| --- | --- | --- |
| Planner | 작업을 subtask graph로 분해 | task type, risk tier, expected output schema를 함께 생성 |
| Workflow Script | subagent 실행 순서와 병렬/순차 흐름을 정의 | 검증 가능한 DSL 또는 제한된 runtime으로 감싼다 |
| Subagent Pool | 탐색, 구현, 검증, 요약 등 역할별 agent 실행 | branch cap, token cap, timeout, cancellation을 둔다 |
| Verifier | 테스트, 빌드, policy check, screenshot, security scan 수행 | 결과를 Artifact로 남기고 다음 단계에서 참조 |
| Human Approval | 고위험 merge/action 전 사용자 확인 | 승인 전후 상태와 근거를 audit log에 기록 |

## 5. AI 플랫폼 및 서비스 적용 방향

- 장시간 업무 agent는 대화 session이 아니라 run/task 단위로 관리해야 한다.
- model routing, token budget, branch budget, retry policy를 runtime 기본 기능으로 둔다.
- 결과 품질은 “agent가 말했다”가 아니라 executable verifier로 확인한다.
- workflow 결과는 텍스트 요약뿐 아니라 Artifact, trace, approval record, rollback link를 포함해야 한다.

## 6. Research Lens: 모델 기능이 아니라 실행 시스템

참고 링크: [Launch Post](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code), [Harness Post](https://claude.com/blog/a-harness-for-every-task-dynamic-workflows-in-claude-code)

Dynamic Workflows의 본질은 “더 긴 생각을 하는 모델”이 아니다. 핵심은 모델이 task-specific harness를 만들고, 그 harness가 subagent 실행, 중간 결과 저장, 검증, 수렴 판단을 담당한다는 점이다. 따라서 경쟁력은 모델 단독 성능보다 run graph, worker isolation, checkpoint, verifier, approval UI를 결합한 실행 시스템에서 나온다.

공식 문서는 workflow가 연구, 보안 분석, agent team, code review처럼 기존에는 별도 harness가 필요했던 작업을 더 자연스럽게 수행하게 만든다고 설명한다. 이 말은 agent 제품이 “대화 UI”에서 “작업별 임시 운영체제”로 이동한다는 의미다. 사용자는 결과를 받지만, 플랫폼은 뒤에서 planner, executor, verifier, reviewer를 분리해 운영해야 한다.

리서치 해석: 앞으로 고급 agent 기능은 prompt template이 아니라, 모델이 생성하거나 선택한 executable harness를 안전하게 실행하는 platform layer가 된다.

## 7. Execution Mechanics

참고 링크: [open-dynamic-workflows](https://github.com/imsai-sh/open-dynamic-workflows), [r/ClaudeCode](https://www.reddit.com/r/ClaudeCode/comments/1tq9pge/introducing_dynamic_workflows_in_claude_code/)

| 단계 | 실행 내용 | 제품화 시 필요한 통제 |
| --- | --- | --- |
| Task Framing | 사용자 요청을 목표, 범위, 금지 조건, 성공 기준으로 정규화 | 요구사항 snapshot, risk tier, 예상 비용, 승인 필요 여부를 먼저 확정 |
| Dynamic Planning | 모델이 작업을 병렬/순차 subtask graph로 나누고 역할별 worker를 정의 | branch cap, dependency edge, retry policy, stop condition 명시 |
| Harness Generation | workflow script 또는 제한된 DSL이 worker 실행과 결과 수집을 조율 | 생성된 harness를 sandbox에서 실행하고, 외부 action 전 approval 요구 |
| Independent Verification | 각 결과를 test, build, static check, secondary reviewer, adversarial agent로 검증 | 검증 없는 결과는 final answer나 production action으로 승격하지 않음 |
| Convergence | 상충 결과를 비교하고 재시도하며 최종 산출물과 근거를 합침 | conflict log, rejected candidate, confidence reason을 artifact로 남김 |
| Resume / Recovery | 장시간 run이 중단되어도 저장된 진행 상태에서 재개 | checkpoint format, artifact retention, cancellation, rollback 표준화 |

## 8. Evaluation Protocol

Dynamic Workflows류 기능은 “멋진 데모”가 아니라 기존 single-agent, fixed workflow, human-led process와 비교해야 한다. 특히 공식 발표의 대규모 migration 사례는 방향성 신호로는 강하지만, 플랫폼 의사결정에는 독립 재현과 실패 사례 분석이 필요하다.

| 평가 축 | 측정 지표 | 실험 설계 |
| --- | --- | --- |
| 성공률 | acceptance test pass-rate, reviewer acceptance, rollback 없는 merge 비율 | 동일 task를 single-agent, static workflow, dynamic workflow로 paired run |
| 검증 품질 | false positive, false negative, duplicated finding, missed edge case | 보안 audit, dead code scan, migration validation처럼 ground truth를 만들 수 있는 task 사용 |
| 비용 | token, wall-clock, subagent count, retry count, human review time | run마다 budget envelope와 stop reason 기록 |
| 복구성 | interrupt 후 resume 성공률, partial artifact 재사용률, failed branch 격리 여부 | 중간 실패, API failure, test failure, reviewer reject를 의도적으로 주입 |
| 운영 안전성 | 권한 위반, 승인 누락, 고위험 action 차단률 | AgentBound식 tool policy와 연결해 workflow action 제한 |

## 9. Platform Blueprint

참고 링크: [AgentBound Deep Dive](../../report/agentbound-deep-dive/index.html), [SWE-Skills-Bench Deep Dive](../../report/swe-skills-bench-deep-dive/index.html)

| 모듈 | 필수 데이터 | 설계 판단 |
| --- | --- | --- |
| Run Graph Store | task id, parent/child edge, worker role, status, artifacts | 대화 transcript보다 run graph가 운영의 source of truth가 됨 |
| Budget Manager | token cap, branch cap, time cap, retry cap, escalation threshold | 동적 workflow는 기본적으로 비용 폭주 가능성이 있으므로 선한도와 중간 경고 필요 |
| Verifier Registry | test command, build command, policy check, screenshot diff, security scanner | agent가 만든 결과보다 verifier가 통과시킨 결과를 신뢰 |
| Approval Console | action summary, affected resource, risk tier, evidence, rollback plan | 고위험 action은 채팅 확인이 아니라 구조화된 승인 카드 필요 |
| Artifact Ledger | input hash, output hash, logs, rejected alternatives, final rationale | 사후 브리핑, 품질 분석, 분쟁 대응에 필요 |

## 10. Recommended Actions

1. 30일 내: 하나의 개발/운영 task를 골라 subagent task graph PoC를 만든다.
2. Workflow schema: task, branch, verifier, budget, approval, artifact schema를 정의한다.
3. Cost guardrail: workflow별 token cap, subagent cap, stop condition을 둔다.
4. Verifier first: test/build/security/policy checker 없이 자동 merge/action을 허용하지 않는다.
5. PoC 평가는 single-agent baseline과 paired comparison으로 설계한다.
6. AgentBound식 tool permission과 연결해 workflow worker별 권한을 최소화한다.

## 출처

### 왜 이걸 정리하게 되었는가

- [Anthropic Blog](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code)
- [r/ClaudeCode](https://www.reddit.com/r/ClaudeCode/comments/1tq9pge/introducing_dynamic_workflows_in_claude_code/)
- [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/comments/1tq9ofy/introducing_dynamic_workflows_in_claude_code/)
- [r/vibecoding](https://www.reddit.com/r/vibecoding/comments/1tqe2yp/anthropic_just_introduced_dynamic_workflows_in/)

### 딥리서치 출처

- [Launch Post](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code)
- [Harness Post](https://claude.com/blog/a-harness-for-every-task-dynamic-workflows-in-claude-code)
- [open-dynamic-workflows](https://github.com/imsai-sh/open-dynamic-workflows)
