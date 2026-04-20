# API 명세서 - 주문 조회

> 프로젝트 이름: `offrun-api`
> 버전: `v1.0.0`
> 최종 수정일: `2026-04-20`
> 작성자: `윤준`

---

## 목차

- [개요](#개요)
- [기본 정보](#기본-정보)
- [공통 요청 헤더](#공통-요청-헤더)
- [공통 응답 형식](#공통-응답-형식)
- [API 목록](#api-목록)
  - [1. 주문 조회 (getOrderInfo)](#1-주문-조회-getorderinfo)
- [에러 코드](#에러-코드)
- [변경 이력](#변경-이력)

---

## 개요

회원 ID와 주문 ID 기반으로 주문 상세 정보 및 주문 상품 목록을 조회하는 API 명세서입니다.

---

## 기본 정보

| 항목 | 내용 |
|------|------|
| Base URL (운영) | `[https://<운영 도메인>](https://kicoebiz.net/api)` |
| 프로토콜 | HTTP / HTTPS |
| 데이터 형식 | JSON (UTF-8) |
| 문자 인코딩 | UTF-8 |

---

## 공통 요청 헤더

모든 API 요청 시 아래 헤더가 포함되어야 합니다.

| 헤더 | 필수 | 설명 | 예시 |
|------|:---:|------|------|
| `x-api-key` | ✅ | 당사에서 발급한 API 키 | `발급된 API key` |
| `Content-Type` | ❌ | 요청 본문 형식 (POST/PUT/PATCH 시 필수) | `application/json` |

### 요청 헤더 예시

```http
GET /order/getOrderInfo?mid=26&oid=7612 HTTP/1.1
Host: https://kicoebiz.net/api
x-api-key: 발급된 API key
Accept: */*
```

> ⚠️ `x-api-key` 값은 당사에서 별도로 발급받아야 하며, **외부에 노출되지 않도록 반드시 안전하게 보관**해야 합니다. (소스코드에 하드코딩 금지 — 환경변수 또는 시크릿 매니저 사용 권장)

---

## 공통 응답 형식

```json
{
  "statusCode": 200,
  "responseMessage": "성공",
  "data": { }
}
```

| 필드 | 타입 | 설명 |
|------|------|------|
| `statusCode` | `integer` | HTTP 상태 코드 |
| `responseMessage` | `string` | 응답 메시지 |
| `data` | `object` | 실제 응답 데이터 |

---

## API 목록

### 1. 주문 조회 (getOrderInfo)

회원 ID 및 주문 ID 기반으로 주문 상세 정보와 주문 상품 목록을 조회합니다.

#### 요청

| 항목 | 내용 |
|------|------|
| Method | `GET` |
| URL | `/order/getOrderInfo` |
| 권한 | `x-api-key` 필요 |

#### Query Parameters

| 이름 | 타입 | 필수 | 기본값 | 설명 |
|------|------|------|--------|------|
| `mid` | `integer` | ✅ | - | 회원 고유 ID |
| `oid` | `integer` | ✅ | - | 주문 고유 ID |

#### 요청 예시 (cURL)

```bash
curl -X GET "https://kicoebiz.net/api/order/getOrderInfo?mid=26&oid=7612" \
  -H "x-api-key: 발급된 API key"
```

#### 요청 URL 예시

```
https://kicoebiz.net/api/order/getOrderInfo?mid=26&oid=7612
```

#### 응답 (200 OK)

```json
{
  "statusCode": 200,
  "responseMessage": "성공",
  "data": {
    "orderInfo": {
      "key": "orderInfo",
      "data": {
        "oid": 7612,
        "orderNm": "슬기로운멍냥",
        "orderPhone": "01099120182",
        "receiverNm": " 허경화",
        "receiverPhone": "0502-1736-2655",
        "receiverZipcode": "41535",
        "receiverRoadAddr": "대구 북구 대동로1길 8-1",
        "orderMemo": "직접 받고 부재 시 문 앞",
        "orderPrice": "26760",
        "orderPaymentPrice": "26760",
        "orderPaymentDeposit": "0",
        "orderPaymentMileage": "0",
        "orderDisPrice": 0,
        "orderStatusNm": "배송 완료",
        "inspectionStatusNm": "검수 준비중",
        "regDt": "2025-07-11 14:41:43",
        "payDt": "2025-07-11 14:51:04",
        "delDt": "2025-07-11 17:12:56",
        "orderItemList": [
          {
            "goodsNo": 1356,
            "clientNm": "에이플러스",
            "brandNm": "이나바",
            "goodsPrice": 1980,
            "goodsEa": 12,
            "canEa": 0,
            "returnEa": 0,
            "changeEa": 0
          }
        ]
      }
    }
  }
}
```

#### 응답 필드 상세

##### `data.orderInfo`

| 필드 | 타입 | 설명 |
|------|------|------|
| `key` | `string` | 응답 데이터 키 (고정값: `orderInfo`) |
| `data` | `object` | 주문 데이터 래퍼 |

##### `data.orderInfo.data` (주문 객체)

| 필드 | 타입 | Nullable | 설명 | 예시 |
|------|------|:---:|------|------|
| `oid` | `integer` | ❌ | 주문 고유 ID | `7612` |
| `orderNm` | `string` | ❌ | 주문자명 | `"슬기로운멍냥"` |
| `orderPhone` | `string` | ❌ | 주문자 연락처 | `"01099120182"` |
| `receiverNm` | `string` | ❌ | 수령인명 | `" 허경화"` |
| `receiverPhone` | `string` | ❌ | 수령인 연락처 | `"0502-1736-2655"` |
| `receiverZipcode` | `string` | ❌ | 수령지 우편번호 | `"41535"` |
| `receiverRoadAddr` | `string` | ❌ | 수령지 도로명 주소 | `"대구 북구 대동로1길 8-1"` |
| `orderMemo` | `string` | ✅ | 배송 메모 | `"직접 받고 부재 시 문 앞"` |
| `orderPrice` | `string` | ❌ | 주문 금액 (원, 문자열) | `"26760"` |
| `orderPaymentPrice` | `string` | ❌ | 실 결제 금액 (원, 문자열) | `"26760"` |
| `orderPaymentDeposit` | `string` | ❌ | 예치금 사용액 (원, 문자열) | `"0"` |
| `orderPaymentMileage` | `string` | ❌ | 마일리지 사용액 (원, 문자열) | `"0"` |
| `orderDisPrice` | `integer` | ❌ | 할인 금액 (원, 정수) | `0` |
| `orderStatusNm` | `string` | ❌ | 주문 상태명 | `"배송 완료"` |
| `inspectionStatusNm` | `string` | ❌ | 검수 상태명 | `"검수 준비중"` |
| `regDt` | `string` | ❌ | 주문 등록일시 (`YYYY-MM-DD HH:mm:ss`) | `"2025-07-11 14:41:43"` |
| `payDt` | `string` | ✅ | 결제일시 (`YYYY-MM-DD HH:mm:ss`) | `"2025-07-11 14:51:04"` |
| `delDt` | `string` | ✅ | 배송 완료일시 (`YYYY-MM-DD HH:mm:ss`) | `"2025-07-11 17:12:56"` |
| `orderItemList` | `array<object>` | ❌ | 주문 상품 목록 (아래 참조) | - |

##### `data.orderInfo.data.orderItemList[]` (주문 상품 객체)

| 필드 | 타입 | Nullable | 설명 | 예시 |
|------|------|:---:|------|------|
| `goodsNo` | `integer` | ❌ | 상품 번호 | `1356` |
| `clientNm` | `string` | ❌ | 거래처명 | `"에이플러스"` |
| `brandNm` | `string` | ❌ | 브랜드명 | `"이나바"` |
| `goodsPrice` | `integer` | ❌ | 상품 단가 (원) | `1980` |
| `goodsEa` | `integer` | ❌ | 주문 수량 | `12` |
| `canEa` | `integer` | ❌ | 취소 수량 | `0` |
| `returnEa` | `integer` | ❌ | 반품 수량 | `0` |
| `changeEa` | `integer` | ❌ | 교환 수량 | `0` |

---

## 에러 코드

| HTTP Status | responseMessage | 설명 |
|-------------|-----------------|------|
| 200 | `성공` | 요청 정상 처리 |
| 204 | `주문 정보 없음` | 조회된 주문 데이터가 존재하지 않음 |
| 400 | `잘못된 요청` | 파라미터 누락 또는 타입 오류 |
| 401 | `인증 실패` | `x-api-key` 헤더 누락 또는 잘못된 키 |
| 500 | `서버 에러` | 내부 서버 오류 |

---

## 참고 사항

- `mid`와 `oid`는 모두 **필수 파라미터**입니다.
- 금액 관련 필드(`orderPrice`, `orderPaymentPrice`, `orderPaymentDeposit`, `orderPaymentMileage`)는 **문자열(string)** 로 반환됩니다. 계산 시 정수로 변환이 필요합니다.
- `orderDisPrice`는 예외적으로 **정수(integer)** 로 반환됩니다.
- `receiverNm` 앞에 **공백 문자가 포함**될 수 있으므로, 화면 렌더링 시 `.trim()` 처리를 권장합니다.
- `payDt`, `delDt`는 결제/배송 상태에 따라 null이 될 수 있습니다.
- `orderItemList`는 주문에 포함된 여러 상품을 담는 배열입니다. 단일 상품 주문도 배열 형태로 반환됩니다.

---

## 변경 이력

| 버전 | 날짜 | 작성자 | 변경 내용 |
|------|------|--------|-----------|
| v1.0.0 | 2026-04-20 | `윤준` | 최초 작성 - 주문 조회 API 추가 |
