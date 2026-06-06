# AgentBound Deep Dive

- 작성 시점: 2026-06-07 00:10 KST
- 조사 수준: Deep Dive
- 유형: Paper / Security / MCP permission boundary
- HTML 정본: `2026-06-07/AgentBound_Deep_Dive.html`

## 1. Executive Summary

참고 링크: [AgentBound PDF](https://programming-group.com/assets/pdf/papers/2026_AgentBound-Securing-Execution-Boundaries-of-AI-Agents.pdf), [FSE 2026](https://conf.researchr.org/details/fse-2026/fse-2026-research-papers/14/AgentBound-Securing-Execution-Boundaries-of-AI-Agents), [arXiv](https://arxiv.org/abs/2510.21236), [MCP Docs](https://modelcontextprotocol.io/)

AgentBound는 MCP 서버를 신뢰 기본값으로 실행하는 구조가 agent 보안의 핵심 위험이라는 점을 다룬 FSE 2026 논문이다. MCP/tool server는 agent의 손과 발이다. 외부 서버를 붙이는 순간 파일, 네트워크, secret, 데이터베이스, 결제성 액션이 하나의 실행면으로 합쳐지므로 permission manifest와 runtime enforcement를 platform baseline으로 둬야 한다.

핵심 판단:

- MCP/tool server registry는 단순 목록이 아니라 권한 선언, 정책 집행, 감사 체계가 되어야 한다.
- 논문은 296개 popular MCP server dataset, 80.9% manifest 자동 생성 정확도, 평균 0.6ms 수준의 제한적 enforcement overhead를 보고한다.
- agent 보안은 prompt safety보다 실행 경계와 최소 권한 모델이 먼저다.

## 2. What It Is

AgentBound는 MCP 서버용 access control framework를 제안한다. 서버가 필요한 접근 권한을 manifest로 선언하고, policy enforcement engine이 파일, 네트워크, secret, system resource 접근을 runtime에서 제한하는 구조다.

논문은 MCP가 agent와 외부 리소스를 연결하는 사실상의 표준으로 커지고 있지만, 서버 실행 권한은 여전히 신뢰 기반으로 처리되는 경우가 많다고 본다. AgentBound의 제안은 Android 앱 권한 모델에 가까운 capability declaration과 집행 체계를 MCP 서버에 적용하는 것이다.

## 3. Trend Signals

참고 링크: [Replication Package](https://zenodo.org/records/19468201), [arXiv](https://arxiv.org/abs/2510.21236)

| 신호 | 논문/생태계 근거 | 해석 |
| --- | --- | --- |
| 296개 MCP 서버 데이터셋 | 논문과 FSE 페이지는 popular MCP server 296개를 수집해 평가했다고 설명한다. | MCP 보안은 이론적 문제가 아니라 이미 충분한 생태계 표본이 있는 문제다. |
| 80.9% manifest 자동 생성 정확도 | 자동 생성된 access policy/manifest 중 상당수가 수정 없이 정확하다고 보고한다. | connector 등록 때 source scan 기반 권한 초안을 만들 수 있다. |
| 0.6ms 평균 overhead | 논문은 policy enforcement의 평균 overhead가 제한적이라고 보고한다. | 권한 집행은 UX/성능 희생이 커서 못 한다는 주장이 약해진다. |
| 공격 범주 | privilege escalation, data tampering, exfiltration, tool poisoning, rug pull 같은 공격을 다룬다. | agent 보안은 prompt safety만으로 충분하지 않고 실행 경계가 필요하다. |

## 4. Technical Architecture

| 구성요소 | 역할 | AI 플랫폼 설계 포인트 |
| --- | --- | --- |
| AgentManifest | MCP 서버가 필요한 capability를 선언. | 권한 선언을 connector 등록의 필수 필드로 만든다. |
| Policy Vocabulary | 파일, 네트워크, secret, system resource 등 접근 범주를 표준화. | 서비스별 resource taxonomy와 매핑한다. |
| Static Analyzer | 서버 source code에서 필요한 권한 후보를 추론. | 자동 manifest 초안과 reviewer workflow를 만든다. |
| Runtime Enforcement | 서버 실행 중 정책 위반 접근을 차단. | allow/deny, rate limit, egress rule, credential scope를 집행한다. |
| Audit Record | 실행한 tool, 접근 resource, 차단 이유를 기록. | 운영 감사, 장애 분석, 분쟁 대응의 근거가 된다. |

## 5. AI 플랫폼 및 서비스 적용 방향

참고 링크: [OpenFGA MCP Authorization](https://openfga.dev/docs/modeling/agents/mcp-authorization), [MCP Permission Commentary](https://mcpblog.dev/blog/2026-03-21-chmod-ai-agents-mcp-permissions), [Community Signal](https://www.reddit.com/r/AI_Agents/comments/1rueo15/nobody_is_asking_where_mcp_servers_get_their_data/)

| 필수 필드 | 설명 | 예시 |
| --- | --- | --- |
| `declared_capabilities` | 서버가 요구하는 권한 범주. | filesystem read, outbound network, secret read, DB query. |
| `runtime_policy` | 허용 endpoint, path, credential scope, data egress rule. | 특정 API host만 허용, user-scoped token만 사용. |
| `risk_tier` | 읽기, 쓰기, 외부 액션, 개인정보, 결제성 액션 위험도. | read-only, write, external action, sensitive action. |
| `approval_policy` | 자동 실행 가능 여부와 승인 주체. | low-risk auto, high-risk user approval, admin approval. |
| `audit_schema` | 어떤 action, payload, resource, result를 남길지. | tool name, input hash, output hash, resource id, deny reason. |

## 6. Recommended Actions

1. 모든 MCP/tool connector에 manifest 제출을 요구하는 registry draft를 만든다.
2. 파일, 네트워크, secret, 데이터, 결제성 액션을 최소 권한 taxonomy로 분류한다.
3. source scan 기반 manifest 자동 생성과 reviewer approval workflow를 붙인다.
4. high-risk tool call은 user approval, admin approval, two-step confirmation 중 하나를 요구한다.
5. agent trace와 tool audit log를 같은 run id로 묶어 사후 분석이 가능하게 한다.
