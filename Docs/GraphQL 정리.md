# GraphQL 정리

Created: 2021년 8월 11일 오전 10:19
링크: https://tech.kakao.com/2019/08/01/graphql-basic/

## GraphQL이란?

페이스북에서 만든 Query Language. SQL과 마찬가지로 쿼리 언어지만, 탄생배경과 목적이 다르다.

GQL

- 목적 : 웹 클라이언트가 데이터를 서버로부터 효율적 부름
- 따라서 클라이언트에서 작성, 호출

```graphql
{
	hero {
		name
		friends{
			name
		}
	}
}
```

SQL

- 목적 : DB에 저장된 데이터를 효율적으로 부름
- 따라서 백엔드에서 작성, 호출

```sql
SELECT plot_id, species_id From surveys
```

처리 순서

1. (클라이언트) gql 쿼리로 요청
2. (서버사이드) gql 처리 결과 클라이언트로 전송

특징

1. HTTP API가 특정 DB, 플랫폼에 종속적이지 않은것 처럼, gql 또한 종속적이지 않음

    심지어 네트워크 방식에도 종속적이지 않음.

2. 일반적으로 gql은 네트워크 레이어 L7의 HTTP POST 메서드와, 웹소켓 프로토콜을 활용. 필요시 TCP/UDP, 심지어 L2형식의 이더넷 프레임까지도 활용 가능.

![GraphQL%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5/Untitled.png](GraphQL%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5/Untitled.png)

GraphQL 파이프라인

## REST API와 비교

- REST API 는 URL, METHOD등의 조합으로 다양한 Endpoint가 존재. 반면 GraphQL은 단 하나의 Endpoint가 존재.
- GraphQL API에서는 불러오는 데이터의 종류를 쿼리 조합을 통해 결정.
REST API는 Endpoint마다 SQL 쿼리가 달라지는 반면, GraphQL은 스키마의 타입마다 SQL 쿼리가 달라짐.

![GraphQL%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5/Untitled%201.png](GraphQL%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5/Untitled%201.png)

HTTP와 GraphQL 기술 스택

![GraphQL%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5/Untitled%202.png](GraphQL%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5/Untitled%202.png)

REST API와 GraphQL API 사용

위 그림처럼, GraphQL API는 한번의 호출로 처리 가능.

## 쿼리/뮤테이션

```graphql
// 쿼리
{
	hero{
		name
	}
}

// 응답
{
	"data": {
		"hero": {
			"name": "R2-D2"
		}
	}
}
```

### 쿼리와 뮤테이션

GraphQL에서는 쿼리와 뮤테이션을 나누는데, 내부적으로는 큰 차이는 없음. 쿼리는 데이터를 **읽는데** 사용하고, 뮤테이션은 데이터를 **변조하는데** 사용

### 일반 쿼리와 오퍼레이션 네임 쿼리

```graphql
{
  human(id: "1000") {
    name
    height
  }
}

query HeroNameAndFriends($episode: Episode) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
```

일반적으로 클라이언트에서 정보를 불러올때 id값이나 다른 인자값을 가지고 데이터를 불러 옴. GraphQL는 이러한 경우를 위해 쿼리에 변수라는 개념이 있음.

```graphql
query getStudentInfomation($studentId: ID){
  personalInfo(studentId: $studentId) {
    name
    address1
    address2
    major
  }
  classInfo(year: 2018, studentId: $studentId) {
    classCode
    className
    teacher {
      name
      major
    }
    classRoom {
      id
      maintainer {
        name
      }
    }
  }
  SATInfo(schoolCode: 0412, studentId: $studentId) {
    totalScore
    dueDate
  }
}
```

**오퍼레이션** 네임 쿼리는 쿼리용 함수. DB의 프로시저와 유사. 프로시저처럼 한번의 네트워크 왕복으로 원하는 모든 데이터를 가져올 수 있음. 프로시저는 DBA 혹은 백엔드 프로그래머가 작성/관리 하지만, gql 오퍼레이션 네임 쿼리는 클라이언트 프로그래머가 작성/관리

때문에 백엔드, 프론트엔드 프로그래머의 협업에도 영향을 줌. REST API에서는 백엔드 프로그래가 작성한 API의 request/response의 형식에 의존하게 되지만, gql을 사용하면 의존도가 많이 사라짐.

## 스키마/타입

C, C++의 헤더파일 작성과 비슷

```graphql
type Character {
  name: String!
  appearsIn: [Episode!]!
}
```

- 타입 : Character
- 필드 : name, appearIn
- 스칼라 타입: String, ID, Int 등
- 느낌표(!): 필수값(non-nullable)

## Resolver

DB에는 DB 어플리케이션을 사용하여 데이터를 가져오는 과정이 구현이 되어있음. 하지만 gql에서는 과정을 직접 구현해야함. gql 쿼리 파싱은 대부분의 gql 라이브러리에서 처리하지만, 데이터를 가져오는 과정은 resolver가 담당하고, 이를 직접 구현해야함. 부담은 있지만, 데이터 source의 종류에 상관 없이 구현 가능. 예를 들어 resolver를 통해 DB에서 가져올수도, 일반 파일에서도, 심지어 http, SOAP와 같은 네트워크 프로토콜을 활용해서 원격 데이터를 가져올 수도 있음.

gql 쿼리에서는 각 필드마다 함수 하나씩 존재. 이 함수는 다음 타입을 반환하는데, 이 함수를 resolver라고 함. 필드가 스칼라 값(문자열, 숫자 같은 primitive 타입)이면 실행 종료. 즉, 연쇄적인 리졸버 호출이 일어나지 않음. 하지만 필드 타입이 스칼라가 아니라 우리가 정의한 타입이면 해당 타입의 리졸버를 호출.

연속적 resolver 호출은 DFS로 구현되었을 것으로 추측됨. (그래서 이름이 Graph이지 않을까...) 연쇄 resolver를 잘 활용 하면 DBMS의 관계에 대한 쿼리를 쉽고 효율적으로 처리 가능.

```graphql
type Query {
  users: [User]
  user(id: ID): User
  limits: [Limit]
  limit(UserId: ID): Limit
  paymentsByUser(userId: ID): [Payment]
}

type User {
	id: ID!
	name: String!
	sex: SEX!
	birthDay: String!
	phoneNumber: String!
}

type Limit {
	id: ID!
	UserId: ID
	max: Int!
	amount: Int
	user: User
}

type Payment {
	id: ID!
	limit: Limit!
	user: User!
	pg: PaymentGateway!
	productName: String!
	amount: Int!
	ref: String
	createdAt: String!
	updatedAt: String!
}
```

User와 Limit의 관계는 1:1, User와 Payment관계는 1:n

```graphql
{
  paymentsByUser(userId: 10) {
    id
    amount
  }
}

//////

{
  paymentsByUser(userId: 10) {
    id
    amount
    user {
      name
      phoneNumber
    }
  }
}
```

동일한 쿼리명이지만, 아래가 더 많은 resolver가 호출된다. 재귀형의 resolver 체인을 잘 활용한다면, 효율적인 설계까 가능하다.

resolver 함수는 총 4개의 인자

```graphql
Query: {
    paymentsByUser: async (parent, { userId }, context, info) => {
        const limit = await Limit.findOne({ where: { UserId: userId } })
        const payments = await Payment.findAll({ where: { LimitId: limit.id } })
        return payments        
    },  
  },
  Payment: {
    limit: async (payment, args, context, info) => {
      return await Limit.findOne({ where: { id: payment.LimitId } })
    }
  }
```

파라미터 설명

- parent: 연쇄적 resolver 호출에서 부모 resolver가 리턴한 객체.
- arps: 쿼리에서 입력으로 넣은 인자
- context: 모든 resolver에 전달되는 인자.
- info: 스키마 정보, 현재 쿼리의 특정 필드 정보를 가진 인자.

## 인트로스펙션(introspection)
