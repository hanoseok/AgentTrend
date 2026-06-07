# COLLEAGUE.SKILL Deep Dive

- 작성 시점: 2026-06-07 00:27 KST
- 조사 수준: Expanded Deep Dive
- 유형: Paper / Trace-to-skill / Expert skill
- HTML 정본: `2026-06-07/COLLEAGUE_SKILL_Deep_Dive.html`

## 1. Executive Summary

참고 링크: [arXiv](https://arxiv.org/abs/2605.31264), [Hugging Face Papers](https://huggingface.co/papers/2605.31264), [Skill Trace Auditing](https://arxiv.org/abs/2605.11946), [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401)

COLLEAGUE.SKILL은 사람이나 role의 작업 trace를 agent가 사용할 수 있는 versioned skill package로 distill하는 논문이다. AI 플랫폼 관점에서는 “지식 검색”을 넘어 “업무 방식, 판단 기준, 커뮤니케이션 습관”을 audit 가능한 스킬로 이전하려는 흐름을 보여준다.

핵심 판단:

- expert skill platform의 방향을 보여주지만, 사람의 trace를 스킬화하는 순간 privacy, consent, IP, 평가/감시 이슈가 같이 생긴다.
- 기술 PoC보다 governance 설계가 먼저다.
- memory, persona, skill framework가 versioned, inspectable, correctable artifact로 수렴하고 있다.

## 2. What It Is

논문은 target person 또는 role의 자료, 작업 흔적, 수정 이력, 커뮤니케이션 예시를 바탕으로 person-grounded AI skill을 생성하는 trace-to-skill distillation system을 제안한다. 결과물은 단순 persona prompt가 아니라 versioned skill package다.

핵심은 “누군가처럼 말하게 하기”가 아니라 “특정 역할의 업무 판단과 실행 방식을 inspectable하게 포장하고 업데이트할 수 있게 하기”다. 이 점에서 COLLEAGUE.SKILL은 enterprise skill, expert assistant, role-based agent 운영의 중요한 참고점이다.

## 3. Two Coordinated Tracks

| 트랙 | 포함 내용 | 플랫폼 의미 |
| --- | --- | --- |
| Capability Track | 업무 practices, mental models, decision heuristics, 절차적 판단 기준. | role별 skill card, playbook, checklist, tool-use rule로 변환 가능하다. |
| Bounded Behavior Track | communication style, interaction rules, correction history, 응답 경계. | 톤앤매너와 행동 경계를 분리해 통제할 수 있다. |
| Versioned Package | 생성, 수정, 검토, rollback 가능한 skill artifact. | agent skill을 배포 가능한 소프트웨어 자산으로 관리한다. |
| Inspectable Output | 사람이 검토하고 고칠 수 있는 구조화된 결과물. | black-box personalization보다 governance에 적합하다. |

## 4. AI 플랫폼 및 서비스 적용 가능성

| 영역 | 가능한 skill | 초기 검증 방식 |
| --- | --- | --- |
| 개발/운영 | 코드리뷰 관점, 장애 대응 playbook, 배포 점검, legacy 운영 지식. | 실제 issue/incident replay로 skill on/off 비교. |
| 서비스 운영 | 정책 위반 판단, 고객 문의 분류, escalation 기준, 공지 작성 기준. | 과거 ticket sample과 reviewer agreement 측정. |
| 콘텐츠/커뮤니티 | moderation playbook, tone guide, safety 판단 기준. | blind review와 false positive/negative 측정. |
| 비즈니스 운영 | 상품 운영 rule, 파트너 대응 방식, 광고/추천 정책. | 업무 산출물 품질 평가와 human correction rate 측정. |

## 5. Governance Risks

| 리스크 | 문제 | 필수 통제 |
| --- | --- | --- |
| Privacy | 작업 trace에 개인정보, 민감한 업무 정보, private communication이 포함될 수 있다. | PII masking, data minimization, source allowlist. |
| Consent | 개인의 업무 방식이 skill로 복제될 수 있다. | 명시적 동의, 사용 범위 제한, opt-out, 삭제권. |
| IP / Labor | 개인의 노하우와 조직 자산의 경계가 모호하다. | ownership policy, usage policy, access control. |
| Surveillance | skill 생성이 평가/감시 도구로 오용될 수 있다. | 목적 제한, audit, reviewer separation. |
| Behavior Drift | 업데이트 과정에서 역할 기준이 왜곡될 수 있다. | versioning, rollback, eval set, human approval. |

## 6. Trace-to-Skill Lifecycle

참고 링크: [COLLEAGUE.SKILL arXiv](https://arxiv.org/abs/2605.31264), [Hugging Face Papers](https://huggingface.co/papers/2605.31264)

COLLEAGUE.SKILL의 핵심은 사람 또는 role의 trace를 한 번에 persona prompt로 바꾸는 것이 아니라, 수집, 필터링, 증류, 검토, 배포, 수정, rollback이 가능한 lifecycle로 다루는 것이다. 이 lifecycle을 갖추지 않으면 expert skill은 지식 이전 도구가 아니라 감시와 모방 리스크가 큰 블랙박스가 된다.

| 단계 | 주요 질문 | 필수 통제 |
| --- | --- | --- |
| Trace Collection | 어떤 자료가 업무 전문성을 대표하는가? | source allowlist, 목적 제한, 기간 제한, 민감정보 제외 |
| Filtering / Masking | 개인정보, 고객정보, 내부 기밀이 포함되어 있는가? | PII redaction, private message exclusion, reviewer approval |
| Skill Distillation | capability와 behavior를 분리해 추출했는가? | role-level heuristic, examples/counterexamples, forbidden behavior |
| Human Inspection | 당사자 또는 owner가 내용을 이해하고 고칠 수 있는가? | diff view, natural-language correction, approval checkpoint |
| Deployment | 어떤 agent host와 task에서 사용할 수 있는가? | scope, owner, version, expiration, access control |
| Correction / Rollback | 잘못된 판단이나 style drift를 어떻게 되돌리는가? | correction log, eval set, rollback version, deletion right |

## 7. Artifact Contract

참고 링크: [COLLEAGUE.SKILL](https://arxiv.org/abs/2605.31264), [Skill Trace Auditing](https://arxiv.org/abs/2605.11946)

전문가 스킬은 “어떤 사람처럼 답하라”는 한 줄 prompt로 관리하면 안 된다. inspectable artifact가 되려면 아래 contract를 갖춰야 한다. 특히 capability와 bounded behavior를 분리해야 지식 이전과 인격 모방을 구분할 수 있다.

| 필드 | 내용 | 왜 필요한가 |
| --- | --- | --- |
| `skill_scope` | 역할, task, 사용 가능한 상황, 금지 상황 | 개인 전반이 아니라 업무 단위 전문성만 이전 |
| `capability_model` | decision heuristics, checklist, escalation rule, tool-use pattern | 실제 업무 판단을 재사용 가능한 구조로 만듦 |
| `bounded_behavior` | 톤, 응답 경계, interaction norm, correction history | 커뮤니케이션 품질을 통제하되 persona overreach를 막음 |
| `provenance` | 사용된 자료 유형, 기간, 승인자, masking 방식 | 감사와 삭제 요청 대응에 필요 |
| `eval_set` | role-task replay, reviewer rubric, expected/non-expected answer | 스킬이 실제로 도움이 되는지 검증 |
| `governance` | owner, access group, retention, deletion, redistribution policy | 개인 trace 기반 자산의 책임과 권리를 명확히 함 |

## 8. Evaluation Framework

참고 링크: [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401), [Skill Trace Auditing](https://arxiv.org/abs/2605.11946)

COLLEAGUE.SKILL류 시스템은 “그 사람처럼 보인다”가 아니라 “해당 role의 업무 성과와 판단 일관성을 높이는가”로 평가해야 한다. 또한 개인정보와 감시 리스크 때문에 성능 평가와 governance 평가를 함께 통과해야 한다.

| 평가 축 | 측정 방식 | 실패 시 조치 |
| --- | --- | --- |
| Role-task Accuracy | 과거 ticket/incident/review sample replay에서 정답 또는 reviewer agreement 측정 | capability track 수정, scope 축소 |
| Decision Consistency | 동일 기준이 유사 케이스에 일관되게 적용되는지 측정 | heuristic과 counterexample 보강 |
| Behavior Boundary | 개인의 말투 모방보다 role-appropriate communication을 유지하는지 검토 | bounded behavior 재작성, persona 표현 제한 |
| Privacy / PII Audit | 스킬 본문과 예시에 민감정보가 남아 있는지 검사 | redaction 후 재검토, source exclusion |
| Correction Rate | 실제 사용 중 human correction이 얼마나 자주 발생하는지 추적 | 재학습, rollback, quarantine |
| Skill Utility | skill on/off의 task quality, time, token, escalation rate 비교 | 효과가 없으면 search-only 또는 archive |

## 9. Recommended Actions

1. expert skill 후보는 개인이 아니라 role/task 단위로 먼저 정의한다.
2. trace 수집 전 consent, masking, retention, deletion policy를 문서화한다.
3. skill package에는 source provenance, reviewer, version, allowed use case를 필수 metadata로 넣는다.
4. SWE-Skills-Bench 방식으로 skill on/off 성능과 token cost를 측정한다.
5. 사용자나 운영자가 skill 내용을 inspect, correct, rollback할 수 있는 UI를 설계한다.
6. 개인 trace 기반 skill은 배포 전 privacy/consent 심사를 통과한 경우에만 사용할 수 있게 한다.
7. 초기 PoC는 개인 이름을 붙인 skill이 아니라 “장애 대응 reviewer”, “정책 분류 reviewer”처럼 role skill로 제한한다.

## 출처

### 왜 이걸 정리하게 되었는가

- [COLLEAGUE.SKILL arXiv](https://arxiv.org/abs/2605.31264)
- [Hugging Face Papers](https://huggingface.co/papers/2605.31264)
- [Skill Trace Auditing](https://arxiv.org/abs/2605.11946)
- [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401)

### 딥리서치 출처

- [COLLEAGUE.SKILL arXiv](https://arxiv.org/abs/2605.31264)
- [Hugging Face Papers](https://huggingface.co/papers/2605.31264)
- [Skill Trace Auditing](https://arxiv.org/abs/2605.11946)
- [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401)
