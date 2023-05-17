# CORS

면접때 받았던 질문 'CORS가 무엇인지 아나요'의 `CORS` 개념 짚고 넘어가자

# SOP

> 💡 SOP
>
> 다른 출처의 리소스를 사용하는 것에 제한하는 보안 방식

```
https://github.com:443/da-nyee?tab=repositories#example

https:// => Protocol
github.com => Host
:443 => PORT
/da-byee => Path
?tab=repsitories => Query String
#example => Fragment
```

- 출처란 URL의 `Protocol`, `Host`, `Port`를 통해 같은 출처인지 다른 출처인지 판단할 수 있다

# Why SOP?

만약 내가 은행 업무를 보기 위해서 은행에 로그인 하였고 해당 토큰을 발행해서 갖고 있고 <br>
은행의 URL은 https://kb.com 이라는 가정한다 <br>
해커는 내게 메일을 보내왔고 해당 메일은 나에게 굉장히 매력적인 내용을 담고 있다 <br>
따라서 나는 해당 URL에 접속했고 해당 URL의 주소는 https://hacker.com이라고 가정하자<br>
해커는 해당 사이트에 자신의 계좌로 돈을 송금하는 스크립트를 심어 놓았았지만<br>
해당 SOP가 출처를 비교하여 `Cross Origin` 에러와 함께 해당 스크립트는 실행되지 않는다<br>
해당 상황이 `SOP`가 존재하는 이유이다

# How can Use another origin source?

> 교차 출처 리소스 공유 (Cross-Origin Resource Sharing, CORS)

CORS는 추가 HTTP 헤더를 사용하여, 한 출처에서 실행 중인 웹 애플리케이션이<br>
다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여하도록 `브라우저`에 알려주는 체제이다

# CORS 접근제어 시나리오

1. 단순 요청 (Simple Request)

2. 프리플라이트 요청(Preflight Request)

3. 인증정보 포함 요청 (Credentialed Request)

# Preflight

1. OPTIONS 메서드를 통해 다른 도메인의 리소스에 요청이 가능한 지 확인 작업

- Preflight Request

  - Origin: 요청 출처

  - Acess-Control-Request-Method: 실제 요청의 메서드

  - Access_Control-Request-Headers: 실에 요청의 추가 헤더

```
//PREFLIGHT Request

OPTIONS /doc HTTP/1.1
Origin http:foo.example
Acess-Control-Request-Method: POST
Acess-Control-Request-Headers: X-PINGOTHER, Content-Type
```

- Preflight Response

  - Access-Control-Allow-Origin: 서버 측 허가 출처

  - Access-Control-Allow-Methods: 서버 측 허가 메서드

  - Access-Control-Allow-Headers: 서버 측 허가 헤더

  - Access-Control-Max-Age: Preflight 응답 캐시 기간

```
//PREFLIGHT Response

Access-Control-Allow-Origin: http://foo.example
Access-Control-Allow-Methods: POST, GET, OPTIONS
Access-Control-Allow-Headers: X-PINGOTHER, Content-Type
Access-Control-Allow-Age: 86400
```

> Preflight Response가 가져야 하는 특징
>
> 응답 코드는 200대여야 한다
>
> 응답 바디는 비어있는 것이 좋다

2. 요청이 가능하다면 실제 요청(Actual Request)을 보낸다

# Simple Request

Preflight 오청 없이 바로 요청을 보내며 다음 조건을 모두 만족해야 한다.

- GET, POST, HEAD (METHOD)

- Content-Type

  - application/x-www-form-urlencoded

  - multipart/form-data

  - text/plain

- 헤더는 Accept, Accept-Language, Content-Language, Content-Type만 허용

# Credentialed Request

인증 관련 헤더를 포함할 때 사용하는 요청이다

클라이언측
credentials: include

서버측
Access-Control-Allow-Credentials: true
(Access-Control-Allow-Orign: \*은 허용되지 않음)
