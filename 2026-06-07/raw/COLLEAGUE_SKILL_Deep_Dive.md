# COLLEAGUE.SKILL Deep Dive

- 작성 시점: 2026-06-07 00:10 KST
- 조사 수준: Deep Dive
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

## 6. Recommended Actions

1. expert skill 후보는 개인이 아니라 role/task 단위로 먼저 정의한다.
2. trace 수집 전 consent, masking, retention, deletion policy를 문서화한다.
3. skill package에는 source provenance, reviewer, version, allowed use case를 필수 metadata로 넣는다.
4. SWE-Skills-Bench 방식으로 skill on/off 성능과 token cost를 측정한다.
5. 사용자나 운영자가 skill 내용을 inspect, correct, rollback할 수 있는 UI를 설계한다.

## 7. Sources

- COLLEAGUE.SKILL arXiv: https://arxiv.org/abs/2605.31264
- Hugging Face paper page: https://huggingface.co/papers/2605.31264
- Counterfactual Trace Auditing of LLM Agent Skills: https://arxiv.org/abs/2605.11946
- SWE-Skills-Bench: https://arxiv.org/abs/2603.15401
- SkillNet: https://arxiv.org/abs/2603.04448
