# AG-UI: Agent-User Interaction Protocol Deep Dive

- 작성 시점: 2026-06-06 20:18 KST
- 조사 수준: Deep Dive
- HTML 정본: `2026-06-06/AG_UI_Deep_Dive.html`

## 1. Executive Summary

참고 링크: [AG-UI Docs](https://docs.ag-ui.com/), [Overview](https://docs.ag-ui.com/introduction), [GitHub](https://github.com/ag-ui-protocol/ag-ui)

AG-UI는 에이전트 백엔드와 사용자 앱 프론트엔드를 연결하는 이벤트 기반 상호작용 프로토콜이다. 에이전트가 장시간 실행되고, 도구를 쓰고, 상태를 바꾸고, 중간 승인을 요구하는 제품에서는 채팅 API만으로 부족하다. AG-UI는 그 빈 공간을 메운다.

브리핑 한 줄:

> AG-UI는 “에이전트가 앱 안에서 일하는 모습을 사용자에게 안전하고 추적 가능하게 보여주는 UX 프로토콜”이다.

핵심:

- AG-UI는 UI component spec이 아니라 event transport/interaction protocol이다.
- agent state, tool call, streaming text, interrupt, shared state를 앱에 전달한다.
- MCP가 tools/data, A2A가 agent-to-agent를 다룬다면 AG-UI는 agent-to-user-facing-app을 다룬다.
- A2-UI와는 경쟁이 아니라 조합 관계다.

## 2. What It Is

참고 링크: [Introduction](https://docs.ag-ui.com/introduction), [A2UI relation](https://a2ui.org/introduction/what-is-a2ui/)

AG-UI 공식 문서는 이를 AI agent와 user-facing application을 연결하는 open, lightweight, event-based protocol로 정의한다.

구분:

- MCP: agent에게 tools/data를 준다.
- A2A: agent 간 협업을 다룬다.
- AG-UI: agent와 사용자 앱 사이의 실시간 상호작용을 다룬다.
- A2-UI: agent가 전달할 수 있는 선언형 UI payload/spec이다.

## 3. Technical Model

참고 링크: [Events](https://docs.ag-ui.com/concepts/events), [State](https://docs.ag-ui.com/concepts/state.md), [Interrupts](https://docs.ag-ui.com/concepts/interrupts), [Tools](https://docs.ag-ui.com/concepts/tools)

### Event Categories

| 카테고리 | 의미 | AI 플랫폼 UX 예 |
| --- | --- | --- |
| Lifecycle | RunStarted, StepStarted, RunFinished, RunError 등 agent run의 경계를 표현 | “예약 가능 매장 검색 중”, “결제 조건 검증 중” 진행 표시 |
| Text Message | 텍스트 메시지 streaming start/content/end | 채팅 UI이 실시간으로 생성됨 |
| Tool Call | agent가 어떤 tool을 어떤 인자로 호출하는지 프론트에 노출 | “지도 검색”, “상품 재고 조회”, “쿠폰 적용” 표시 |
| State | StateSnapshot/StateDelta로 agent와 UI state를 동기화 | 장바구니, 예약 조건, 배송지, 선택 옵션 동기화 |
| Activity | 계획, 검색, 분석 같은 중간 활동을 구조화 | CS agent가 문의 원인 분석 단계를 보여줌 |
| Interrupt | 사용자 승인/수정/재시도/에스컬레이션을 위해 run을 멈춤 | 결제, 개인정보 제공, 외부 전송 전에 승인 요청 |
| Custom / Raw | 앱별 이벤트와 외부 시스템 이벤트를 담는 확장 지점 | 대화형 AI 서비스 채널, 알림, 지도, 결제 이벤트를 감쌈 |

### 주요 기능

- Streaming chat, tool result streaming, cancel/resume.
- Shared state와 JSON Patch 기반 incremental update.
- Frontend tool calls와 backend tool rendering.
- Human-in-the-loop interrupt.
- Sub-agent composition, scoped tracing, cancellation.
- Reasoning visibility는 raw chain-of-thought가 아니라 summaries/traces 중심.

## 4. Ecosystem / Trend Signal

참고 링크: [Dojo examples](https://dojo.ag-ui.com/), [AG-UI/A2UI explainer](https://www.copilotkit.ai/docs/AG-UI-and-A2UI-Explained.pdf), [CopilotKit walkthrough](https://www.copilotkit.ai/blog/build-with-googles-new-a2ui-spec-agent-user-interfaces-with-a2ui-ag-ui)

AG-UI는 CopilotKit, LangGraph, CrewAI와의 초기 연동에서 출발했지만 현재 문서와 GitHub는 Microsoft Agent Framework, Google ADK, AWS Strands/Bedrock, Mastra, Pydantic AI, Agno, LlamaIndex, AG2 등 다양한 agent framework와의 연동을 내세운다.

판단:

- AG-UI는 아직 모든 플랫폼이 채택한 확정 표준은 아니다.
- 하지만 agent 앱을 실제 제품으로 만들 때 필요한 event taxonomy가 상당히 실무적이다.
- AI 플랫폼은 그대로 종속되기보다 AG-UI-compatible internal event schema를 만드는 것이 현실적이다.

## 5. AI Platform Implications

| AI 서비스 적용 영역 | AG-UI가 해결하는 문제 | 필요 설계 |
| --- | --- | --- |
| 대화형 AI 서비스 / 대화형 agent | 말풍선만으로 장시간 task, tool call, approval 상태를 표현하기 어렵다 | thread/run/step/event id 체계, resumable run, cancel UI |
| 커머스 / 결제 | 상품 비교, 옵션 선택, 결제 승인, 주문 추적이 모두 다른 UI 상태다 | StateSnapshot/Delta + Interrupt + Trusted Surface event |
| CS / 운영 agent | agent가 어떤 근거와 tool로 판단했는지 추적해야 한다 | ToolCall events, activity trace, human escalation |
| 개발자/사내 agent | 장시간 workflow 진행 상황과 subagent 작업을 보여줘야 한다 | Run/Step lifecycle, nested delegation trace, log streaming |

## 6. Recommended Actions

1. AI 서비스 Agent Event Schema: AG-UI event taxonomy를 참고해 내부 공통 event envelope를 만든다.
2. Interrupt First: 결제, 개인정보 제공, 외부 메시지 발송, 예약 확정은 모두 interrupt로 모델링한다.
3. Trace Policy: raw chain-of-thought 없이 tool call, decision summary, evidence, approval record를 남긴다.
4. AG-UI + A2-UI PoC: AG-UI transport 위에 A2-UI payload를 얹어 쇼핑/예약 UI를 동적으로 렌더링한다.
5. Mobile Constraints: 모바일 대화형 서비스 환경에서 event 폭주, reconnect, offline, push notification, partial rendering을 테스트한다.

## 7. Sources

### Official / Specification

- AG-UI official documentation: https://docs.ag-ui.com/
- AG-UI Overview: https://docs.ag-ui.com/introduction
- AG-UI Events: https://docs.ag-ui.com/concepts/events
- AG-UI State Management: https://docs.ag-ui.com/concepts/state.md
- AG-UI Interrupts: https://docs.ag-ui.com/concepts/interrupts
- AG-UI Tools: https://docs.ag-ui.com/concepts/tools
- AG-UI Generative UI: https://docs.ag-ui.com/concepts/generative-ui-specs.md
- AG-UI Reasoning: https://docs.ag-ui.com/concepts/reasoning
- AG-UI GitHub repository: https://github.com/ag-ui-protocol/ag-ui
- AG-UI TypeScript SDK overview: https://docs.ag-ui.com/sdk/js/core/overview
- AG-UI Python SDK overview: https://docs.ag-ui.com/sdk/python/core/overview

### Ecosystem / Demos

- AG-UI Dojo examples: https://dojo.ag-ui.com/
- CopilotKit - AG-UI and A2UI explained: https://www.copilotkit.ai/docs/AG-UI-and-A2UI-Explained.pdf
- CopilotKit - Building with A2UI and AG-UI: https://www.copilotkit.ai/blog/build-with-googles-new-a2ui-spec-agent-user-interfaces-with-a2ui-ag-ui
- A2UI explanation of AG-UI transport relationship: https://a2ui.org/introduction/what-is-a2ui/
