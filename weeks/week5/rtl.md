---
description: React Testing Library에 대해 알아보자
---

# React Testing Library

## React Testing Library?

좋은 테스트 관행을 장려하는 간단하고 완벽한 테스트 유틸리티

- [공식문서](https://testing-library.com/docs/)
- 사용자 중심 테스팅: 사용자가 실제로 어떻게 앱을 사용하는지에 초점을 맞추어 테스트 작성.
- DOM에 의존하지 않음: React Testing Library는 DOM에 의존하지 않으며, React 컴포넌트를 테스트.
- 가독성 높은 테스트 코드: 테스트 코드가 가독성이 높고 유지보수하기 쉽다.
- React 생태계와 통합: React Testing Library는 React 생태계와 통합되어 있으며, React 컴포넌트를 테스트하기 위한 다양한 유틸리티 함수를 제공.
- 테스트 코드의 신뢰성 향상: 사용자 중심 테스팅을 통해 테스트 코드의 신뢰성을 향상.
- 테스트 코드의 유지보수성 향상: 가독성이 높은 테스트 코드를 작성함으로써 테스트 코드의 유지보수성을 향상.

### 기본 사용법

```tsx
import {render, screen} from '@testing-library/react';

import App from './App';

test('App', () => {
  render(<App />);

  screen.getByText('Apple'); // Apple이라는 텍스트가 있는지 확인
});
```

## given - when - then 패턴

given - when - then 패턴은 테스트 코드를 작성할 때 사용하는 패턴 중 하나인데, 이 패턴은 테스트 코드를 구성하는 세 가지 부분으로 나누어 작성.

- given: 테스트를 수행하기 위해 필요한 초기 상태를 설정.
- when: 테스트를 수행하는데 필요한 동작을 수행.
- then: 테스트 결과를 검증.
- given - when - then 패턴을 사용하면 테스트 코드를 구성하는 세 가지 부분으로 나누어 작성함으로써, 테스트 코드의 가독성과 유지보수성을 높일 수 있다.

### RTL을 사용한 given - when - then 패턴

```tsx
import {fireEvent, render, screen} from '@testing-library/react';

import TextField from './TextField';

const context = describe;

describe('TextField', () => {
  const label = 'Name';
  const value = 'Tester';
  const setText = jest.fn();

  beforeEach(() => {
   jest.clearAllMocks(); // 모든 mock 함수 초기화
  });

  function renderTextField() { // given이면서 when이라고 볼수있는데 왜냐하면 렌더링을 하면서 테스트를 수행하기 때문이다.
    render((
      <TextField
        label={label}
        placeholder='Input your name'
        text={value}
        setText={setText}
      />
    ));
  }

  function inputText(value: string) { // when
    fireEvent.change(screen.getByLabelText(label), {
      target: {value},
    });
  }

  it('renders a text field', () => {
    renderTextField();

    // Then
    screen.getByLabelText(label);
    screen.getByDisplayValue(value);
    screen.getAllByPlaceholderText(/name/);
  });

  context('when the user types in the text field', () => {
    beforeEach(() => {
      // Given
      renderTextField();
    });

    it('calls the setText function', () => {
      // When
      inputText('New Name');

      // Then
      expect(setText).toBeCalledWith('New Name');
    });
  });
});

```

## Mocking

Mocking은 테스트 코드에서 테스트 대상 코드가 의존하는 코드를 대체하는 것을 의미한다.  
테스트 대상 코드가 의존하는 코드를 대체함으로써, 테스트 대상 코드를 독립적으로 테스트할 수 있다.

### Mocking을 사용하는 이유

- 테스트 대상 코드가 의존하는 코드가 테스트 환경에 존재하지 않을 때
- 테스트 대상 코드가 의존하는 코드가 테스트 환경에 존재하지만,
  - 테스트 환경에 존재하는 의존하는 코드를 사용할 수 없을 때
  - 테스트 환경에 존재하는 의존하는 코드를 사용하면 테스트 코드를 작성하기 어려울 때

예를 들어 아래와 같은 상황에서 Mocking을 사용할 수 있다.

```tsx
import {fireEvent, render, screen} from '@testing-library/react';

import TextField from './TextField';

const context = describe;

describe('TextField', () => {
  const label = 'Name';
  const value = 'Tester';
  const setText = jest.fn();

  beforeEach(() => {
    jest.clearAllMocks();
  });

  function renderTextField() {
    render((
      <TextField
        label={label}
        placeholder='Input your name'
        text={value}
        setText={setText}
      />
    ));
  }
});
```

TextField 컴포넌트 입장에서는 setText함수는 그저 props로 전달받은 함수일 뿐이다.  
그렇기 때문에 테스트를 작성할 때 함수의 구현 내용을 신경쓰는것보다는 제대로 전달받은 함수를 호출하는지가 중요하다.  
이럴때, Mocking을 사용하면 테스트 대상 코드가 의존하는 코드를 대체함으로써, 테스트 대상 코드를 독립적으로 테스트할 수 있다.

## Test fixture

fixture는 고정된 상태를 가지는 오브젝트를 의미한다.  
Test fixture는 테스트를 수행하기 위해 필요한 상태를 설정하는 코드를 의미한다.  
Test fixture 목적은 결과를 반복 가능하고 고정된 환경에서 테스트 할 수 있음을 보장하기 위함이다.  
즉, 테스트를 수행하기 위해 필요한 중복되는 코드를 fixture로 분리함으로써, 테스트 코드의 가독성과 유지보수성을 높일 수 있다.

### RTL 환경에서 fixture 사용해보기

#### 처음 코드

```tsx
// App.test.tsx
import {render, screen} from '@testing-library/react';

import App from './App';

jest.mock('./hooks/useFetchProducts', () => () => [
  {
    category: 'Fruits',
    name: 'Apple',
    price: '$1',
    stock: true,
  },
]);

test('App', () => {
  render(<App />);

  screen.getByText('Apple');
});

```

useFetchProducts를 다른 테스트 코드에서도 많이 사용할것 같고, 그때마다 위처럼 작성하는것은 비효율적으로 보인다.  
때문에 fixture로 분리하여 가독성과 유지보수성을 높여보도록 하자.

#### 중복되는 코드 분리

```tsx
// fixtures/products.ts
  const products = [
  {
    category: 'Fruits',
    name: 'Apple',
    price: '$1',
    stock: true,
  },
];

export default products;


// fixtures/index.ts
import products from './products';
// ...

export default {
  products,
  // ...
};

```

#### 테스트 코드

```tsx
// App.test.tsx
import {render, screen} from '@testing-library/react';

import {App} from './App';
import fixtures from '../fixtures';

jest.mock('./hooks/useFetchProducts', () => fixture.products);

describe('App', () => {
  it('renders a product list', () => {
    render((
      <App/>
    ));

    screen.getByText('Apple');
  });
});
```

#### 위 코드에서 `useFetchProducts`를 Mock파일로 분리하면 아래와 같이 사용할 수 있다

```tsx
// hooks/__mocks__/useFetchProducts.ts
import fixtures from '../../../fixtures';

const useFetchProducts = jest.fn(() => fixtures.products);

export default useFetchProducts;
```

#### 테스트 코드 (Mock파일 분리)

```tsx
// App.test.tsx
import {render, screen} from '@testing-library/react';

import {App} from './App';
import fixtures from '../fixtures';

jest.mock('./hooks/useFetchProducts'); // useFetchProducts를 Mock파일로 분리

describe('App', () => {
  it('renders a product list', () => {
    render((
      <App/>
    ));

    screen.getByText('Apple');
  });
});
```
