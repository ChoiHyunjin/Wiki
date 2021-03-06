# What is HTTP

[HTTP 개요 - HTTP | MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/Overview)

# 개요

HTTP는 HTML문서와 같은 **리소스를 가져올 수 있도록 해주는 프로토콜.** 웹에서 이루어지는 모든 데이터 교환의 기초이며, 클라이언트-서버 프로토콜이다. 하나의 완전한 문서는 텍스트, 레이아웃, 이미지 등 불러온(fetched) 하위 문서들로 재구성된다.

- 클라이언트-서버 프로토콜이란?
    
    수신자 측에 의해 요청이 초기화되는 프로토콜
    

클라이언트와 서버는 개별적인 메시지 교환에 의해 통신한다. 

- 요청 : 클라이언트(보통 브라우저)에 의해 전송되는 메시지
- 응답 : 요청에 대해 서버에서 전송하는 메시지

![Untitled](images/What%20is%20HTTP/Untitled.png)

HTTP는 진화해온 확장가능한 프로토콜이다. 애플리케이션 계층의 프로토콜로, 신뢰 가능한 전송 프로토콜이라면 무엇이든 전송할 수 있으나, TCP 혹은 TLS(암호화된 TCP)를 통해 전송된다. HTTP의 확장석 덕분에, 하이퍼텍스트 뿐 아니라 이미지, 비디오 HTML 폼 결과와 같은 내용을 서버로 POST하기 위해서도 사용된다. 또한 필요할 때마다 웹 페이지를 갱신하기 위해 문서의 일부를 가져오는데 사용할 수도 있다.

# HTTP 기반 시스템의 구성요소

HTTP는 클라이언트-서버 프로토콜이다. 요청은 사용자 에이전트(혹은 그것을 대신하는 프록시)에서만 전송된다. 사용자 에이전트는 일반적으로 브라우저지만, 무엇이든 될 수 있다.(ex. 검색 엔진 인덱스를 채워넣고 유지하기 위해 웹을 돌아다니는 로봇)

클라이언트가 요청(request)을 하면 서버에서 받아서 이에 대한 응답(response)를 전달한다. 이 과정 사이에는 게이트웨이, 캐시 역할을 하는 프록시 등의 개체들이 있다.

- 게이트웨이란?

![Untitled](images/What%20is%20HTTP/Untitled%201.png)

실제로는 브라우저와 서버 사이에 모뎀과 라우터 등의 다양한 컴퓨터들이 존재한다. 웹의 계층적인 설계 덕분에, 이들은 네트워크와 전송 Layer에 숨겨지고, 애플리케이션 Layer인 HTTP와는 거의 관련이 없다.

## 클라이언트 : 사용자 에이전트

사용자 에이전트는 사용자를 대신해 동작하는 모든 도구. 일반적으로 브라우저.

브라우저는 항상 요청을 보낸다.

웹 페이지 표시 과정

1. 브라우저에서 HTML 문서를 가져오기 위한 요청을 보낸다.
2. 그 뒤 파일을 분석해서 실행해야 할 스크립트, 페이지 내 리소스(이미지, 비디오 등)를 표시하기 위한 레이아웃 정보(CSS)에 대응하는 추가적인 요청들을 가져온다.
3. 모두 가져오면 이것들을 조립한다.
4. 브라우저에 의해 실행된 스크립트는 이후 더 많은 리소스를 가져올 수 있으며, 그에 따라 웹 페이지를 갱신한다.

웹 페이지는 하이퍼텍스트 문서로, 텍스트의 일부는 사용자가 브라우저를 제어할 수 있고, 새로운 웹페에지를 가져오기 위해 실행(보통 마우스 클릭)될 수 있는 링크이다. 브라우저는 HTTP 요청 내에서 이런 지시를 변환하고, HTTP 응답을 해석해서 사용자에게 표시한다.

## 웹 서버

## 프록시

# 기초적 측면

## 간단

## 확장 가능

## 상태가 없지만, 세션은 있다

## 연결

# HTTP로 제어할 수 있는 것

# HTTP 흐름

# HTTP 메시지

## 요청

## 응답

# HTTP 기반 API

# 결론
