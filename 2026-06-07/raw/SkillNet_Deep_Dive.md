# SkillNet Deep Dive

- 작성 시점: 2026-06-07 00:10 KST
- 조사 수준: Deep Dive
- 유형: Paper / Project / Skill infrastructure / skill graph
- HTML 정본: `2026-06-07/SkillNet_Deep_Dive.html`

## 1. Executive Summary

참고 링크: [arXiv](https://arxiv.org/abs/2603.04448), [GitHub](https://github.com/zjunlp/SkillNet), [Project Site](https://skillnet.openkg.cn), [Hugging Face Blog](https://huggingface.co/blog/xzwnlp/skillnet)

SkillNet은 AI skills를 생성, 평가, 검색, 연결하기 위한 open infrastructure다. skill을 prompt 조각이 아니라 ontology, graph, evaluation metadata를 가진 software asset으로 다루는 방향을 보여준다.

핵심 판단:

- SkillNet은 skill platform의 공급 측 청사진이다.
- 다만 “많은 skill”만으로는 충분하지 않다. SWE-Skills-Bench가 보여준 것처럼 measured utility, compatibility, token cost, activation policy를 함께 붙여야 AI 플랫폼의 실제 생산성 자산이 된다.
- agent 제품의 경쟁력이 model prompt보다 skill graph, eval, runtime activation으로 이동하고 있다.

## 2. What It Is

SkillNet 논문은 heterogeneous source에서 skill을 만들고, unified ontology로 관계를 연결하며, Safety, Completeness, Executability, Maintainability, Cost-awareness 기준으로 다차원 평가하는 open infrastructure를 제안한다.

프로젝트는 paper, GitHub repository, interactive platform, Python toolkit을 함께 제공한다. GitHub README는 keyword/semantic search, one-line install, auto-create, 5-D evaluation, skill graph를 주요 기능으로 설명한다.

## 3. Architecture Reading

| 구성요소 | 역할 | AI 플랫폼 설계 포인트 |
| --- | --- | --- |
| Skill Repository | 대규모 skill을 저장하고 검색. | skill source, license, owner, version, eval metadata를 함께 저장한다. |
| Skill Ontology | skill의 domain, task, prerequisite, relation을 구조화. | 서비스별 domain taxonomy와 연결한다. |
| Skill Graph | `similar_to`, `belong_to`, `compose_with`, `depend_on` 관계. | workflow planner가 skill 조합을 추천할 수 있게 한다. |
| Auto-Create Pipeline | repo, PDF, 문서, 대화, text prompt에서 skill 생성. | source provenance와 human review 없이는 production 등록하지 않는다. |
| 5-D Evaluation | Safety, Completeness, Executability, Maintainability, Cost-awareness 평가. | quality gate와 activation score의 기본 지표가 된다. |

## 4. Evidence and Caveats

| 근거 | 내용 | 주의점 |
| --- | --- | --- |
| 대규모 repository | 논문은 200,000개 이상의 skills repository를 통합한다고 설명한다. 일부 marketplace/검색 사이트는 더 큰 수치를 표기한다. | 수량은 품질이나 task fit을 의미하지 않는다. |
| Benchmark 성과 | ALFWorld, WebShop, ScienceWorld에서 average rewards 40% 향상, execution steps 30% 감소를 보고한다. | 해당 benchmark 환경 기준이며 실제 업무 task에는 별도 검증이 필요하다. |
| 5-D 평가 | Safety, Completeness, Executability, Maintainability, Cost-awareness를 평가 축으로 제안한다. | 각 평가가 자동화 가능한지, human review가 필요한지 분리해야 한다. |
| Skill 과잉 | 검색/설치 가능한 skill이 많을수록 context noise와 activation 오류가 커질 수 있다. | SWE-Skills-Bench식 marginal utility 평가가 필요하다. |

## 5. SWE-Skills-Bench와 함께 읽는 이유

참고 링크: [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401), [Local Deep Dive](../SWE_Skills_Bench_Deep_Dive.html), [SWE GitHub](https://github.com/GeniusHTX/SWE-Skills-Bench)

SkillNet은 “스킬을 만들고 연결할 수 있다”는 공급 측 논리를 보여준다. SWE-Skills-Bench는 “그 스킬이 실제 software task에 도움이 되는지는 별도 검증해야 한다”는 수요 측 반론을 제공한다. 따라서 skill platform의 핵심 데이터 모델은 아래처럼 잡는 것이 맞다.

| 데이터 모델 | 필드 | 이유 |
| --- | --- | --- |
| Skill Card | name, source, owner, license, supported domain, input/output, tool requirement. | 설치 가능한 artifact로 관리하기 위해 필요하다. |
| Compatibility | language, framework, version, repository pattern, environment. | version mismatch로 인한 성능 저하를 막는다. |
| Eval Result | pass-rate delta, token delta, wall-clock delta, failure mode, test set. | measured utility를 판단한다. |
| Graph Edge | depends_on, compose_with, conflicts_with, replaces, similar_to. | planner가 skill을 조합하거나 제외할 수 있게 한다. |
| Governance | reviewer, approval status, retention, sensitive data flag, rollback version. | trace-to-skill과 enterprise 적용에 필요하다. |

## 6. Recommended Actions

1. SkillNet을 참고해 skill card, graph edge, eval result schema를 먼저 정의한다.
2. skill 생성보다 검증 pipeline을 먼저 만든다. 최소 기준은 skill on/off paired evaluation이다.
3. skill search는 popularity가 아니라 task fit, compatibility, measured utility 순으로 rank한다.
4. 자동 생성 skill은 source provenance, human review, rollback version 없이는 production 사용을 막는다.
5. platform roadmap에서는 “skill registry”와 “skill runtime activation”을 분리해 설계한다.
