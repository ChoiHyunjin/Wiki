# What is Domain Name?

[What is a domain name? - Web 개발 학습하기 | MDN](https://developer.mozilla.org/ko/docs/Learn/Common_questions/What_is_a_domain_name)

인터넷에 연결된 모든 컴퓨터는 IP 주소를 통해 접근이 가능하다. IP는 IPv4(32bits), IPv6(128bit) 두 가지가 있지만 모두 사람이 기억하기는 어렵다. 이를 해결 하기 위해서, 사람이 읽고 기억하기 좋도록 만들어 진 것이 도메인 이름이다.

# 구조

일반적으로 Domain Name 은 오른쪽에서부터 해석이 가능하다.

![Untitled](images/What%20is%20Domain%20Name/Untitled.png)

## TLD (Top-Level Domain)

가장 오른쪽에 위치한 Domain. 해당 웹사이트의 일반적인 목적을 알 수 있다. (ex. .com / .org 등)

### Label

하나의 단어부터 완벽한 문장까지, 무엇이든 올 수 있다. label의 갯수에는 제한이 없다.

TLD 바로 왼쪽의 Label은 SLD(Second-Level Domain)이라고도 부른다.

mozilla.org 에서 따로 추가적인 도메인을 얻고 싶다면 developer.mozilla.org와 같이 라벨을 추가할 수 있다.

# 구매하기

## 누가 가지고 있는가?

## 이용가능한 Domain Name 찾기

## 도메인 이름 얻기

## DNS 갱신

# DNS 리퀘스트

1. 사용자가 브라우저에 [mozilla.org](http://mozilla.org) 입력
2. 브라우저는 컴퓨터에 저장된 DNS cache 검색
3. cache에 mozilla.org가 
    1. 있으면, 해당 IP를 가져와서 웹페이지 작성 시작
    2. 없으면, DNS 서버에 [mozilla.org](http://mozilla.org) 에 대한 IP 주소 요청.
