# A2A Deep Dive

- 검증 시점: 2026-06-06 18:45 KST
- 주제: Agent2Agent Protocol, A2A
- 목적: AI 플랫폼 및 서비스 설계에 필요한 A2A의 의미, 구조, 리스크, 적용 포인트 정리

## 0. 브리핑 핵심 판단

참고 링크: [Google A2A 발표](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/), [Linux Foundation 이관](https://www.linuxfoundation.org/press/linux-foundation-launches-the-agent2agent-protocol-project-to-enable-secure-intelligent-communication-between-ai-agents), [A2A v1.0](https://a2a-protocol.org/latest/announcing-1.0/)

### 결론

A2A는 에이전트 시대의 "서비스 간 협업 프로토콜" 후보이며, AI 플랫폼이 AI 서비스와 에이전트 플랫폼을 만들 때 내부 표준 설계에 반영해야 할 가능성이 높다. 지금 당장 A2A를 전면 채택한다기보다, 내부 Agent Registry, Task 상태 모델, Artifact schema, 인증/인가, audit trace를 A2A-compatible하게 설계하는 것이 전략적으로 안전하다.

### 왜 중요한가

- 시장 신호: Google 발표 이후 Linux Foundation 프로젝트로 이동했고, 2026년 기준 주요 기업과 클라우드 플랫폼 지원이 확대되고 있다.
- 기술 신호: MCP가 agent-to-tool 표준 후보라면, A2A는 agent-to-agent 표준 후보로 자리 잡고 있다.
- AI 플랫폼 신호: 대화형 AI 서비스, 쇼핑/커머스, 로컬/지도, 결제, 일정, 고객센터가 각각 독립 에이전트화될 경우 에이전트 간 위임과 상태 추적 표준이 필요하다.
- 리스크 신호: 에이전트 간 위임은 개인정보, 결제, 예약, 메시지 발송 같은 고위험 액션을 확장하므로 보안/인가/감사 체계를 먼저 설계해야 한다.

### AI 플랫폼 의사결정 포인트

1. 내부 에이전트 플랫폼의 기본 계약을 A2A-compatible하게 잡을 것인가.
2. 서비스별 Agent Card와 Agent Registry를 중앙 플랫폼 기능으로 제공할 것인가.
3. 고위험 액션에 대해 human approval, per-skill authorization, delegated token을 공통 표준으로 둘 것인가.
4. MCP와 A2A를 결합한 구조를 내부 AI 플랫폼 SDK에 포함할 것인가.
5. 대화형 AI 서비스 Orchestrator를 중심으로 전문 에이전트 협업 PoC를 언제 시작할 것인가.

### 권장 액션

- 즉시: A2A 스펙을 기준으로 내부 Agent Card, Task, Artifact 초안을 만든다.
- 30일 내: 검색/문서, 일정, 쇼핑 추천 중 2~3개 도메인 에이전트로 A2A-compatible PoC를 만든다.
- 병행: signed Agent Card, per-skill authorization, trace replay, human approval을 보안 baseline으로 정의한다.
- 계속 추적: A2A v1.x 변경, 주요 벤더 SDK 지원, MCP와 A2A 결합 사례, production 보안 사고를 추적한다.

## 1. 핵심 요약

A2A는 서로 다른 에이전트가 능력을 공개하고, 상대를 발견하고, 작업을 위임하고, 진행 상태와 결과물을 주고받기 위한 공개 프로토콜이다. MCP가 에이전트와 도구, API, 데이터 소스를 연결하는 쪽에 가깝다면, A2A는 에이전트와 에이전트를 연결하는 계층이다.

AI 플랫폼 관점에서 A2A의 핵심 가치는 특정 모델이나 특정 벤더에 묶이지 않는 멀티 에이전트 플랫폼 계약을 만들 수 있다는 점이다. 예를 들어 대화형 AI 서비스 계열의 대화 에이전트가 쇼핑, 로컬, 커머스, 예약, 결제, 고객센터, 사내 업무 에이전트에게 작업을 위임하고, 각 에이전트가 상태와 결과물을 표준 형태로 돌려주는 구조를 만들 수 있다.

가장 중요한 설계 포인트는 Agent Card, Task lifecycle, Artifact, 인증/인가, 추적성이다. 특히 사용자 데이터와 결제, 예약, 메시징처럼 민감한 AI 서비스에서는 단순한 에이전트 호출보다 per-skill authorization, human approval, delegation trace, policy enforcement가 먼저 설계되어야 한다.

## 2. 현재 상태와 타임라인

참고 링크: [2025 Google 발표](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/), [2026 LF 업데이트](https://www.linuxfoundation.org/press/a2a-protocol-surpasses-150-organizations-lands-in-major-cloud-platforms-and-sees-enterprise-production-use-in-first-year), [v1.0 발표](https://a2a-protocol.org/latest/announcing-1.0/)

- 2025-04-09: Google이 A2A를 공개하고 50개 이상의 기술 파트너와 함께 에이전트 상호운용성 표준으로 발표했다.
- 2025-06-23: Linux Foundation이 A2A Protocol Project를 출범시켰다. 프로젝트가 특정 벤더 주도에서 더 넓은 오픈 거버넌스 구조로 이동한 것이 중요하다.
- 2026년 기준: Linux Foundation 발표에 따르면 A2A는 150개 이상의 조직 지원, 주요 클라우드 플랫폼 통합, 엔터프라이즈 프로덕션 사용 사례를 확보한 것으로 소개된다.
- A2A v1.0: 안정 버전으로 발표되었고, multi-protocol binding, version negotiation, signed Agent Card, enterprise security 기능이 강조된다.

판단: A2A는 아직 모든 산업의 사실상 표준이라고 보기에는 이르지만, MCP와 함께 에이전트 플랫폼의 표준 인터페이스 후보로 반드시 추적해야 한다. AI 플랫폼은 내부 표준을 폐쇄적으로 만들기보다 A2A 호환성을 열어두는 편이 전략적으로 유리하다.

## 3. A2A와 MCP의 차이

참고 링크: [A2A and MCP](https://a2a-protocol.org/latest/topics/a2a-and-mcp/), [MCP Docs](https://modelcontextprotocol.io/)

MCP는 주로 에이전트가 외부 도구, 데이터, API에 접근하기 위한 표준이다. 예를 들어 캘린더 조회, CRM 검색, 문서 검색, 내부 DB 질의, 결제 API 호출 같은 도구 접근을 표준화한다.

A2A는 에이전트 간 협업을 표준화한다. 한 에이전트가 다른 에이전트의 능력을 발견하고, 작업을 위임하고, 상태를 추적하고, 결과물을 받는 구조다.

실제 플랫폼에서는 둘을 같이 쓰게 될 가능성이 높다.

- 사용자-facing 오케스트레이터: 사용자 의도 이해, 플래닝, 라우팅
- A2A: 전문 에이전트 간 위임과 협업
- MCP: 각 전문 에이전트가 도구, API, 데이터 소스에 접근

AI 플랫폼 예시:

- 대화형 AI 서비스 Orchestrator가 "이번 주말 가족 식사 예약하고 선물도 추천해줘"라는 요청을 받는다.
- A2A로 로컬/예약 에이전트, 커머스 에이전트, 결제 에이전트에게 작업을 나눈다.
- 각 에이전트는 MCP 또는 내부 API 게이트웨이를 통해 실제 서비스 데이터와 액션에 접근한다.
- 최종 응답은 오케스트레이터가 사용자에게 합성해서 제공한다.

## 4. 핵심 구성요소

참고 링크: [Key Concepts](https://a2a-protocol.org/latest/topics/key-concepts/), [Agent Discovery](https://a2a-protocol.org/latest/topics/agent-discovery/), [Specification](https://a2a-protocol.org/dev/specification/)

### Agent Card

Agent Card는 원격 에이전트의 공개 명세다. 에이전트의 이름, 설명, 엔드포인트, 지원 기능, 입출력 형식, 인증 요구사항, 스킬 정보를 담는다. A2A에서 discovery의 중심이다.

AI 플랫폼에서는 Agent Card를 내부 Agent Registry의 기본 단위로 삼는 것이 좋다. 단, 전체 정보를 공개하지 않고 public card와 extended/private card를 분리해야 한다. 예를 들어 외부 공개에는 "예약 가능"만 보이고, 내부 확장 카드에는 사용 가능한 지역, 결제 권한, 개인정보 접근 범위, SLA, 승인 정책이 포함될 수 있다.

### A2A Client와 A2A Server

A2A Client는 작업을 위임하는 쪽이다. A2A Server 또는 Remote Agent는 작업을 수행하는 쪽이다. 중요한 점은 A2A가 에이전트 내부 구현, 모델 종류, 프롬프트, 툴체인을 강제하지 않는다는 것이다. 외부에서 보이는 계약만 표준화한다.

### Message, Part, Artifact

Message는 에이전트 사이에서 오가는 대화 또는 작업 요청 단위다. Part는 텍스트, 파일, 구조화 데이터 등 메시지를 이루는 조각이다. Artifact는 작업 결과물이다. 보고서, 추천 리스트, 예약 후보, 결제 요청, 생성 이미지, 코드 패치 등이 Artifact가 될 수 있다.

AI 서비스에서는 Artifact를 단순 텍스트가 아니라 UI와 액션으로 연결되는 구조화 결과물로 보는 것이 중요하다. 예를 들어 "선물 추천 결과"는 텍스트 답변이 아니라 상품 ID, 가격, 재고, 배송 가능일, 추천 이유, 구매 승인 필요 여부를 가진 구조화 Artifact여야 한다.

### Task lifecycle

A2A는 작업을 상태 기반으로 관리한다. 주요 상태는 submitted, working, input_required, auth_required, completed, failed, canceled, rejected 등이다.

이 상태 모델은 AI 플랫폼에 특히 중요하다. 실제 서비스에서는 에이전트가 바로 실행할 수 없는 경우가 많다.

- input_required: 사용자의 추가 정보가 필요함
- auth_required: 인증 또는 권한 승인이 필요함
- rejected: 정책, 안전성, 권한 부족으로 거부됨
- failed: 외부 서비스 장애 또는 실행 실패
- completed: 결과 Artifact 생성 완료

이 상태를 내부 워크플로우와 잘 맞추면 사용자 경험도 좋아지고 감사 추적도 쉬워진다.

## 5. 프로토콜 흐름

기본 흐름은 다음과 같다.

1. Client agent가 Agent Registry 또는 공개 URL에서 Agent Card를 찾는다.
2. Card의 기능, 인증 요구사항, 입출력 형식을 확인한다.
3. 적절한 remote agent를 선택한다.
4. message:send 또는 streaming 방식으로 작업을 보낸다.
5. remote agent는 Message 또는 Task를 생성하고 상태를 갱신한다.
6. client는 polling, server-sent events, webhook 등으로 진행 상황을 받는다.
7. 작업이 끝나면 Artifact를 받는다.

중요한 설계 판단:

- discovery와 execution을 분리해야 한다.
- 에이전트 선택 로직은 모델 프롬프트에만 맡기지 말고 policy/ranking/eval 결과를 함께 써야 한다.
- Task와 Artifact에는 traceId, contextId, userId/tenantId, approval state가 붙어야 한다.
- 실패와 중단, 사용자 추가 입력 요청을 정상 경로로 설계해야 한다.

## 6. 보안과 거버넌스

참고 링크: [Enterprise Ready](https://a2a-protocol.org/latest/topics/enterprise-ready/), [Signed Agent Card](https://a2a-protocol.org/latest/topics/agent-discovery/)

A2A는 인증과 인가를 프로토콜 자체의 마법으로 해결하지 않는다. HTTP, TLS, OAuth, OIDC, mTLS, API key 등 기존 엔터프라이즈 보안 메커니즘과 결합하는 방식에 가깝다. 따라서 플랫폼 구현자가 책임져야 할 영역이 크다.

AI 플랫폼이 특히 봐야 할 리스크:

- 잘못된 Agent Card 또는 악성 Agent Card 등록
- 에이전트 간 prompt injection 전파
- 사용자 권한을 초과한 delegated action
- 결제, 예약, 메시지 발송 같은 irreversible action의 자동 실행
- 개인정보 또는 대화 맥락의 과도한 전달
- 에이전트 결과의 품질과 출처 검증 부족
- 장애 발생 시 어느 에이전트가 어떤 결정을 했는지 추적 불가

필수 통제:

- Agent Registry 승인 절차
- Signed Agent Card와 발급자 검증
- per-skill authorization
- 최소 권한 delegation token
- human approval checkpoint
- action risk tiering
- audit log와 trace replay
- red-team/eval 기반 agent ranking
- prompt injection 방어와 data exfiltration 감지

## 7. AI 플랫폼 및 서비스 적용 방향

### 7.1 내부 Agent Registry

AI 플랫폼의 각 서비스 또는 도메인 에이전트를 Agent Card 기반으로 등록한다. 검색, 발견, 권한, SLA, 데이터 접근 범위, 정책 태그를 하나의 registry에서 관리한다.

우선순위가 높은 에이전트 후보:

- 대화형 AI 서비스 대화/메시징 에이전트
- 로컬/지도/예약 에이전트
- 쇼핑/커머스 에이전트
- 결제/정산 에이전트
- 일정/리마인더 에이전트
- 고객센터/CS 에이전트
- 사내 업무/문서 검색 에이전트

### 7.2 Orchestrator와 전문 에이전트 분리

사용자-facing 에이전트는 모든 기능을 직접 수행하지 않고 orchestration에 집중한다. 전문 에이전트는 자신의 도메인 기능, 정책, 데이터 접근, 액션 실행을 책임진다.

이 구조는 서비스 조직별 책임과도 잘 맞는다. 각 서비스 팀은 자신의 Agent Card와 실행 서버를 운영하고, 중앙 AI 플랫폼 팀은 discovery, policy, tracing, eval, SDK를 제공할 수 있다.

### 7.3 구조화 Artifact 중심 UX

에이전트 결과를 텍스트 답변으로만 다루면 서비스 연결성이 떨어진다. Artifact를 UI 컴포넌트와 액션으로 연결해야 한다.

예:

- 예약 후보 Artifact: 장소 ID, 시간, 인원, 가격, 취소 정책, 예약 가능 여부
- 선물 추천 Artifact: 상품 ID, 재고, 배송일, 예산, 추천 이유, 구매 버튼
- 결제 요청 Artifact: 금액, 수취인, 승인 필요 여부, 만료 시간
- 일정 Artifact: 일정 ID, 참석자, 충돌 여부, 수정 옵션

### 7.4 Human-in-the-loop

결제, 예약 확정, 메시지 발송, 개인정보 공유처럼 사용자에게 영향이 큰 액션은 자동 실행하지 않고 명시적 승인을 받아야 한다. A2A의 auth_required, input_required 상태를 UX에 자연스럽게 연결하는 것이 중요하다.

### 7.5 플랫폼 SDK

조직 내부 개발자가 쉽게 A2A-compatible agent를 만들 수 있도록 SDK와 템플릿을 제공해야 한다.

필수 SDK 기능:

- Agent Card 생성/검증
- Task 상태 관리
- Artifact schema 정의
- 인증/인가 연동
- traceId/contextId 자동 전파
- policy check hook
- eval/logging hook
- MCP 또는 내부 API gateway 연동

## 8. 지금 해야 할 일

1. A2A-compatible internal contract 초안 작성
   - Agent Card, Task, Message, Artifact, auth_required, input_required를 내부 표준으로 정의한다.

2. Agent Registry PoC
   - 3개 정도의 내부 에이전트로 시작한다. 예: 검색/문서, 일정, 쇼핑 추천.

3. Security baseline 설계
   - Signed card, per-skill auth, 최소 권한 token, human approval, audit log를 초기부터 포함한다.

4. Orchestrator PoC
   - 하나의 사용자 요청을 2개 이상의 전문 에이전트로 위임하고, 결과 Artifact를 합성하는 흐름을 검증한다.

5. Eval/observability 설계
   - 어떤 에이전트가 어떤 근거로 선택되었고, 실패했을 때 어느 단계에서 실패했는지 추적 가능해야 한다.

## 9. 계속 추적할 변화

- A2A v1.x 스펙 변경: Task 상태, Agent Card schema, security extension
- 주요 벤더 SDK 지원 현황: Google, Microsoft, AWS, Salesforce, ServiceNow, Atlassian, OpenAI 생태계와의 연결
- MCP와 A2A의 결합 패턴
- 엔터프라이즈 보안 사례: signed card, agent identity, delegated authorization
- agent commerce와 결제 승인 흐름
- production 사례에서 나온 장애, 보안 사고, 악성 agent discovery 이슈

## 10. 출처

- Google Developers Blog, A2A announcement: https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/
- Linux Foundation, A2A Protocol Project launch: https://www.linuxfoundation.org/press/linux-foundation-launches-the-agent2agent-protocol-project-to-enable-secure-intelligent-communication-between-ai-agents
- Linux Foundation, 2026 adoption update: https://www.linuxfoundation.org/press/a2a-protocol-surpasses-150-organizations-lands-in-major-cloud-platforms-and-sees-enterprise-production-use-in-first-year
- A2A Protocol, Key Concepts: https://a2a-protocol.org/latest/topics/key-concepts/
- A2A Protocol, Agent Discovery: https://a2a-protocol.org/latest/topics/agent-discovery/
- A2A Protocol, Specification: https://a2a-protocol.org/dev/specification/
- A2A Protocol, v1.0 announcement: https://a2a-protocol.org/latest/announcing-1.0/
- A2A Protocol, Enterprise Ready: https://a2a-protocol.org/latest/topics/enterprise-ready/
- A2A Protocol, A2A and MCP: https://a2a-protocol.org/latest/topics/a2a-and-mcp/
