# What is DNS?

[DNS란 무엇입니까? - DNS 소개 - AWS](https://aws.amazon.com/ko/route53/what-is-dns/)

[](https://namu.wiki/w/DNS)

Domain Name Service 는 domain name을 IP 주소로 변환한다.

DNS 서버는 DNS 가 운영하는 서버이다. DNS 서버는 IP 주소로 변환하여 최종 사용자를 어떤 서버에 연결할 것인지 제어한다. 이 요청을 **쿼리**라고 한다.

DNS 는 Forward zone (Domain name → IP)와 Reverse Zone(IP → Domain)이 있다.

## DNS 응답

### 1. 신뢰가능한 응답

DNS서버에 질의 받은 IP 주소의 레코드를 Forward, Reverse Zone 모두 가지고 있는 경우 신뢰가능한 응답을 전송한다

| 순서 | 질의자 | 응답자 | 종류 | 쿼리 내용 |
| --- | --- | --- | --- | --- |
| 1 | 클라이언트 | ns1.daum.net | 질의 | "www.daum.net" |
| 2 | ns1.daum.net에서 자신의 레코드 확인(성공) |  |  |  |
| 3 | 클라이언트 | ns1.daum.net | 응답 | "www.daum.net"의 IP 주소 |

### 2. 신뢰가능하지 않은 응답

DNS에 Forward, Reverse Zone 중 하나라도 없는 경우. DNS 서버에 해당 도메인을 구성하지 않은 호스트, 즉 외부 서버의 주소를 질의 받았을 때이다.

일반적으로 가정집에서 DNS 서버에 질의하면 받게 된다.

| 순서 | 질의자 | 응답자 | 종류 | 쿼리의 내용 |
| --- | --- | --- | --- | --- |
| 1 | 클라이언트 | ISP | 질의 | "www.daum.net" |
| 2 | ISP의 DNS 서버에서 자신의 레코드를 확인 (실패) |  |  |  |
| 3 | ISP | .(Root DNS) | 질의 | net DNS 서버의 도메인 이름 |
| 4 | ISP | .(Root DNS) | 응답 | net DNS 서버의 IP 주소 |
| 5 | ISP | net | 질의 | daum.net DNS 서버의 도메인 이름 |
| 6 | ISP | net | 질의 | daum.net DNS 서버의 IP 주소 |
| 7 | ISP | daum.net | 질의 | "www.daum.net" |
| 8 | ISP | daum.net | 질의 | "www.daum.net"의 IP 주소 |
| 9 | 클라이언트 | ISP | 응답 | "www.daum.net"의 IP 주소 |

## DDNS - Dynamic DNS

동적 DNS로, 유동 ip때문에 생겨난 DNS. 회사, 정부 같은 곳은 고정 IP를 사용하지만, 개인의 경우 사생활 침해의 우려로 유동 IP를 사용하는데, 이 때문에 생겨나게 됐다.

## DNS over TLS

TLS(Transport Layer Security)에서 동작하는 DNS. 인터넷상에서 DNS요청을 할 때에는 53번 포트를 통하게 되는데, 이 경우 패킷 열람이 가능하기 때문에 보안상의 위험이 있다.

이를 방지하기 위해서 TLS를 통해서 DNS를 정보를 통신한다. 사용자간의 인증과 DNS 서버간 통신을 암호화하여 DNS 정보를 열람할 수 없게 하고 중간자 공격을 차단하여 DNS 스푸핑 공격의 가능성을 줄여준다.

이 기술은 DNS 서버가 지원해야지만 의미가 있는 기술이다. DNS 서버 자체가 오염되어 있는 경우 무용지물이된다.

- TLS
    
    [Transport Layer Security - 웹 보안 | MDN](https://developer.mozilla.org/ko/docs/Web/Security/Transport_Layer_Security)
    

## DNS over HTTPS (DoH)

HTTPS를 사용해 사이트를 방문해도, HTTPS의 패킷들은 암호화 되지만, DNS 요청(패킷의 목적지)은 암호화 되지 않는다.

DoH는 DNS 요청을 HTTPS를 통해 GET(혹은 POST) 명령과 JSON 문법으로 DNS 정보에 접근한다. DNS 쿼리가 웹 쿼리로 위장되어 보안성을 높여준다.