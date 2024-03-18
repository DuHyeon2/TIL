# HTTP METHOD란?
    서버 - 클라이언트 형식에서 요청(Request)와 응답(Response)이 이루어지는 방식

    서버에게 수행해야하는 동작을 요청하고 서버는 응답

    GET, POST, PUT, DELETE, PATCH, HEAD, OPTIONS, CONNECT 이렇게 크게 8가지가 있다.

## 멱등성
- 여러번 사용해도 결과가 달라지지 않는 성질
- HTTP METHOD에선 동일 요청을 한번이든 여러번이든 요청해도 결과가 달라지지 않는 성질

## GET
- 서버로부터 데이터를 가져오는데 사용된다.
- 링크 접속 등으로 사용된다.
