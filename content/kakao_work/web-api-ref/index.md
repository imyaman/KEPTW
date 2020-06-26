---
bookHidden: false
---

# Web API 레퍼런스

## 시작하기

---

카카오워크(Kakao Work)는 HTTP Remote Procedure Call(이하 RPC) 스타일의 API를 제공합니다. HTTP RPC 스타일은 리소스 중심의 REST API와는 달리, 종류별로 그룹 지어진 기능들을 URL로 표현하여 API를 구분합니다.

카카오워크 Web API의 URL은 다음과 같은 형식을 따릅니다.

```json
https://cbt-kw-api.kkep.io/v1/*{API 종류}.{API 이름}*
```

- API 종류: 각 API는 기능 영역별로 분류되어 제공됩니다. 예를 들어 사용자 정보를 다루는 API들은 Users 분류로 묶여 제공되고, 채팅방을 다루는 API는 Conversations, 메시지를 다루는 API는 Messages 분류로 묶여 제공됩니다.
- API 이름: 기능 영역별로 분류된 API 종류 중에서 각 행위를 수행하는 API명을 지칭합니다.

예를 들어, 워크 스페이스에 속한 멤버의 상세 정보를 조회하는 API는 다음과 같이 표현할 수 있습니다.

```json
https://cbt-kw-api.kkep.io/v1/users.info
```

## Web API 리스트

---

본 문서에서는 아래와 같은 Web API에 대한 자세한 설명을 제공합니다. 

[표. Action 리스트](Untitled%20a9cc7c16df854105946bc92ae39ad7fe/Action%20c8284dc745ba49fbb4c45b694579f9b1.csv)


## 공통 사항

---

API 공통 가이드는 카카오워크 Web API를 사용하여 Bot을 개발할 때 미리 알아야 하는 내용을 설명합니다.

### HTTPS Only

모든 카카오워크 Web API는 오직 HTTPS 프로토콜을 통해서만 사용 가능합니다. 어떠한 예외도 허용되지 않습니다.

### API 사용 제한

원활한 카카오워크 서비스 제공을 위하여 과도한 API 사용은 예고없이 제한될 수 있습니다. 보다 구체적인 제한 사항은 추후 확정될 예정입니다. API 사용에서 지속적인 제한이 발생한다면 호출 빈도를 조절하여 사용 가능합니다.

## API 요청

---

카카오워크에서 제공하고 있는 API의 요청 URL 형태는 아래와 같습니다.

HTTP RPC 스타일의 API 요청(Request)에서는 파라미터를 다음과 같은 방법으로 전달할 수 있습니다.

### API 호출 방식

[표. API 호출 방식](API%207b9ed98b6c7948698b9688cc77ca1c7d/API%208d700fa0383a424781e638f1a51ab838.csv)

API 호출 방식은 일반적으로 `application/json`과 `application/x-www-form-urlencoded` 2가지 방식 모두 허용하지만, 카카오워크 Web API에서는 `application/json` 방식 사용을 선호하고 있습니다. 특히 복잡한 데이터를 전달받아야하는 일부 API들은 `application/json`으로 표현된 데이터만 허용됩니다.

예시.`application/x-www-form-urlencoded` 방식의 HTTP 요청

```json
POST /v1/conversations.open
Content-type: application/x-www-form-urlencoded
Authorization: Bearer {YOUR_ACCESS_TOKEN}

user_id={OPPONENT_USER_ID}
```

예시. `application/json` 방식의 HTTP 요청

```json
POST /v1/conversations.open
Content-type: application/json
Authorization: Bearer {YOUR_ACCESS_TOKEN}

{"user_id": {OPPONENT_USER_ID}}
```

### API 인증

카카오워크의 Web API는 Bot 등록 과정에서 전달받은 Access Token을 HTTP 요청(Request)의 `Authorization` 헤더를 통해 전달하여 어떤 Bot에서 받은 요청인지 인증 및 권한 검사를 수행합니다. Access Token 발급 방법에 대한 자세한 내용은 [[Kakao Work] Bot 시작 가이드](https://www.notion.so/Bot-e2e20daa2d92476e949a0a15078d7ec0) 문서의 Bot 생성 및 등록 챕터를 참고하시기 바랍니다.

예시. Authorization Header 사용 CURL 요청

```bash
$ curl -X POST https://cbt-kw-api.kkep.io/v1/conversation.open \
-H "Authorization: Bearer {YOUR_ACCESS_TOKEN}" \
-d "user_id={USER_ID}"
```

## API 응답

---

모든 카카오워크 API 응답(Response)은 아래 테이블과 같은 구조를 갖는 JSON 객체로 표현됩니다. 따라서 Content Type은 `application/json`으로 고정됩니다.

### 공통 응답 포맷

모든 카카오워크 API 응답은 다음의 구조를 갖는 JSON 객체로 표현됩니다.

[표. 공통 응답 포맷](API%207b9ed98b6c7948698b9688cc77ca1c7d/Untitled%20b8071201694f45d9af25e88d96993262.csv)

[[error](https://www.notion.so/6f35fae26c6841ee94e25daf5c5677b1?v=1dbe53845109419ebe92222711851606)](API%207b9ed98b6c7948698b9688cc77ca1c7d/Untitled%20b8071201694f45d9af25e88d96993262/error%20bdd829673eb646998fcaccc2d69482df.md)

호출된 API의 성공 혹은 실패 여부는 `success` 필드의 값을 통해 확인할 수 있습니다. 만약 `success` 필드의 값이 “True”인 경우에는 호출된 API는 문제 없이 성공된 것이고, 반면에 `success` 필드의 값이 “False”인 경우에는 실패를 나타냅니다.

예시. API 호출 성공 응답

```json
{
  "success": true,
  "user": { /* ... user entity ... */ }
}
```

### 공통 에러 정보

---

API 호출이 실패한 경우, `error` 필드를 통해 실패 이유에 대해 보다 상세한 정보를 얻을 수 있습니다.

[code](API%207b9ed98b6c7948698b9688cc77ca1c7d/error%200d077009030549c8aca048e7349b6466/code%20ac9f7ea06a4443fa816336446f19ba1e.md)

예시. API 호출 실패 응답

```json
{
  "success": false,
  "error": {
    "code": "missing_parameter",
    "message": "user_id parameter is missing."
  }
}
```

### 필드 타입 표기

API 응답은 기본적으로 [JSON 타입](https://ko.wikipedia.org/wiki/JSON#%EC%9E%90%EB%A3%8C%ED%98%95%EA%B3%BC_%EB%AC%B8%EB%B2%95)을 따르며, 추가로 확장된 타입은 아래 표기를 따릅니다.

[표. 필드 타입 표기](API%207b9ed98b6c7948698b9688cc77ca1c7d/Untitled%2081e6528aa064462cb313bea00e92e713.csv)

## Pagination

---

Pagination이 적용된 목록 조회 API는 아래와 같은 규칙과 과정을 가집니다.

### 기본 규칙

API 응답이 아래 두 조건 중 하나라도 충족할 경우 더 이상 조회할 내용이 없는 것으로 판단합니다.

- 목록이 빈 배열일 경우
- `cursor`가 NULL일 경우

### 첫번째 호출

목록 조회 단위인 `limit`를 파라미터로 지정합니다. 없으면 API에 정의된 기본값이 사용됩니다.

### 첫번째 호출 이후의 모든 호출

바로 이전 API 호출 응답의 `cursor`만 파라미터로 지정합니다. `cursor` 외 다른 파라미터를 같이 호출할 시 실패 응답이 반환됩니다.