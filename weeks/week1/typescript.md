---
description: TypeScript에 대해 알아보자
---

# 타입스크립트

## REPL(Read Eval Print Loop)

- 사용자가 입력한 명령어를 읽고(R) 명령어를 평가하고(E) 결과를 출력한 다음(P) 다시 입력을 기다리는 상태를 반복(L)
- typescript에서는 아래처럼 ts-node를 실행하여 간단하게 사용할 수 있다.

  ```bash
  npx ts-node
  ```

## TypeScript

`타입스크립트는 자바스크립트를 기반으로 하는 강력한 프로그래밍 언어로, 규모에 관계없이 더 나은 툴을 제공합니다.` 라고 사이트에 나와 있다

### Interface vs Type

- 둘이 매우 유사하다.
- 큰 차이는 타입은 새 프로퍼티를 추가하도록 개방될 수 없고, 인터페이스는 확장할 수 있다.

  ```typescript
  // 인터페이스는 새 프로퍼티 추가 가능
  interface Window {
    name: string;
  }

  interface Window {
    ts: TypescriptAPI;
  }

  const foo: Window = {
    name: 'foo',
    ts: TypescriptAPI,
  }

  // 타입은 생성한 뒤에는 달라질 수 없다.
  type Window = {
    title: string
  }

  type Window = {
    ts: TypeScriptAPI
  }

  // Error: Duplicate identifier 'Window'.
  ```

- 확장하는 방법이 다르다.

  ```typescript
  interface Person {
    name: string;
    age: number;
  }
  interface Developer extends Person {
    language: string;
  }

  type Person = {
    name: string;
    age: number;
  };
  type Developer = Person & {
    language: string;
  };
  ```

- 인터페이스는 오직 객체 모양을 선언하는 데에만 사용할 수 있다.

  ```typescript
  interface X extends string {

  }

  // An interface cannot extend a primitive type like 'string'; an interface can only extend named types and classes(2840)

  type X = string; // 가능!
  ```

- 문서에는 우선 interface를 사용하고 이후 문제가 발생하였을 때 type을 사용하라고 나와있다.
- 개인적인 생각은 interface, Type은 개인적인 선호에 따라 사용하면 괜찮다고 생각하지만, 프로젝트마다 어느정도 컨벤션을 정해 일관성 있게 사용하는 것이 더 중요하다고 생각이 든다.

### 타입 추론

typescript는 Javascript 언어를 알고 있어 대부분의 경우 타입을 생성해준다. 예를 들어 변수를 생성할때 값을 할당하면 그 값을 변수의 타입으로 지정해준다.
  
  ```typescript
  let foo = 123; // foo: number

  foo = '123' // Type 'string' is not assignable to type 'number'.
  ```

### Union Type vs Intersection Type

- Union Type은
  - `|`를 사용하여 표현한다.
  - 많이 사용되는 사례중 하나는 리터럴 집합을 설명할 때 사용된다.

    ```typescript
    type Direction = 'left' | 'right' | 'up' | 'down';
    type Age = 10 | 20 | 30 | 40;
    ```

  - 이외에도 여러가지 타입을 허용해야하는 경우나 타입을 확정할 수 없는 경우에 사용된다.
- Intersection Type은
  - `&`를 사용하여 표현한다.
  - 두개 이상의 타입을 조합하여 만든다.

  ```typescript
  interface ColorRGB {
    r: number;
    g: number;
    b: number;
  }
  
  interface Alpha {
    a: number;
  }

  type ColorRGBA = ColorRGB & Alpha;

  ```

### Optional Parameter

- 함수의 파라미터에 `?`를 붙여서 사용한다.

  ```typescript
  function foo(bar?: number) {
    // ...
  }
  ```

- `?`를 사용하면 해당 파라미터는 필수가 아니게 된다.
- 파라미터에 기본값을 설정할 수 있다.

  ```typescript
  function foo(bar: number = 0) { // bar?: number 와 같다.
    // ...
  }
  ```
