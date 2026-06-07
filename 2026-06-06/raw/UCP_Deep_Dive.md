# UCP: Universal Commerce Protocol Deep Dive

- 작성 시점: 2026-06-06 20:18 KST
- 조사 수준: Deep Dive
- HTML 정본: `2026-06-06/UCP_Deep_Dive.html`

## 1. Executive Summary

참고 링크: [UCP Docs](https://ucp.dev/), [Google Dev Blog](https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/), [Shopify Engineering](https://shopify.engineering/UCP)

UCP는 agentic commerce의 “상거래 실행 언어”다. 에이전트가 상품 탐색, 장바구니, 체크아웃, 주문, 멤버십/계정 연결, 결제 핸들러를 merchant마다 별도 연동 없이 사용할 수 있게 만드는 표준이다.

브리핑 한 줄:

> UCP는 “AI가 추천하는 쇼핑”이 아니라 “AI가 실제로 구매/예약/주문을 실행하는 커머스 운영체제”에 가깝다.

핵심:

- UCP는 결제 프로토콜이 아니라 commerce capability/discovery/checkout/order protocol이다.
- AP2는 UCP checkout에 붙는 결제 trust layer다.
- Google, Shopify, 대형 리테일/결제사들이 agentic shopping의 표준 접점을 만들고 있다.
- AI 플랫폼은 대화형 AI 서비스, 커머스, 쇼핑, 예약, 지도, 로컬, 결제를 가진 agentic commerce 플랫폼 후보군이다.

## 2. What It Is

참고 링크: [Core Concepts](https://ucp.dev/documentation/core-concepts/), [Specification](https://ucp.dev/latest/specification/overview/), [GitHub](https://github.com/Universal-Commerce-Protocol/ucp)

공식 문서 기준 UCP는 platform, agent, business가 상거래 기능을 상호운용할 수 있도록 하는 open standard다. 기존 commerce API가 각 쇼핑몰 또는 플랫폼별로 따로 붙어야 했다면, UCP는 business가 한 번 capability를 선언하고, agent/platform이 이를 발견하고 협상해 실행하는 모델이다.

UCP가 다루는 범위:

- catalog search / lookup
- cart building
- checkout
- identity linking
- order management
- post-purchase status
- payment handler integration

## 3. Technical Model

참고 링크: [Schema Reference](https://ucp.dev/2026-04-08/specification/reference/), [Versioning](https://ucp.dev/versioning/), [UCP + AP2](https://ucp.dev/documentation/ucp-and-ap2/)

### Roles

- Platform / Application / Agent: business capability를 소비하고 사용자를 대신해 orchestration한다.
- Business: capability를 노출하고 Merchant of Record로 남는다.
- Credential Provider: payment/shipping/PII를 안전하게 관리한다.
- PSP: 결제 처리 인프라를 담당한다.

### Discovery

- Business는 `/.well-known/ucp`에 profile을 발행한다.
- Platform은 요청마다 `UCP-Agent` header로 자기 profile을 알린다.
- Business가 양쪽 capability/version intersection을 계산한다.

### Capabilities

예:

- `dev.ucp.shopping.checkout`
- `dev.ucp.shopping.cart`
- `dev.ucp.shopping.catalog.search`
- `dev.ucp.shopping.catalog.lookup`
- `dev.ucp.shopping.order`
- `dev.ucp.common.identity_linking`

### Extensions

Discount, fulfillment, buyer consent, AP2 mandate 같은 확장 기능은 checkout/cart 같은 base capability 위에 composition된다.

### Transport

UCP는 REST, MCP, A2A, Embedded transport를 지원한다.

### Security

OAuth 2.0, HTTP Message Signatures, API key, mTLS를 지원한다. Profile은 capability와 signing key discovery를 같이 제공한다.

## 4. Recent Updates / Trend Signal

참고 링크: [Google agentic shopping](https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/), [UCP updates](https://blog.google/products-and-platforms/products/shopping/ucp-updates/), [Universal Cart](https://blog.google/products-and-platforms/products/shopping/shopping-updates-google-marketing-live/)

| 날짜 | 신호 | 의미 |
| --- | --- | --- |
| 2026-01-11 | Google/Shopify 중심으로 UCP 발표 | agentic commerce를 위한 open standard가 대형 리테일/결제 생태계와 함께 출발 |
| 2026-03-19 | Cart, Catalog, Identity Linking capability 업데이트 | 단일 상품 checkout을 넘어 실제 쇼핑 패턴, 실시간 재고/가격, 멤버십 혜택을 지원 |
| 2026-05-20 | Google Universal Cart, UCP-powered checkout, 호텔/음식 확장 발표 | 문서상 표준에서 Google Search/Gemini/Maps 소비자 접점으로 이동 |

## 5. AI Platform Implications

AI 플랫폼은 UCP를 “해외 shopping protocol”로만 보면 안 된다. conversational surface, 커머스/쇼핑/예약/로컬/지도, 결제 서비스, 사용자 계정, 채널/파트너 생태계를 가진 쪽에서 UCP식 profile과 capability negotiation은 직접적인 플랫폼 설계 문제다.

| AI 플랫폼 자산 | UCP식 capability 후보 | 설계 질문 |
| --- | --- | --- |
| 커머스 / 쇼핑 | catalog.search, cart, checkout, order | agent가 상품 비교, 옵션 선택, 결제, 배송지 확인을 한 흐름으로 실행할 수 있는가? |
| 결제 서비스 | payment handler, AP2 mandate | agent 결제 승인과 증빙을 payment-service trusted surface에서 처리할 수 있는가? |
| 사용자 계정 / 멤버십 | identity_linking | partner benefit, 쿠폰, 포인트, 등급 혜택을 안전하게 연결할 수 있는가? |
| 지도 / 로컬 / 예약 | lodging, food, reservation extension | 상품 구매뿐 아니라 예약, 주문, 픽업, 배달로 확장되는가? |
| 채널 / 파트너 | business profile onboarding | 중소 merchant가 쉽게 capability profile을 발행하도록 도와줄 수 있는가? |

## 6. Recommended Actions

1. AI 서비스 UCP Profile Draft: `/.well-known/agent-commerce` 또는 UCP-compatible profile schema 초안을 만든다.
2. Merchant Capability Pilot: 커머스/쇼핑에서 catalog, cart, checkout, order 네 가지 capability만 먼저 pilot한다.
3. AP2 결합 설계: checkout completion 전 payment-service trusted surface에서 mandate를 발행하는 흐름을 설계한다.
4. AG-UI/A2-UI 프론트 결합: cart 비교, 배송지 선택, 결제 승인, 주문 추적을 agent UI event와 declarative UI로 분리한다.
5. 리스크 검토: 가격/재고 불일치, 환불/취소 책임, 판매자 고지 의무, 개인정보/결제정보 위임, 전자상거래법/전자금융 규제 검토를 병행한다.

## 출처

### 왜 이걸 정리하게 되었는가

- [Google Dev Blog](https://developers.googleblog.com/under-the-hood-universal-commerce-protocol-ucp/)
- [Shopify Engineering](https://shopify.engineering/UCP)
- [Google agentic shopping](https://blog.google/products/ads-commerce/agentic-commerce-ai-tools-protocol-retailers-platforms/)
- [UCP updates](https://blog.google/products-and-platforms/products/shopping/ucp-updates/)
- [Universal Cart](https://blog.google/products-and-platforms/products/shopping/shopping-updates-google-marketing-live/)

### 딥리서치 출처

- [UCP Docs](https://ucp.dev/)
- [Core Concepts](https://ucp.dev/documentation/core-concepts/)
- [Specification](https://ucp.dev/latest/specification/overview/)
- [Schema Reference](https://ucp.dev/2026-04-08/specification/reference/)
- [Versioning](https://ucp.dev/versioning/)
- [UCP GitHub](https://github.com/Universal-Commerce-Protocol/ucp)
- [UCP and AP2](https://ucp.dev/documentation/ucp-and-ap2/)
