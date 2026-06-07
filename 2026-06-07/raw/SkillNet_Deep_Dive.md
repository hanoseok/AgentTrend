# SkillNet Deep Dive

- 작성 시점: 2026-06-07 00:27 KST
- 조사 수준: Expanded Deep Dive
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

## 6. Skill Supply Pipeline

참고 링크: [SkillNet arXiv](https://arxiv.org/abs/2603.04448), [GitHub CLI](https://github.com/zjunlp/SkillNet), [Project Site](https://skillnet.openkg.cn)

SkillNet은 skill을 단순 문서 저장소가 아니라 공급망으로 본다. skill은 생성, 검색, 설치, 평가, 관계 분석, 재평가를 거쳐야 agent runtime에서 안전하게 활성화될 수 있다. 이 파이프라인이 없으면 skill repository는 빠르게 품질이 낮은 prompt fragment 저장소로 변한다.

| 단계 | SkillNet 신호 | 플랫폼 요구사항 |
| --- | --- | --- |
| Source Ingestion | repo, document, trajectory, prompt 등 heterogeneous source에서 skill 생성 | source provenance, license, data sensitivity를 먼저 기록 |
| Skill Generation | CLI와 toolkit으로 create workflow 제공 | 자동 생성 skill은 human review 전 quarantine 상태 |
| Search / Retrieval | keyword와 semantic search 지원 | task intent, compatibility, measured utility를 ranking feature로 사용 |
| Evaluation | Safety, Completeness, Executability, Maintainability, Cost-awareness 5-D 평가 | 자동 점수와 task-level empirical eval을 분리 |
| Graph Analysis | skill 관계와 조합 가능성을 분석 | compose_with, conflicts_with, depends_on edge를 runtime planner에 제공 |
| Runtime Activation | agent가 필요 skill을 검색/설치/사용할 수 있는 구조 | 기본 주입 대신 just-in-time activation과 token budget 적용 |

## 7. Skill Data Model

SkillNet과 SWE-Skills-Bench를 함께 보면, AI 플랫폼의 skill data model은 최소한 아래 네 가지를 동시에 가져야 한다. 특히 graph edge와 eval result가 분리되어야 “연결되어 보이는 skill”과 “실제로 도움이 되는 skill”을 구분할 수 있다.

| 객체 | 핵심 필드 | 설계 이유 |
| --- | --- | --- |
| Skill Source | origin URL, author, license, created_at, source_type, sensitive_data_flag | 출처와 권리, 보안 위험을 추적 |
| Skill Card | task_scope, input/output, required_tools, supported_versions, examples | agent와 사람이 적용 범위를 이해할 수 있게 함 |
| Quality Evaluation | safety_score, executable_score, maintainability_score, cost_score, reviewer | 등록 심사와 재평가 기준 제공 |
| Empirical Utility | pass-rate delta, token delta, latency delta, failure cases, test_set_id | 실제 task 개선 여부를 수치로 관리 |
| Graph Edge | depends_on, compose_with, conflicts_with, replaces, similar_to | workflow planner가 조합과 배제를 판단 |
| Runtime Policy | activation_rule, max_tokens, max_tools, risk_tier, approval_required | skill 활성화가 비용과 권한을 통제하도록 만듦 |

## 8. Research Agenda and Caveats

참고 링크: [SWE-Skills-Bench Local](../SWE_Skills_Bench_Deep_Dive.html), [COLLEAGUE.SKILL Local](../COLLEAGUE_SKILL_Deep_Dive.html)

SkillNet은 agent skill의 공급 측 청사진으로 매우 중요하지만, 실제 업무 플랫폼에 적용하려면 아직 풀어야 할 연구 문제가 많다. 특히 benchmark 환경의 reward 향상이 실제 운영 업무의 품질 향상으로 곧장 이어진다고 보면 안 된다.

| 연구 질문 | 왜 중요한가 | 검증 방법 |
| --- | --- | --- |
| Transferability | 한 benchmark나 repo에서 효과가 있던 skill이 다른 업무에도 통하는가? | domain별 holdout task와 cross-repo evaluation |
| Compositional Validity | 두 skill을 합치면 실제로 더 좋아지는가, 아니면 충돌하는가? | compose_with edge별 paired evaluation과 conflict detection |
| Cost-aware Activation | skill이 성능을 조금 올리지만 비용을 크게 늘리면 쓸 가치가 있는가? | cost-adjusted utility score와 budget-aware retrieval |
| Safety of Auto-created Skills | 자동 생성 skill이 민감정보, 잘못된 절차, 오래된 지식을 포함할 수 있음 | source audit, PII scan, human review, rollback |
| Skill Decay | framework, API, 정책이 바뀌면 skill이 빠르게 낡음 | freshness badge, dependency monitor, scheduled re-eval |

## 9. Recommended Actions

1. SkillNet을 참고해 skill card, graph edge, eval result schema를 먼저 정의한다.
2. skill 생성보다 검증 pipeline을 먼저 만든다. 최소 기준은 skill on/off paired evaluation이다.
3. skill search는 popularity가 아니라 task fit, compatibility, measured utility 순으로 rank한다.
4. 자동 생성 skill은 source provenance, human review, rollback version 없이는 production 사용을 막는다.
5. platform roadmap에서는 “skill registry”와 “skill runtime activation”을 분리해 설계한다.
6. SkillNet식 5-D quality score와 SWE-Skills-Bench식 empirical utility를 모두 저장한다.
7. skill graph에는 도움이 되는 관계뿐 아니라 conflicts_with와 replaces 같은 배제 관계도 포함한다.

## 출처

### 왜 이걸 정리하게 되었는가

- [SkillNet arXiv](https://arxiv.org/abs/2603.04448)
- [SkillNet GitHub](https://github.com/zjunlp/SkillNet)
- [Project Site](https://skillnet.openkg.cn)
- [Hugging Face Blog](https://huggingface.co/blog/xzwnlp/skillnet)
- [SWE-Skills-Bench](https://arxiv.org/abs/2603.15401)

### 딥리서치 출처

- [SkillNet arXiv](https://arxiv.org/abs/2603.04448)
- [SkillNet GitHub](https://github.com/zjunlp/SkillNet)
- [Project Site](https://skillnet.openkg.cn)
- [Hugging Face Blog](https://huggingface.co/blog/xzwnlp/skillnet)
- [SWE-Skills-Bench GitHub](https://github.com/GeniusHTX/SWE-Skills-Bench)
