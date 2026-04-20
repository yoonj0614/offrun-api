# API 명세서 - 송장 조회

> 프로젝트 이름: `offrun-api`
> 버전: `v1.0.0`
> 최종 수정일: `2026-04-20`
> 작성자: `<작성자 이름>`

---

## 목차

- [개요](#개요)
- [기본 정보](#기본-정보)
- [공통 응답 형식](#공통-응답-형식)
- [API 목록](#api-목록)
  - [1. 송장 조회 (getInvcInfo)](#1-송장-조회-getinvcinfo)
- [에러 코드](#에러-코드)
- [변경 이력](#변경-이력)

---

## 개요

주문(Order)에 대한 송장 정보를 조회하는 API 명세서입니다.
회원 ID와 주문 ID를 기반으로 배송비 및 송장 번호를 조회할 수 있습니다.

---

## 기본 정보

| 항목 | 내용 |
|------|------|
| Base URL (개발) | `http://localhost:8082` |
| Base URL (운영) | `https://<운영 도메인>` |
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
GET /order/getInvcInfo?mid=26&oid=7612 HTTP/1.1
Host: localhost:8082
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

### 1. 송장 조회 (getInvcInfo)

회원 ID 및 주문 ID 기반으로 해당 주문의 송장 정보를 조회합니다.

#### 요청

| 항목 | 내용 |
|------|------|
| Method | `GET` |
| URL | `/order/getInvcInfo` |
| 권한 | `x-api-key` 필요 |

#### Query Parameters

| 이름 | 타입 | 필수 | 기본값 | 설명 |
|------|------|------|--------|------|
| `mid` | `integer` | ✅ | - | 회원 고유 ID |
| `oid` | `integer` | ✅ | - | 주문 고유 ID |

#### 요청 예시 (cURL)

```bash
curl -X GET "http://localhost:8082/order/getInvcInfo?mid=26&oid=7612" \
  -H "x-api-key: 발급된 API key"
```

#### 요청 URL 예시

```
http://localhost:8082/order/getInvcInfo?mid=26&oid=7612
```

#### 응답 (200 OK)

```json
{
  "statusCode": 200,
  "responseMessage": "성공",
  "data": {
    "invcInfo": {
      "key": "invcInfo",
      "data": {
        "oid": 7612,
        "deliveryPrice": 3000,
        "invcNo": "530525207082"
      }
    }
  }
}
```

#### 응답 필드 상세

##### `data.invcInfo`

| 필드 | 타입 | 설명 |
|------|------|------|
| `key` | `string` | 응답 데이터 키 (고정값: `invcInfo`) |
| `data` | `object` | 송장 데이터 래퍼 |

##### `data.invcInfo.data` (송장 객체)

| 필드 | 타입 | Nullable | 설명 | 예시 |
|------|------|:---:|------|------|
| `oid` | `integer` | ❌ | 주문 고유 ID | `7612` |
| `deliveryPrice` | `integer` | ❌ | 배송비 (원) | `3000` |
| `invcNo` | `string` | ❌ | 송장 번호 | `"530525207082"` |

---

## 에러 코드

| HTTP Status | responseMessage | 설명 |
|-------------|-----------------|------|
| 200 | `성공` | 요청 정상 처리 |
| 204 | `송장 정보 없음` | 조회된 송장 데이터가 존재하지 않음 |
| 400 | `잘못된 요청` | 파라미터 누락 또는 타입 오류 |
| 401 | `인증 실패` | `x-api-key` 헤더 누락 또는 잘못된 키 |
| 500 | `서버 에러` | 내부 서버 오류 |

---

## 참고 사항

- `mid`와 `oid`는 모두 필수 파라미터입니다.
- `invcNo`는 택배사 송장 번호로, 문자열 형태로 반환됩니다 (숫자로 파싱 금지 — 앞자리 `0` 손실 가능).
- 송장이 아직 등록되지 않은 주문의 경우 `204 No Content` 가 반환될 수 있습니다.
- `deliveryPrice`는 원(KRW) 단위의 정수입니다.

---

## 변경 이력

| 버전 | 날짜 | 작성자 | 변경 내용 |
|------|------|--------|-----------|
| v1.0.0 | 2026-04-20 | `<이름>` | 최초 작성 - 송장 조회 API 추가 |
