---
description: React Component에 대해 알아보자
---

# React Component

## REST API

### REST API 란 무엇인가

Representational State Transfer의 약자로, 웹에 존재하는 모든 자원(이미지, 동영상, DB 자원 등)에 고유한 URI를 부여해 활용하는 것을 의미한다.  
자원을 정의하고 자원에 대한 주소를 지정하는 방법론을 의미한다.  
HTTP 프로토콜을 의도에 맞게 디자인하도록 유도하고 있다.  

- HTTP URI(Uniform Resource Identifier)를 통해 자원(Resource)을 명시하고, HTTP Method(GET, POST, PUT, DELETE, PATCH)를 통해 해당 자원에 대한 CRUD Operation을 적용하는 것을 의미한다.
- 즉, REST는 [자원 기반의 구조(ROA, Resource Oriented Architecture)](https://en.wikipedia.org/wiki/Resource-oriented_architecture) 설계의 중심에 자원(Resource)과 행위(Verb)가 있고, HTTP Method를 통해 자원을 처리하도록 설계된 아키텍처이다.

### CRUD Operation ??

- Create : 생성(POST)
- Read : 조회(GET)
- Update : 수정(PUT)
- Delete : 삭제(DELETE)

### REST는 왜 등장했는가 ?

1991년에 팀 버너스리에 의해 웹이 세상에 나오고, 고민이 있었다.  
*인터넷에서 정보를 주고 받는 방법은 어떻게 해야할까?*  
1994년도에 HTTP 1.0이 나왔고, 이때 프로토콜 작업에 참여한 로이 필딩은 HTTP Object Model을 설계하면서 REST를 고안했다.  

### REST 구성 스타일

- Client-Server
- Stateless
- Cache
- Uniform Interface
- Layered System
- Code-On-Demand(Optional)

HTTP만 잘 따라도 위의 조건을 대부분 지키고 있다.  
하지만 여기서 Uniform Interface는 조금 다르다.

#### Uniform Interface

- Identification of resources: 리소스가 URI로 식별되어야 한다.
- Manipulation of resources through representations: 리소스를 생성,수정,삭제,조회하기 위한 표준 방식이 존재해야 한다.
- Self-descriptive messages: 메시지는 스스로를 설명해야 한다.
- Hypermedia as the engine of application state(HATEOAS): 애플리케이션의 상태는 Hyperlink를 이용해 전이되어야 한다.

Uniform Interface가 필요한 이유는 다음과 같다.

- 서버와 클라이언트의 역할을 명확하게 분리하기 위해
  - 서버와 클라이언트가 독립적으로 개발될 수 있음
- 클라이언트의 의존성을 줄이기 위해
  - 클라이언트가 서버의 상태를 알 필요가 없음
- 서버의 응답을 캐싱하기 위해
  - 응답을 캐싱하면 클라이언트의 응답시간이 줄어듬
- 서버의 기능을 단순화시키기 위해
  - 서버의 기능이 단순해지면 클라이언트의 구현이 단순해짐
- 서버와 클라이언트의 개발 독립성을 높이기 위해
  - 서버와 클라이언트가 독립적으로 개발되면 서버와 클라이언트의 개발 속도가 빨라짐

## GraphQL

### GraphQL은 왜 등장했는가?

REST API는 클라이언트가 서버에게 요청을 보내면 서버가 정해놓은 규칙에 따라 응답을 보내주는 방식이다. 이 방식은 클라이언트가 요청하는 데이터의 종류에 따라 응답의 크기가 달라지는 단점이 있다. 이를 해결하기 위해 등장한 것이 GraphQL이다. GraphQL은 클라이언트가 요청하는 데이터의 종류에 따라 응답의 크기가 달라지는 것이 아니라 클라이언트가 요청한 데이터의 종류에 따라 응답의 크기가 달라지는 방식이다.

### GraphQL의 특징

- 쿼리를 선언하여 데이터를 요청한다.
- 클라이언트가 요청한 데이터의 종류에 따라 응답의 크기가 달라진다.
- client와 server사이에 새로운 계층을 두어서 새로운 계층에서 데이터를 조작한다. 즉 client에서는 서버에 요청하는것이 아니라 새로운 계층에 요청하고 새로운 계층에서 서버에 요청한다.

## REST API vs GraphQL

| - | REST API | GraphQL |
|---|---|---|
|요청방법| GET, POST, PUT, DELETE | Query |
|요청데이터| 요청시 Body에 데이터를 담아서 전송 | Query String |
|응답데이터| JSON | JSON |
|응답속도| 빠름 | 느림 |
|요청데이터의 종류에 따른 응답의 크기| 응답의 크기가 달라지지 않음 | 응답의 크기가 달라짐 |
|요청데이터의 크기에 따른 응답속도| 응답속도가 달라지지 않음 | 응답속도가 달라짐 |
|다중 자원 요청 시| 다중 자원 요청 시 여러번 요청을 보내야 함 | 다중 자원 요청 시 한번의 요청으로 보낼 수 있음 |

## JSON

JavaScript Object Notation의 약자로 데이터를 저장하거나 전송할 때 많이 사용되는 경량의 DATA 교환 형식이다.

- 자바스크립트의 객체 표기법을 따르고 있으며, 언어 독립적인 텍스트 형식이다.
- XML을 대체하고 있다.
- 서버와 클라이언트 간의 데이터 교환 형식으로 많이 사용되고 있다.
- 데이터의 구조가 단순하고, 가독성이 좋으며, 용량이 작다.
- 대부분의 프로그래밍 언어에서 JSON을 처리할 수 있는 라이브러리를 제공하고 있다.

## DSL(Domain-Specific Language)

특정 도메인을 위해 설계된 언어를 말한다.

- 특정 도메인에 특화된 언어
- 특정 도메인에 특화된 언어이기 때문에 러닝커브가 있지만, 해당 도메인의 전문가는 사용하기 쉽다.
- React는 JSX라는 DSL을 사용한다.

## 선언형 프로그래밍

Declarative Programming이라고도 한다. 프로그램이 어떻게 동작하는지 보다는 무엇을 하는지에 중점을 둔다. 즉, 코드의 추상화 수준이 높다.

```js
// 선언형 프로그래밍

const numbers = [1, 2, 3, 4, 5];

const result = numbers.map((number) => number * 2);

console.log(result); // [2, 4, 6, 8, 10]
```

- 코드의 추상화 수준이 높다.
- 코드의 재사용성이 높다.
- 코드의 가독성이 높다.
- 코드의 유지보수가 쉽다.
- 대표적으로 함수형 프로그래밍이 있다.
- React는 선언형 프로그래밍을 지향한다.

## 명령형 프로그래밍

Imperative Programming이라고도 한다. 프로그램이 어떻게 동작하는지에 중점을 둔다. 즉, 코드의 추상화 수준이 낮다.

```js
// 명령형 프로그래밍
const numbers = [1, 2, 3, 4, 5];

const result = [];

for (let i = 0; i < numbers.length; i++) {
  result.push(numbers[i] * 2);
}

console.log(result); // [2, 4, 6, 8, 10]
```

- 코드의 추상화 수준이 낮다.
- 코드의 재사용성이 낮다.
- 코드의 가독성이 낮다.
- 코드의 유지보수가 어렵다.
- 대표적으로 객체지향 프로그래밍이 있다.

## SRP(단일 책임 원칙)

Single Responsibility Principle의 약자로 하나의 클래스는 하나의 책임만 가져야 한다는 원칙이다.

- 하나의 클래스는 하나의 책임만 가져야 한다.
- 클래스가 변경되는 이유는 오직 하나여야 한다.
- 클래스가 가지는 책임이 많아질수록 코드의 재사용성이 떨어진다.
- React는 컴포넌트를 만들 때 SRP를 지키도록 한다.

## Atomic Design

Atomic Design은 웹을 구성하는 기본 단위인 Atom, Molecule, Organism, Template, Page로 구성된다.  
위에 말한 SRP를 지키기 쉽도록 도와준다.

- Atom: 웹을 구성하는 기본 단위
- Molecule: Atom을 조합한 단위
- Organism: Atom과 Molecule을 조합한 단위
- Template: Organism을 배치한 단위
- Page: Template을 배치한 단위
- 컴포넌트를 재사용하기 쉽게 만들어주고 컴포넌트의 구조를 일관성 있게 만들어준다.
- SRP를 지키기 쉽다.

## React component 와 props

### Extract Function

함수를 추출하는 리팩토링 기법이다.

- 함수를 추출하면 코드의 재사용성이 높아진다.
- 함수를 추출하면 코드의 가독성이 높아진다.
- 함수를 추출하면 코드의 유지보수가 쉬워진다.
- 리액트 컴포넌트도 함수이기 때문에 적용할 수 있다.
- 우선 기능을 구현하고, 중복되는 코드가 발생하면 함수를 추출한다.

### props

props는 부모 컴포넌트가 자식 컴포넌트에게 주는 값이다. 자식 컴포넌트에서는 props를 받아오기만 하고, 받아온 props를 직접 수정할 수는 없다.

- props는 부모 컴포넌트가 자식 컴포넌트에게 주는 값이다.
- props를 통해 컴포넌트간의 데이터를 전달할 수 있다. (단방향)
- callBack 함수를 통해 자식 컴포넌트에서 부모 컴포넌트의 데이터를 변경할 수 있다. (양방향)
- props의 값이 변경되면 해당 props를 사용하는 컴포넌트는 리렌더링 된다.

## 추가로 궁금하거나 알아볼 것들

- React에서 컴포넌트를 디자인할 때 Atomic Design을 사용하는 것이 좋은가?
  - 이외에도 컴포넌트를 디자인할 때 사용하는 방법들
- 컴포넌트 잘게 추상화를 하면 재사용성이 높아지는 것은 맞는 것인가?
  - 재사용성이 높아지는 것이 꼭 좋은 것인가?
- 컴포넌트 추상화 레벨이 높을수록 재사용성은 좋아질것 같지만, 유지보수하기 어려워지는 것은 아닌가?
