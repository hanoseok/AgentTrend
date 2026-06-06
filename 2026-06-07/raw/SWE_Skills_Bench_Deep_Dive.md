# SWE-Skills-Bench Deep Dive

- 작성 시점: 2026-06-07 00:10 KST
- 조사 수준: Deep Dive
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

## 6. Recommended Actions

1. Skill card schema에 `task_scope`, `supported_versions`, `known_failures`, `eval_result`, `token_cost`를 넣는다.
2. 내부 후보 skill 10개를 골라 skill on/off paired evaluation을 수행한다.
3. pass-rate 개선이 없는 skill은 기본 주입하지 않고 search-only 상태로 둔다.
4. version mismatch가 감지되면 skill을 자동 비활성화하거나 경고를 표시한다.
5. 스킬 추천 UX에는 “왜 이 스킬을 넣었는가”와 “예상 비용”을 함께 보여준다.

## 7. Sources

- SWE-Skills-Bench arXiv: https://arxiv.org/abs/2603.15401
- SWE-Skills-Bench GitHub: https://github.com/GeniusHTX/SWE-Skills-Bench
- GitHub CLI Skills changelog: https://github.blog/changelog/2026-04-16-manage-agent-skills-with-github-cli/
- Counterfactual Trace Auditing: https://arxiv.org/abs/2605.11946
- SkillNet paper: https://arxiv.org/abs/2603.04448
- SkillNet GitHub: https://github.com/zjunlp/SkillNet
