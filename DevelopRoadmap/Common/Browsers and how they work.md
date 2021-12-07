# Browsers and how they work?

[What is a web browser?](https://www.mozilla.org/en-US/firefox/browsers/what-is-a-browser/)

# **How does a web browser work?**

웹 브라우저는 이미지, 동영상 등의 HTML(Hypertext Markup Language) 형태의 자원을 HTTP를 통해 받아 온다. 웹 브라우저는 받아온 HTML을 rendering engine으로 해석해서 사용자에게 제공한다.

그치만 모든 브라우저가 동일하게 해석하지 않는다. 그래서 사용자는 기능이 다르다고 느끼는 경우가 있다. 이 편차를 줄이기 위해 web standard가 있다.

hyperlink 는 다른 페이지로 넘어갈 수 있는 링크이다.

웹 사이트에 있는 이미지, 동영상 등의 자원은 URL(Unique Resource Locator)를 가진다.

# **Cookies (not the yummy kind)**

일반적으로 쿠키는 다음 번 방문을 위해 웹 사이트 정보들을 저장하는 것. 아이디, 패스워드 저장, 웹 브라우저 사용 패턴과 같은 것들이 있다. 이 정보들을 취합해서 사용자에게 더 알맞는 정보들을 제공하기도 하는데, 광고에서도 많이 사용된다.

third-party cookie 라고 불리는 것들도 있는데, 그 당시에 방문하지도 않은 사이트에서 사용자의 정보를 모으기 위해 사이트 이동 정보를 수집하기도 한다. 이들은 심지어는 팔리기도 한다.

---

[How does web browsers work?](https://medium.com/@monica1109/how-does-web-browsers-work-c95ad628a509)

# High-level architecture of browser

![Untitled](images/Browsers%20and%20how%20they%20work/Untitled.png)

### 1. User Interface

사용자가 브라우저와 소통하는 영역. 검색창, 앞뒤 버튼 등

### 2. Browser Engine

UI와 Rendering Engine 사이에 있는 브릿지. 유저 인터페이스에서 입력을 받아 렌더링 엔진에 요청한다.

### 3. Rendering Engine

요청된 페이지를 UI로 그리는 엔진. HTML, XML 문서와 CSS 포맷의 이미지들을 해석하고 레이아웃을 그린다.

브라우저 마다 사용 엔진이 다르다.

*구글 크롬 등의 일부 웹 브라우저는 각 탭마다 분리된 프로세스에서 각각의 렌더링 엔진을 가진다.*

- Internet Explorer: Trident
- Firefox & other Mozilla browsers: Gecko
- Chrome & Opera 15+: Blink
- Chrome (iPhone) & Safari: Webkit

Redering 순서

![Untitled](images/Browsers%20and%20how%20they%20work/Untitled%201.png)

1. HTML을 해석해서 DOM(Document Object Model) tree로 생성
- DOM 이란?
    
    HTML/XML 문서에 접근하기 위한 인터페이스. 문서의 모든 요소에 대해 정의하고 있고, 각 요소에 접근하는 방법을 제공한다.
    
    [코딩교육 티씨피스쿨](http://tcpschool.com/javascript/js_dom_concept)
    
1. 1번이 진행되는 동안 Render tree 생성 (CSSOM : CSS Object Model)
    - Render Tree는 비주얼 적인 요소들의 tree
2. 2번 이후 Render Tree의 Layout(위치, 크기 등)을 잡는다.
3. Painting: 실제 화면에 나타낸다. UI Backend layer를 사용한다.

### 4. Networking

브라우저의 구성요소로, HTTP와 FTP를 이용하여 URL을 검색한다. 

### 5. Javascript Interpreter

브라우저의 한 컴포넌트로, 웹 페이지에 있는 자바스크립트 코드를 해석하고 실행한다. 해석 결과는 Render Engine으로 보내 그리도록 한다. 만약 스크립트가 외부에 있다면, 첫 번째 자원을 네트워크를 통해 불러온다. 

### 6. UI Backend

실제 UI를 그리는 영역. 플랫폼에 종속되지 않는 UI를 작성한다.

### 7. Data Persistence/Storage

[How Do Web Browsers Work? | Hacker Noon](https://hackernoon.com/how-do-web-browsers-work-40cefd2cb1e1)
