# SWE-Skills-Bench Deep Dive

- 작성 시점: 2026-06-07 00:27 KST
- 조사 수준: Expanded Deep Dive
- 유형: Paper / Skill eval / SWE benchmark
- HTML 정본: `2026-06-07/SWE_Skills_Bench_Deep_Dive.html`

## 1. Executive Summary

참고 링크: [arXiv](https://arxiv.org/abs/2603.15401), [GitHub](https://github.com/GeniusHTX/SWE-Skills-Bench), [GitHub gh skill](https://github.blog/changelog/2026-04-16-manage-agent-skills-with-github-cli/), [Skill Trace Auditing](https://arxiv.org/abs/2605.11946)

SWE-Skills-Bench는 agent skill이 실제 software engineering task에서 성능을 올리는지 paired evaluation으로 측정한 벤치마크다. 결론은 냉정하다. 스킬은 많이 넣는다고 좋아지는 context가 아니라, 검증된 task fit이 있을 때만 유효한 고비용 실행 자산이다.

핵심 판단:

- skill marketplace의 KPI는 skill 개수가 아니라 measured utility다.
- 등록 전후 pass-rate, token cost, wall-clock, version compatibility, failure mode를 기록하지 않으면 skill은 생산성 도구가 아니라 configuration drift가 된다.
- skill activation은 retrieval 문제이자 cost optimization 문제다.

## 2. What It Is

논문은 49개 public SWE skills를 실제 GitHub repository, 고정 commit, acceptance criteria가 있는 requirement document, deterministic tests와 묶어 평가한다. 같은 task를 skill 주입 전후로 비교해 skill의 marginal utility를 분리 측정하는 방식이다.

중요한 점은 벤치마크가 단순 문답이 아니라 end-to-end software engineering task를 다룬다는 것이다. 즉, skill이 모델의 답변을 그럴듯하게 만드는지보다 실제 수정, 테스트 통과, 요구사항 충족에 도움이 되는지를 본다.

## 3. Key Findings

| 결과 | 논문 수치 | 플랫폼 해석 |
| --- | --- | --- |
| 효과 없음 | 49개 public SWE skills 중 39개가 pass-rate 개선을 만들지 못했다고 보고한다. | skill registry는 수량 중심으로 운영하면 실패한다. |
| 평균 개선 제한 | 평균 pass-rate 개선은 +1.2%로 제한적이라고 보고한다. | 범용 skill을 기본 주입하는 전략은 비용 대비 효과가 낮을 수 있다. |
| 토큰 비용 | pass-rate가 변하지 않는 상황에서 token overhead가 최대 451% 증가할 수 있다고 보고한다. | skill activation은 retrieval과 cost optimization 문제다. |
| 전문화 효과 | 7개 specialized skills는 의미 있는 개선을 만들고, 최대 +30% 개선을 보고한다. | task-specific, version-aware skill은 가치가 있다. |
| 버전 불일치 | version-mismatched guidance는 최대 -10% 성능 저하를 만들었다고 보고한다. | skill card에는 지원 버전, 환경, 금지 조건이 필요하다. |

## 4. Skill Marketplace 설계 원칙

| 기존 가정 | 벤치마크가 주는 경고 | 설계 원칙 |
| --- | --- | --- |
| 스킬이 많을수록 좋다. | 다수 public skills는 pass-rate 개선이 없었다. | 검증된 task coverage와 success delta를 KPI로 둔다. |
| 좋은 설명은 좋은 실행을 만든다. | 추상적이거나 오래된 guidance는 방해가 될 수 있다. | supported versions, repository patterns, failure cases를 기록한다. |
| 스킬은 무료 context다. | token overhead가 커질 수 있다. | activation threshold, token budget, expected benefit score를 둔다. |
| 인기 스킬은 자동 추천해도 된다. | project compatibility가 맞지 않으면 성능이 내려간다. | repository fingerprint, dependency version, task type matching을 사용한다. |

## 5. SkillNet과 함께 읽는 이유

참고 링크: [SkillNet Paper](https://arxiv.org/abs/2603.04448), [SkillNet GitHub](https://github.com/zjunlp/SkillNet), [SkillNet Site](https://skillnet.openkg.cn)

SkillNet은 skill을 만들고 연결하는 공급 측 infrastructure를 보여준다. SWE-Skills-Bench는 그 skill이 실제 task에 도움이 되는지 따져야 한다는 수요 측 검증 프레임을 제공한다. 두 논문을 합치면 skill platform의 핵심은 graph와 eval을 동시에 운영하는 것이다.

- Skill graph: prerequisite, compose_with, depend_on, similar_to 관계를 관리한다.
- Measured utility: pass-rate delta, token delta, wall-clock delta, failure cases를 저장한다.
- Compatibility: language, framework, version, repository shape, task type을 skill metadata로 둔다.
- Activation: 모든 skill을 넣지 않고 task별 최소 후보만 검색, 설명, 주입한다.

## 6. Research Method Reading

참고 링크: [SWE-Skills-Bench arXiv](https://arxiv.org/abs/2603.15401), [Benchmark GitHub](https://github.com/GeniusHTX/SWE-Skills-Bench)

이 논문은 skill 효과를 “모델이 좋아 보이는 답을 냈는가”가 아니라 “동일한 실제 SWE task에서 skill 주입 전후 pass-rate가 변했는가”로 측정한다. 따라서 skill의 marginal utility를 분리해 보는 데 가치가 있다. 특히 repository를 fixed commit으로 고정하고, requirement document와 deterministic verification을 결합한 점이 실무 평가에 가깝다.

| 설계 요소 | 왜 중요한가 | 내부 평가로 옮길 때 |
| --- | --- | --- |
| Public skills 49개 | 실제 유통되는 skill의 품질 편차를 반영 | 내부/외부 skill을 섞어 평가하되, 출처와 작성 시점을 기록 |
| Pinned repositories | 동일 task 반복 평가와 회귀 비교가 가능 | 대표 서비스 repo를 snapshot으로 고정하고 branch drift를 차단 |
| Acceptance criteria | 추상적 “잘함” 대신 명시 요구사항 충족을 측정 | ticket, PRD, 운영 요청을 testable requirement로 변환 |
| Paired evaluation | skill on/off 차이를 직접 비교 | 같은 모델, 같은 seed/temperature, 같은 tool 권한으로 skill만 변경 |
| Execution-based tests | 최종 산출물이 실제로 동작하는지 확인 | unit/integration/e2e test와 reviewer rubric을 함께 사용 |

## 7. Skill Contract: Marketplace가 갖춰야 할 최소 형식

참고 링크: [GitHub gh skill](https://github.blog/changelog/2026-04-16-manage-agent-skills-with-github-cli/), [Skill Trace Auditing](https://arxiv.org/abs/2605.11946)

논문이 주는 핵심 경고는 skill을 prompt 조각으로 취급하면 안 된다는 것이다. skill은 실행 자산이므로 적용 범위, 버전 호환성, 실패 조건, 평가 결과, 비용 정보를 포함해야 한다. 그래야 추천/자동 활성화가 근거 있는 의사결정이 된다.

| 필드 | 설명 | 없을 때 생기는 문제 |
| --- | --- | --- |
| `task_scope` | 어떤 작업 유형에서 사용해야 하는지 | 범용 skill로 오인되어 context noise를 만듦 |
| `compatibility` | 언어, framework, dependency version, repository shape | version-mismatched guidance로 성능이 하락 |
| `activation_rule` | 언제 자동 주입하고 언제 search-only로 둘지 | 모든 skill을 기본 주입해 비용이 폭증 |
| `eval_result` | pass-rate delta, token delta, wall-clock delta, test set | 인기나 직관만으로 추천 |
| `known_failures` | 실패한 repo, task, 버전, 반례 | 같은 실패가 반복되고 correction이 축적되지 않음 |
| `review_status` | 검토자, 승인 시점, 재평가 주기 | skill supply chain과 책임 소재가 불명확 |

## 8. Internal Evaluation Playbook

AI 플랫폼에서 skill registry를 만들려면 출시 전후로 아래 실험을 반복해야 한다. 특히 “좋아 보이는 skill”보다 “비활성화했을 때 성능이 떨어지는 skill”만 기본 활성화 후보로 남겨야 한다.

| 실험 | 통과 기준 | 운영 결정 |
| --- | --- | --- |
| Skill On/Off Paired Run | 동일 task set에서 통계적으로 의미 있는 pass-rate 또는 reviewer score 개선 | 개선이 없으면 default injection 금지 |
| Token Cost Audit | 효과 대비 token 증가가 허용 budget 안에 있음 | 고비용 skill은 explicit opt-in 또는 high-value task 전용 |
| Version Compatibility Test | 지원 버전 밖에서 자동 비활성화 또는 경고 | repository fingerprint 기반 activation |
| Negative Transfer Test | 관련 없는 task에서 성능 저하를 만들지 않음 | scope를 좁히거나 retrieval-only로 전환 |
| Regression Re-eval | 모델/도구/프레임워크 업데이트 후 기존 효과 유지 | 재평가 실패 skill은 quarantine |

## 9. Recommended Actions

1. Skill card schema에 `task_scope`, `supported_versions`, `known_failures`, `eval_result`, `token_cost`를 넣는다.
2. 내부 후보 skill 10개를 골라 skill on/off paired evaluation을 수행한다.
3. pass-rate 개선이 없는 skill은 기본 주입하지 않고 search-only 상태로 둔다.
4. version mismatch가 감지되면 skill을 자동 비활성화하거나 경고를 표시한다.
5. 스킬 추천 UX에는 “왜 이 스킬을 넣었는가”와 “예상 비용”을 함께 보여준다.
6. skill registry의 핵심 KPI를 등록 수가 아니라 pass-rate delta와 cost-adjusted utility로 둔다.
7. 모델 또는 tool runtime이 바뀔 때마다 상위 skill을 자동 재평가하는 pipeline을 만든다.

## 출처

### 왜 이걸 정리하게 되었는가

- [SWE-Skills-Bench arXiv](https://arxiv.org/abs/2603.15401)
- [Benchmark GitHub](https://github.com/GeniusHTX/SWE-Skills-Bench)
- [GitHub gh skill](https://github.blog/changelog/2026-04-16-manage-agent-skills-with-github-cli/)
- [SkillNet Paper](https://arxiv.org/abs/2603.04448)

### 딥리서치 출처

- [SWE-Skills-Bench arXiv](https://arxiv.org/abs/2603.15401)
- [Benchmark GitHub](https://github.com/GeniusHTX/SWE-Skills-Bench)
- [Skill Trace Auditing](https://arxiv.org/abs/2605.11946)
- [SkillNet GitHub](https://github.com/zjunlp/SkillNet)
- [SkillNet Site](https://skillnet.openkg.cn)
