# http 프로토콜, 헤더의 구조

# HTTP의 구조

<center>
<img width ='500px' src = "https://user-images.githubusercontent.com/69442847/196601857-3e63dc5e-ea3c-4932-96c4-8504d01af04f.png">

<br>

<img width ='400px' src = "https://user-images.githubusercontent.com/69442847/196601196-54dfd719-85b6-4ee8-a1fc-7ab2d90eb5f7.png">

</center>
<br>
<ul>

**1** - Request Line (요청 라인) 또는 Start Line (상태라인) 

**2** - Header (헤더) : **general header** | **request header** or **response header** | **entity header**

**3** - Blank Line (공백)

**4** - Body (본문, 바디)

</ul>
<br>

---

# 1. HTTP의 Start Line

## 1 - a. 요청 메시지의 시작라인 (request line)

### `GET /search?q=hello&hl=ko HTTP/1.1`

- method : HTTP 메서드 (Get:조회), 메서드로 서버가 수행할 동작을 지정
- Request-target : 요청 대상 (/search?q=hello&hl=ko), 경로
- HTTP Version

## 1 - b. 응답 메시지의 시작라인 (status line)

### 예시 : `**HTTP/1.1 200 OK**`

- **HTTP Version**
- **HTTP 상태 코드 :** 요청 성공, 실패를 나타냄
    - 200: 성공
    - 400: 클라이언트 요청 오류
    - 500: 서버 내부 오류
- **이유 문구 :** 사람이 이해할 수 있는 짧은 상태 코드 설명 글

<br>

---

# 2. HTTP의 Header

![image](https://user-images.githubusercontent.com/69442847/196601214-c79f3cae-9ded-4a00-9bcd-f9db5c437308.png)

![image](https://user-images.githubusercontent.com/69442847/196601246-c2ea5e9d-66b6-4330-880c-c432b4652dfc.png)


## 2 - 0. Header

- HTTP메시지(요청/응답)와 본문에 대한 정보로, 해당 메시지가 제공하는 기능에 대한 최소한의 정보
- 표현 데이터를 해석할 수 있는 정보(부가정보) 제공
(그 프로토콜에 불필요한 내용을 담으면 네트워크로 전송되는 데이터의 크기가 커져서 빠른 전송이 불가능하기 때문에 프로토콜을 설계할 때부터 꼭 필요한 내용만 담아야 하고 모든 기능이 표현되어야 한다.)

<br>

## 2 - 1. HTTP Header의 구조

- General Header
- Request/Response Header
- Entity Header

### 2 - 1  - 1. General Header

: 전송되는 컨텐츠에 대한 정보보다는 요청/응답이 이루어지는 날짜 및 시간등에 대한 일반적인 정보가 포함된다.

- **Date**

    현재시간 (Sat, 23 Mat 2019 GMT)

- **Pragma**

    캐시제어 (no-cache) : HTTP/1.0에서 쓰던 것으로 HTTP/1.1에서는 Cache-Control이 쓰인다.
- **Cache-Control**
캐시 제어
    
    \+ no-cache : 모든 캐시를 쓰기 전에 서버에 해당 캐시를 사용해도 되는지 확인하겠다.
    
    \+ public : 공유 캐시에 저장해도 된다.
    
    \+ max-age : 캐시의 유효시간을 명시하겠다.
    
    \+ private : '브라우저' 같은 특정 사용자 환경에만 저장하겠다.
    
    \+ must-revalidate : 만료된 캐시만 서버에 확인하겠다.
    
    \+ no-store : 캐시를 저장하지 않겠다.
- **Transfer-Encoding**
body 내용 자체 압축 방식 지정본문에 데이터 길이가 나와서 야금야금 브라우저가 해석해서 화면에 뿌려줄 때 이 기능을 사용한다.
'chunked'면 본문의 내용이 동적으로 생성되어 길이를 모르기 때문에 나눠서 보낸다는 의미다.
- **Upgrade**
프로토콜 변경시 사용 ex) HTTP/2.0
- **Via**
중계(프록시)서버의 이름, 버전, 호스트명
- **Content-Encoding**
본문의  리소스 압축 방식 (transfer-encoding은 body 자체이므로 다름)
- **Content-type**
본문의 미디어 타입(MIME) ex) application/json, text/html
- **Content-Length**
본문의 길이
- **Content-language**
본문을 이해하는데 가장 적절한 언어 ex) ko.
한국사이트여도 본문을 이해하는데 영어가 제일 적절하면 영어로 지정된다.
- **Expires**
자원의 만료 일자
- **Allow**
사용이 가능한 HTTP 메소드 방식 ex) GET, HEAD, POST
- **Last-Modified**
최근에 수정된 날짜
- **ETag**
캐시 업데이트 정보를 위한 임의의 식별 숫자
- **Connection**
클라이언트와 서버의 연결 방식 설정.
**HTTP/1.1은 kepp-alive 로 연결 유지하는게 디폴트.**

### 2 - 1 - 2. Request/Response Header

- 요청 메시지 헤더 : 

    **`Host:www.google.com`**

- 응답 메시지 헤더 :
    
    **`Content-type: text/html;charset=UTF-8`** <br>
    **`Content-Length: 3423`**
    

    ![image](https://user-images.githubusercontent.com/69442847/196601279-7e9bf6c8-f9c5-4ad9-84f0-691da22177ec.png)

    ![image](https://user-images.githubusercontent.com/69442847/196601301-53d4ead5-9377-46b9-a23c-b6b5b3074aad.png)




### 2 - 1 - 2 - a. **Request Header**

웹브라우저가 웹서버에 요청하는 것을 텍스트로 변환한 메시지

- **request header form**
    
    
    - **Request Line**
    어떤 웹서버로 접속(Host 부분)해서 어떠한 방식(HTTP/1.1)으로, 어떠한 메소드(GET)를 통해 무엇을(/doc/test/.html) 요청했는지에 대한 메시지가 담겨있다.
        - HTTP 메소드
            - GET
            요청하는 데이터가 HTTP Request Message의 Header 부분의 url 에 담겨서 쿼리스트링으로 전송되는 메소드
                - url 공간에 전송할 수 있는 데이터의 크기가 제한적
                - 데이터가 그대로 url 에 노출
                ⇒ 보안 취약
            - POST
            HTTP Message의 Body 부분에 데이터가 담겨서 전송되는 메소드
                - 데이터 크기가 GET 방식보다 크고 보안 면에서 낫다.
                (하지만 보안적인 측면에서는 암호화를 하지 않는 이상 고만고만하다.)
            - HEAD
            GET과 비슷하지만 서버는 응답으로 엔터티 본문 반환없이 헤더만을 반환하고 클라이언트는 리소스를 가져올 필요 없이 헤더 만을 통해 정보를 얻을 수 있다.
            - PUT
            서버가 요청의 본문을 갖고 요청 URI의 이름대로 새 문서를 만들거나 이미 URI가 존재한다면 요청 본문을 변경하는 메서드 ( 수정 )
            - DELETE
            서버에서 요청 URI 리소스를 삭제하도록 요청하는 메서드
            - TRACE
            클라이언트와 목적지 서버 사이에 있는 모든 HTTP 애플리케이션의 요청/응답 연쇄를 따라가면서 자신이 보낸 메시지의 이상 유무를 파악하는 메서드
                - 서버는 응답 메시지의 본문에 자신이 받은 요청메시지를 넣어 응답하며, 주로 진단을 위해 사용
            - OPTIONS
            서버에게 특정 리소스가 어떤 메소드를 지원하는지 물어보는 메서드
    - **Host**
    요청하려는 서버 호스트 이름과 포트번호
    - **User-agent**
    클라이언트 프로그램 정보 ex) Mozilla/4.0, Windows NT5.1
    이 정보를 통해서 서버는 클라이언트 프로그램(브라우저)에 맞는 최적의 데이터를 보내줄 수 있다.
    - **Referer**
    바로 직전에 머물렀던 웹 링크 주소(해당 요청을 할 수 있게 된 페이지)
    - **Accept**
    클라이언트가 처리 가능한 미디어 타입 종류 나열 ex) */* - 모든 타입 처리 가능, application/json - json데이터 처리 가능.
    - **Accept-charset**
    클라이언트가 지원가능한 문자열 인코딩 방식
    - **Accept-language**
    클라이언트가 지원가능한 언어 나열
    - **Accept-encoding**
    클라이언트가 해석가능한 압축 방식 지정 ex) gzip, deflate
    압축이 되어있다면 content-length와 content-encoding으로 압축을 해제한다.
    - **Content-location**
    해당 개체의 실제 위치
    - **Content-disposition**
    응답 메세지를 브라우저가 어떻게 처리할지 알려줌. ex) inline, attachment; filename='jeong-pro.xlsx'
    - **Content-Security-Policy**
    다른 외부 파일을 불러오는 경우 차단할 리소스와 불러올 리소스 명시ex) default-src 'self' -> 자기 도메인에서만 가져옴
        - ex) default-src 'none' -> 외부파일은 가져올 수 없음
        - ex) default-src https -> https로만 파일을 가져옴
    - **If-Modified-Since**
    여기에 쓰여진 시간 이후로 변경된 리소스 취득. 페이지가 수정되었으면 최신 페이지로 교체하기 위해 사용된다.
    - **Authorization**
    인증 토큰을 서버로 보낼 때 쓰이는 헤더
    - **Origin**
    서버로 Post 요청을 보낼 때 요청이 어느 주소에서 시작되었는지 나타내는 값.
    이 값으로 요청을 보낸 주소와 받는 주소가 다르면 CORS 에러가 난다.
    - **Cookie**
    쿠기 값 key-value로 표현된다. ex) attr1=value1; attr2=value2

### 2 - 1 - 2 - b. **Response Header**

: 웹 서버가 웹브라우저에 응답하는 콘텐츠가 들어가있는 메시지

- response header form
웹브라우저가 요청한 메시지에 대한 status, 메세지, 요청한 응답 값들이 body에 담겨있다.
    
    - HTTP Version
    - status code(상태코드)
        - 100 - 109
        메시지 정보
        - 200 - 206
        요청 성공
        - 300 - 305
        리다이렉션
        - 400 - 415
        클라이언트 에러
        - 500 - 505
        서버에러

    - **Location**

        301, 302 상태코드일 때만 볼 수 있는 헤더, <br> 서버의 응답이 다른 곳에 있다고 알려주면서 해당 위치(URI)를 지정한다.
    - **Server**
    웹서버의 종류 ex) nginx
    - **Age**

        max-age 시간내에서 얼마나 흘렀는지 초 단위로 알려주는 값

    - **Referrer-policy**

        서버 referrer 정책을 알려주는 값 ex) origin, no-referrer, unsafe-url

    - **WWW-Authenticate**
    
        사용자 인증이 필요한 자원을 요구할 시, 서버가 제공하는 인증 방식

    - **Proxy-Authenticate**
    
        요청한 서버가 프록시 서버인 경우 유저 인증을 위한 값

### 2 - 1 - 3. Entity header

실제 주고받는 컨텐츠와 관련된 http 본문에 대한 정보

# HTTP의 본문 - 페이로드(payload)

: 요청이나 응답에서 전달할 실제 데이터

- 메시지 본문(message body)을 통해 표현 데이터를 전달한다.

<br>


---
### 참고

[https://velog.io/@jch9537/WEB-HTTP](https://velog.io/@jch9537/WEB-HTTP)

[https://hazel-developer.tistory.com/145](https://hazel-developer.tistory.com/145)

[https://velog.io/@wngud4950/HTTP와-HTTP-Header](https://velog.io/@wngud4950/HTTP%EC%99%80-HTTP-Header)

[https://velog.io/@gparkkii/HTTPMessage](https://velog.io/@gparkkii/HTTPMessage)

<br>

<sub>
날짜: 2022/10/05<br>
상태: Done<br>
자료조사: 또로리, namnameeroo<br>
태그: network
</sub>