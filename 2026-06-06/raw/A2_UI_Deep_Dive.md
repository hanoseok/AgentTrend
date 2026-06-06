# A2-UI: Agent-to-User Interface Deep Dive

- 작성 시점: 2026-06-06 20:18 KST
- 조사 수준: Deep Dive
- HTML 정본: `2026-06-06/A2_UI_Deep_Dive.html`

## 1. Executive Summary

참고 링크: [A2UI Docs](https://a2ui.org/), [What is A2UI](https://a2ui.org/introduction/what-is-a2ui/), [GitHub](https://github.com/a2ui-project/a2ui)

A2-UI는 agent가 사용자에게 보여줄 UI를 안전한 선언형 JSON으로 생성하는 generative UI protocol/spec이다. 핵심은 LLM이 HTML/JavaScript를 실행하게 하는 것이 아니라, agent가 허용된 component catalog 안에서 UI 의도를 보내고 client가 native UI로 렌더링하게 하는 것이다.

브리핑 한 줄:

> A2-UI는 agent가 “텍스트로 설명”하는 대신 “사용자가 바로 조작할 수 있는 안전한 UI”를 만들어내는 표준 방향이다.

핵심:

- A2-UI는 agent-generated UI payload/spec이다.
- AG-UI는 agent와 frontend 사이의 event channel이고, A2-UI는 그 안에 실을 수 있는 UI description이다.
- 대화형 AI 서비스 말풍선 안에서 쇼핑, 예약, 결제 승인, 비교표, 배송지 입력 같은 UI를 agent가 동적으로 제안하게 만들 수 있다.

## 2. What It Is

참고 링크: [Core Concepts](https://a2ui.org/concepts/overview/), [v0.10 protocol](https://a2ui.org/specification/v0_10/docs/a2ui_protocol/), [AG2 integration](https://docs.ag2.ai/latest/docs/user-guide/a2a/a2ui/)

A2UI 공식 문서는 이를 agent-driven interfaces를 위한 declarative UI protocol이라고 설명한다. Remote agent나 multi-agent system에서는 agent가 사용자의 앱 UI를 직접 조작할 수 없고, raw HTML/JavaScript를 보내면 보안, 스타일, 접근성 문제가 생긴다.

A2-UI는 UI를 code가 아니라 data로 보내게 한다.

Client는 자기가 신뢰하는 component catalog를 유지한다. Agent는 Text, Button, Row, Form, Input 같은 사전 승인된 component와 data binding/action만 요청한다. 앱은 이를 자기 디자인 시스템과 native widget으로 매핑한다.

## 3. Technical Model

참고 링크: [v0.9 protocol](https://a2ui.org/specification/v0_9/docs/a2ui_protocol/), [v0.10 protocol](https://a2ui.org/specification/v0_10/docs/a2ui_protocol/), [Spec folder](https://github.com/a2ui-project/a2ui/tree/main/specification)

- Surface: UI가 렌더링되는 독립 영역. chat response 하나, 카드 하나, checkout panel 하나가 surface가 될 수 있다.
- Component: Text, Button, Input, Row, Column 등 catalog에 정의된 구성요소. agent는 catalog 외 component를 쓸 수 없어야 한다.
- Data Model: UI state를 JSON-like data model로 관리하고 component property를 path로 binding한다.
- Action: 사용자 클릭/입력/제출이 client에서 server/agent로 전달되는 명령. 모든 action은 whitelist와 validation이 필요하다.
- Catalog: 조직별 component schema. AI 플랫폼은 서비스 디자인 시스템 catalog를 만들어야 한다.
- Transport: A2-UI는 transport-agnostic이다. A2A, AG-UI, SSE, WebSocket, MCP tool output 등으로 전달 가능하다.

### Protocol Operations

- `createSurface`: 새 UI surface를 만든다.
- `updateComponents`: flat component list와 adjacency reference를 갱신한다.
- `updateDataModel`: UI state/data binding 값을 바꾼다.
- `deleteSurface`: surface를 제거한다.
- Streaming/progressive rendering을 전제로 component와 data가 순차적으로 도착할 수 있다.

## 4. Version Status / Trend Signal

참고 링크: [v0.8](https://a2ui.org/specification/v0.8-a2ui/), [v0.9](https://a2ui.org/specification/v0_9/docs/a2ui_protocol/), [Macaron-A2UI](https://arxiv.org/abs/2605.24830)

A2-UI는 빠르게 움직이는 early-stage 표준이다.

- 공식 사이트는 v0.8 stable, v0.9 current를 표시한다.
- v0.10 protocol docs도 공개되어 있다.
- GitHub README에는 public preview/expect changes 성격이 강하게 남아 있다.
- 브리핑에서는 “유력한 방향이지만 pre-v1로 빠르게 변경 중”이라고 표현하는 것이 안전하다.

중요한 추가 신호는 2026-05-24 제출된 Macaron-A2UI 논문이다. 이 논문은 personal agent가 text-only chat을 넘어 generative UI action을 생성하는 모델과 benchmark를 제안한다. 즉 A2-UI류 UI spec은 단순 프론트엔드 기술이 아니라 모델 학습/평가 영역으로도 이동 중이다.

## 5. AI Platform Implications

| Use Case | A2-UI 적용 | 필요 조건 |
| --- | --- | --- |
| 커머스 agent | 추천 상품 카드, 옵션 선택, 배송지 확인, 메시지 작성 UI를 agent가 생성 | commerce component catalog, price/stock validation, 결제 approval 분리 |
| 예약 / 로컬 agent | 날짜/시간/인원/장소 선택 form을 말풍선 안에 생성 | 지도/예약 API action whitelist, cancellation policy 표시 |
| CS agent | 문제 유형 선택, 증빙 업로드, 환불/교환 옵션 UI를 동적으로 구성 | 개인정보 필드 제한, 상담원 escalation action |
| 사내 업무 agent | 승인 dashboard, incident checklist, 데이터 분석 결과 card를 생성 | 권한별 component exposure, audit log, role-based action control |

AI 플랫폼 설계 원칙:

- Agent는 UI code를 만들지 않고, catalog에 허용된 UI intent만 만든다.
- 모든 action은 server-side policy와 user permission을 다시 검증한다.
- payment, 개인정보, 외부 발송은 A2-UI component에서 바로 처리하지 않고 AG-UI interrupt + trusted surface로 넘긴다.
- 대화형 AI 서비스 mobile/web/native renderer가 동일한 payload를 각 환경의 native component로 렌더링해야 한다.

## 6. Recommended Actions

1. AI 서비스 A2-UI Catalog: Text, Button, ProductCard, OptionSelector, DateTimePicker, AddressSelector, ApprovalSummary 등 기본 catalog를 정의한다.
2. Renderer PoC: 대화형 AI 서비스 웹/모바일 중 하나에서 A2-UI payload를 native UI로 렌더링하는 PoC를 만든다.
3. Validator: agent output이 catalog schema, child reference, data binding, action whitelist를 통과해야만 렌더링되게 한다.
4. AG-UI Transport 결합: A2-UI payload는 AG-UI event stream 안에서 surface update로 전달한다.
5. Security Review: phishing UI, deceptive button, hidden data exfiltration, malicious action injection, accessibility failure를 테스트한다.

## 7. Sources

### Official / Specification

- A2UI official site: https://a2ui.org/
- What is A2UI?: https://a2ui.org/introduction/what-is-a2ui/
- A2UI Core Concepts: https://a2ui.org/concepts/overview/
- A2UI v0.8 protocol docs: https://a2ui.org/specification/v0.8-a2ui/
- A2UI v0.9 protocol docs: https://a2ui.org/specification/v0_9/docs/a2ui_protocol/
- A2UI v0.10 protocol docs: https://a2ui.org/specification/v0_10/docs/a2ui_protocol/
- A2UI GitHub repository: https://github.com/a2ui-project/a2ui
- A2UI GitHub specification folder: https://github.com/a2ui-project/a2ui/tree/main/specification

### Implementations / Ecosystem

- AG2 A2UI integration docs: https://docs.ag2.ai/latest/docs/user-guide/a2a/a2ui/
- AG2 A2UI reference agent: https://docs.ag2.ai/latest/docs/user-guide/reference-agents/a2uiagent/
- A2UI Composer demo: https://a2ui-composer.ag-ui.com/
- CopilotKit - AG-UI and A2UI explained: https://www.copilotkit.ai/docs/AG-UI-and-A2UI-Explained.pdf
- CopilotKit - A2UI and AG-UI walkthrough: https://www.copilotkit.ai/blog/build-with-googles-new-a2ui-spec-agent-user-interfaces-with-a2ui-ag-ui

### Research / Related

- Macaron-A2UI paper: https://arxiv.org/abs/2605.24830
- AG-UI docs for transport relationship: https://docs.ag-ui.com/
