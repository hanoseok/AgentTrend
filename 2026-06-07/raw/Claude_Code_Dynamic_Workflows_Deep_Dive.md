# Claude Code Dynamic Workflows / ultracode Deep Dive

- 작성 시점: 2026-06-07 00:04 KST
- 조사 수준: Deep Dive
- 유형: Agent runtime / orchestration
- HTML 정본: `2026-06-07/Claude_Code_Dynamic_Workflows_Deep_Dive.html`

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

## 6. Recommended Actions

1. 30일 내: 하나의 개발/운영 task를 골라 subagent task graph PoC를 만든다.
2. Workflow schema: task, branch, verifier, budget, approval, artifact schema를 정의한다.
3. Cost guardrail: workflow별 token cap, subagent cap, stop condition을 둔다.
4. Verifier first: test/build/security/policy checker 없이 자동 merge/action을 허용하지 않는다.
