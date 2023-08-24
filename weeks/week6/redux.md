---
description: redux에 대해 알아보자
---

# Redux

## [Redux?](https://ko.redux.js.org/tutorials/essentials/part-1-overview-concepts/)

Redux는 "액션"이라는 이벤트를 사용하여 애플리케이션 상태를 관리하고 업데이트하기 위한 패턴 및 라이브러리이다.  
상태를 예측 가능한 방식으로만 업데이트하는 규칙을 따라서 전역상태 관리를 할 수 있게 해준다.

### 왜 Redux를 사용하는가?

- 전역 상태 관리를 위해
- 상태를 예측 가능한 방식으로 업데이트하기 위해
  - 개발환경에서 devtool을 사용하여 상태를 쉽게 추적할 수도 있음
- 상태를 불변 객체로 관리하기 위해
- UI로직과 비즈니스 로직을 분리하기 위해

Redux는 만능이 아니기 때문에, 현재 개발하고 있는 애플리케이션에 Redux가 문제를 해결하는데 가장 도움이 되는지 먼저 생각해보고 사용해야 한다.

### Redux 핵심 키워드

- 단방향
  - Redux는 단방향 데이터 흐름을 따른다. (Flux Architecture)
  - 데이터가 항상 일정한 경로를 통해 흐르기 때문에, 데이터의 흐름을 예측할 수 있다.
- 불변성
  - Redux는 상태를 불변 객체로 관리한다.
  - 상태를 예측 가능한 방식으로 업데이트할 수 있다.
  - 상태를 업데이트할 때, 새로운 상태를 반환한다.
  - 상태를 업데이트할 때, 기존 상태를 변경하지 않는다.
- dispatch
  - Action을 전달하는 함수
  - Action을 전달하면, Reducer가 호출된다.
- selector
  - Store의 상태를 조회하는 함수
  - 동일한 데이터를 조회해야할때, 중복을 방지할 수 있다.
- Action
  - Store에게 전달할 데이터를 가지고 있는 객체
  - Type을 가지고 있다.
  - Optional하게 Payload를 가질 수 있다.
  - View에서 사용자의 입력을 받아 Dispatcher에게 전달하는 역할
- Reducer
  - dispatch를 통해 전달받은 Action을 처리하는 함수
  - Action을 받아 처리할 때, Action의 Type을 확인하여 처리한다.
  - 처리가 완료되면, Store에게 변경된 데이터를 전달한다.
  - Store의 상태를 변경하지 않고, 새로운 상태를 반환한다. (불변성)
  - 호출될때 이전 상태와 Action을 인자로 받는다.
- Store
  - Reducer를 통해 데이터를 관리하는 객체
  - Reducer를 통해 데이터를 관리할 때, Action의 Type을 확인하여 처리한다.
  - 처리가 완료되면, View에게 변경된 데이터를 전달한다.
  - Reducer를 통해 상태를 변경한다.
  - 상태를 불변 객체로 관리한다.
  - 상태를 변경할 때, 새로운 상태를 반환한다. (불변성)

<details>

<summary>Reducer 네이밍을 사용하는 이유</summary>

Reducer의 이름은 Array.reduce 메서드에서 영감을 받아서 만들어졌다.  
Array.reduce 메서드는 배열의 각 요소에 대해 주어진 reducer 함수를 실행하고, 하나의 결과값을 반환한다.  
Reducer는 이전 상태와 Action을 인자로 받아서, 새로운 상태를 반환한다.  
이전 상태와 Action을 인자로 받아서, 새로운 상태를 반환하는 함수이기 때문에, Reducer라는 이름을 사용하였다.  
또한, Array.reduce 메서드와 마찬가지로, Reducer는 순수 함수이다.  
순수 함수는 동일한 인자를 받으면, 항상 동일한 결과를 반환한다.  

</details>

## [Reflect](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Reflect#%EC%A0%95%EC%A0%81_%EB%A9%94%EC%84%9C%EB%93%9C)

Reflect는 ES6에서 도입된 내장 객체이다.  
중간에서 가로챌 수 있는 JavaScript 작업에 대한 메서드를 제공하는 내장 객체이다.  
메서드 종류는 [프록시 처리기](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Proxy/Proxy)와 동일하고 Reflect는 함수 객체가 아니기 때문에 new 연산자를 사용하여 인스턴스를 생성할 수 없다.

- JavaScript 내장 객체
- 프록시 처리기와 동일한 메서드를 제공한다.
- 여러가지 정적 메서드를 제공한다.
- Reflect는 함수 객체가 아니기 때문에, new 연산자를 사용하여 인스턴스를 생성할 수 없다.

### reflect-metadata

reflect-metadata는 TypeScript에서 메타데이터를 사용할 수 있게 해주는 라이브러리이다.  
reflect-metadata는 Reflect를 사용하여 구현되었다.

- TypeScript에서 메타데이터를 사용할 수 있게 해주는 라이브러리
- Reflect를 사용하여 구현되었다.
- typescript에서는 reflect-metadata를 사용하여 메타데이터를 사용할 수 있다.
  - typescript가 컴파일될 때 객체의 메타데이터를 정적으로 읽고 쓸 수 있도록 도와준다.
  - typescript는 정적 타입 언어이기 때문에 동적으로 메타데이터를 읽고 쓸수 있는 Reflect를 typescript에서 사용하기 위해 reflect-metadata를 사용한다.

## Redux 따라하기

`src/stores/BaseStore.ts`

```ts
export type Action<Payload> = {
  type: string;
  payload?: Payload;
};

type Reducer<State, Payload> = (state: State, action: Action<Payload>) => State;

type Reducers<State> = Record<string, Reducer<State, any>>;

type Listener = () => void;

export default class BaseStore<State> {
  state: State;
  reducer: Reducer<State, any>;
  listeners = new Set<Listener>();

  constructor(initialState: State, reducers: Reducers<State>) {
    this.state = initialState;

    this.reducer = (state: State, action: Action<any>) => {
      const reducer = Reflect.get(reducers, action.type);
      if (!reducer) {
        return state;
      }

      return reducer(state, action);
    };
  }

  dispatch(action: Action<any>) {
    this.state = this.reducer(this.state, action);
    this.publish();
  }

  publish() {
    this.listeners.forEach(listener => {
      listener();
    });
  }

  addListener(listener: Listener) {
    this.listeners.add(listener);
  }

  removeListener(listener: Listener) {
    this.listeners.delete(listener);
  }
}
```

`src/stores/Store.ts`

```ts
import {singleton} from 'tsyringe';
import BaseStore, {type Action} from './BaseStore';

const initialState = {
  count: 0,
  name: 'tester',
};
export type State = typeof initialState;

const reducers = {
  INCREMENT(state: State, action: Action<number>) {
    return {
      ...state,
      count: state.count + (action.payload ?? 1),
    };
  },
  DECREMENT(state: State, action: Action<number>) {
    return {
      ...state,
      count: state.count - (action.payload ?? 1),
    };
  },
};

export function increment(step?: number) {
  return {type: 'INCREMENT', payload: step};
}

export function decrement(step?: number) {
  return {type: 'DECREMENT', payload: step};
}

@singleton()
export default class Store extends BaseStore<State> {
  constructor() {
    super(initialState, reducers);
  }
}

```

`src/hooks/useDispatch.ts`

```ts
import {container} from 'tsyringe';

import {type Action} from '../stores/BaseStore';
import Store from '../stores/Store';

export default function useDispatch<Payload>() {
  const store = container.resolve(Store);
  return (action: Action<Payload>) => {
    store.dispatch(action);
  };
}
```

`src/hooks/useSelector.ts`

```ts
import {container} from 'tsyringe';
import {useEffect, useRef} from 'react';

import Store, {type State} from '../stores/Store';

import useForceUpdate from './useForceUpdate';

type Selector<T> = (state: State) => T;

export default function useSelector<T>(selector: Selector<T>): T {
  const store = container.resolve(Store);
  const state = useRef(selector(store.state));
  const forceUpdate = useForceUpdate();

  useEffect(() => {
    const update = () => {
      const newState = selector(store.state);
      // TODO: Selector의 결과가 객체인경우 처리 필요함
      if (newState !== state.current) {
        state.current = newState;
        forceUpdate();
      }
    };

    store.addListener(update);
    return () => {
      store.removeListener(update);
    };
  }, [store, forceUpdate]);

  return selector(store.state);
}

```

`src/components/CounterControl.tsx`

```tsx
import useDispatch from '../hooks/useDispatch';
import useSelector from '../hooks/useSelector';

import {decrement, increment} from '../stores/Store';

export default function CountControl() {
  const dispatch = useDispatch();
  const count = useSelector(state => state.count);

  return (
    <div>
      <p>{count}</p>
      <button onClick={() => {
        dispatch(increment());
      }}>increase</button>
      <button onClick={() => {
        dispatch(increment(10));
      }}>increase 10</button>
      <button onClick={() => {
        dispatch(decrement());
      }}>decrease</button>
      <button onClick={() => {
        dispatch(decrement(10));
      }}>decrease 10</button>
    </div>
  );
}
```
