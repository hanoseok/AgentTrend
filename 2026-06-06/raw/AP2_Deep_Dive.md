# AP2: Agent Payments Protocol Deep Dive

- 작성 시점: 2026-06-06 20:18 KST
- 조사 수준: Deep Dive
- HTML 정본: `2026-06-06/AP2_Deep_Dive.html`

## 1. Executive Summary

참고 링크: [AP2 Docs](https://ap2-protocol.org/), [AP2 GitHub](https://github.com/google-agentic-commerce/AP2), [FIDO](https://fidoalliance.org/fido-alliance-to-develop-standards-for-trusted-ai-agent-interactions/)

AP2는 agentic commerce에서 “사용자가 정말 이 결제를 허락했는가, merchant가 받은 요청은 실제 사용자 의도와 일치하는가, 나중에 분쟁이 나면 무엇을 증거로 삼는가”를 해결하려는 결제 trust layer다.

브리핑 한 줄:

> AP2는 agent가 돈을 쓰는 시대의 “전자서명 + 위임장 + 결제 증빙” 표준에 가깝다.

핵심:

- AP2는 상품 검색이나 checkout API가 아니라 결제 위임, 사용자 의도 증명, 분쟁 증거, agent key 신뢰를 다룬다.
- Checkout Mandate, Payment Mandate, Receipt, Verifiable Digital Credential, Trusted Surface, deterministic verification이 핵심이다.
- 결제 서비스가 agent 결제를 지원하려면 “LLM이 결제를 결정했다”가 아니라 “비-agent trusted surface가 구체적 조건을 사용자가 승인했다”는 증거가 필요하다.

## 2. What It Is

참고 링크: [AP2 v0.2 Spec](https://ap2-protocol.org/ap2/specification/), [UCP + AP2](https://ucp.dev/documentation/ucp-and-ap2/), [Google Cloud](https://cloud.google.com/blog/products/ai-machine-learning/announcing-agents-to-payments-ap2-protocol)

공식 문서 기준 AP2는 secure, reliable, interoperable agent commerce를 위한 open protocol이다. A2A와 UCP의 extension으로 제공되며, MCP 등 다른 tool stack과도 결합될 수 있다.

AP2가 해결하려는 문제:

- Authorization: 사용자가 어떤 범위로 agent에게 권한을 줬는가?
- Authenticity: merchant가 받은 cart와 결제 조건이 사용자 의도와 같은가?
- Accountability: 잘못된 거래가 발생하면 누가 어떤 증거로 책임을 판단하는가?

## 3. Technical Model

참고 링크: [AP2 Docs Repo](https://github.com/google-agentic-commerce/AP2/tree/main/docs), [Code Samples](https://github.com/google-agentic-commerce/AP2/tree/main/code/samples), [Zero-trust paper](https://arxiv.org/abs/2602.06345)

### Roles

| Role | 설명 | AI 플랫폼 대응 |
| --- | --- | --- |
| Shopping Agent | 상품 탐색, checkout 구성, 구매 실행을 수행하는 agent | 대화형 AI 서비스/대화형 쇼핑 agent |
| Credential Provider | payment credential 접근 권한을 검증하고 credential scope를 제한 | 결제 서비스, wallet, card vault |
| Merchant | checkout을 제공/완료하고 inventory, price, discount integrity를 검증 | 커머스, 쇼핑 merchant, 외부 파트너 |
| Merchant Payment Processor | payment credential이 checkout에 대해 승인됐는지 확인하고 결제를 처리 | PG/PSP 또는 결제 서비스 processor |
| Trusted Surface | 사용자 동의를 명확히 받고 mandate를 생성하는 신뢰 UI. AP2 spec상 non-agentic이어야 함 | 대화형 AI 서비스/결제 승인 UI 승인 화면 |

### Mandates

- Checkout Mandate: 무엇을 사는지, cart/price/terms가 무엇인지 묶는 증빙.
- Payment Mandate: 어떤 payment instrument로 어떤 조건까지 결제를 허용하는지 묶는 증빙.
- Open Mandate: human-not-present autonomous flow에서 예산, 카테고리, merchant, 기간 같은 제약을 사전에 부여.
- Closed Mandate: 특정 checkout과 특정 금액에 바인딩된 최종 승인.
- Receipt: checkout/payment 실행 후 분쟁 증거로 쓸 수 있는 결과 기록.

핵심 원칙:

- AP2는 agent를 신뢰하지 않는다.
- Trusted Surface와 deterministic verification code가 agent의 요청을 검증한다.

## 4. Main Flows

참고 링크: [Human-present sample](https://github.com/google-agentic-commerce/AP2/tree/main/code/samples/python/scenarios/a2a/human-present/cards), [Human-not-present sample](https://github.com/google-agentic-commerce/AP2/tree/main/code/samples/python/scenarios/a2a/human-not-present/cards), [x402 sample](https://github.com/google-agentic-commerce/AP2/tree/main/code/samples/python/scenarios/a2a/human-not-present/x402)

| Flow | 설명 | AI 플랫폼 UX 예 |
| --- | --- | --- |
| Human Present / Direct | 사용자가 checkout 내용을 보고 payment-service trusted surface에서 바로 승인한다 | 대화형 쇼핑 agent가 장바구니를 만들고 결제 승인 화면에서 사용자가 최종 결제 |
| Human Not Present / Autonomous | 사용자가 사전에 제약 조건이 있는 open mandate를 승인한다 | “이번 주 생필품 5만원 이하, 특정 제휴몰에서만 자동 구매” |
| Agent-to-Agent Delegation | Shopping Agent가 다른 agent에게 작업을 위임할 수 있으나, 결제 권한은 mandate와 agent key 제약으로 제한된다 | personal agent가 여행 예약 전문 agent에게 위임하되 payment mandate 범위만 허용 |

## 5. AI Platform Implications

결제 서비스에 필요한 설계:

- Trusted Surface: LLM이 생성한 화면이 아니라 결제 서비스가 제어하는 deterministic approval UI가 필요하다.
- Mandate Schema: merchant, amount, item hash, cart hash, delivery, expiry, retry policy, refund/cancel policy를 포함해야 한다.
- Agent Identity: 어떤 agent/provider/key가 위임받았는지 기록하고 폐기/정지 가능해야 한다.
- Audit Trail: 사용자 intent, checkout state, payment state, merchant response, receipt를 분쟁 대응 가능한 형태로 보관해야 한다.
- Policy Engine: 자동 결제 한도, 카테고리 제한, 미성년/고위험 상품 제한, 반복 결제 제한을 deterministic rule로 처리한다.

사업 기회:

AI 서비스가 먼저 확보할 수 있는 지점은 “대화형 서비스 안에서 안전하게 agent에게 결제 권한을 일부 맡기는 UX”다. 선물 추천, 장보기, 예약, 음식 주문, 정기 구매, 쿠폰 최적화 같은 flow에서 AP2식 mandate는 사용자 신뢰를 만드는 핵심 장치가 된다.

## 6. Recommended Actions

1. AP2-compatible mandate draft: 조직 내부용 Checkout Mandate와 Payment Mandate schema를 작성한다.
2. Trusted Surface prototype: 대화형 AI 서비스/결제 승인 UI에서 agent 결제 승인 화면을 분리해 PoC한다.
3. Dispute evidence model: 분쟁, 환불, 오주문, agent 오류 발생 시 필요한 evidence package를 정의한다.
4. Regulatory review: 전자금융, PG, 본인확인, 카드사/네트워크, 개인정보 위임 범위를 법무/보안/결제 조직과 함께 검토한다.
5. Risk simulation: replay attack, context-binding failure, amount manipulation, merchant-side cart mutation, prompt injection을 테스트한다.
