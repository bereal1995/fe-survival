---
description: TDD에 대해 알아보자
---

# TDD (Test Driven Development)

## TDD란

테스트 코드를 먼저 작성하고, 그 다음에 실제 코드를 작성하는 방식이다.  
여기서 테스트 코드를 먼저 작성한다는 의미는 테스트 코드를 작성하기 위해 실제 코드를 작성하는 것이다.

### TDD Cycle

1. 실패하는 테스트 코드 작성, 인터페이스와 스펙에 집중 (Red)
2. 테스트 통과하는 실제 코드 작성, 어떤수단으로든 통과에 집중 (Green)
3. 리팩토링, 통과했던 코드를 올바르게 만드는 과정 (Refactor)

## Jest

거의 모든 것을 갖춘 테스팅 도구.  
facebook에서 만든 테스트 도구이다.  
rspec에서 영향을 받아 만들어졌다.  

- [Jest](https://jestjs.io/)
- react, node, vue, typescript 등 다양한 환경에서 사용할 수 있다.
- describe, it을 지원한다.
- expect로 단언 할 수 있다.
- mocking도 지원한다.
- [읽으면좋은자료](https://johngrib.github.io/wiki/junit5-nested/)

## Describe - Context - It 패턴

BDD(Behavior Driven Development)에서 나온 패턴이다.
Given-When-Then 패턴과 비슷하다.

- describe: 테스트 대상을 설명
- context: 테스트 대상이 놓인 상황
- it: 테스트 대상에 대한 구체적인 행동
- 이렇게 패턴을 사용하면 테스트 코드를 읽기 쉽고(계층구조), 테스트 대상을 명확하게 알 수 있다.
- 패턴을 통해 작성하다 보면 테스트 대상에 대해 여러가지 생각을 하게되고, 이를 통해 빠트린 케이스를 발견할 가능성이 높아진다.

## Given - When - Then 패턴

BDD(Behavior Driven Development)에서 나온 패턴이다.
시나리오 기반으로 테스트를 작성할 때 유용하다.

- Given: 동작에 대한 전제 조건
- When: 테스트 대상에 대한 행동
- Then: 예상되는 테스트 결과

## 단위테스트란

Unit Test라고 하며, 테스트 가능한 최소한의 단위로 테스트를 진행하는 것이다.

- 일반적으로 클래스나 함수 단위로 테스트를 진행한다.
- 크기가 작을수록 복잡성이 낮아지고, 테스트가 쉬워진다.
- 테스트가 쉬워지면
  - 버그를 찾기 좋다.
  - 리팩토링, 유지보수, 재사용 등이 쉬워진다.
- react에서는 React Testing Library를 사용하여 단위테스트를 진행하면 좋다.

## [테스트를 왜 해야할까?](https://hhs-organization.gitbook.io/dev-note/dev-note/init-1/testing-library#undefined-1)

이전에 작성한 글 참고
