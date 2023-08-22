---
description: TSyringe에 대해 알아보자
---

# TSyringe

## [TSyringe?](https://github.com/microsoft/tsyringe)

Typescript용 DI(의존성 주입 컨테이너)라이브러리  
PropDrilling을 피하기 위해 사용할 수 있다. (React의 경우 Context API를 사용할 수도 있다.)  

### 설치 및 사용법

설치

```shell
npm i tsyringe reflect-metadata
```

- reflect-metadata는 TSyringe에서 사용하는 데코레이터를 사용하기 위해 설치한다.

사용법

src/main, src/setupTests에 reflect-metadata를 import한다.

```ts
import 'reflect-metadata';
```

src/stores/CounterStore.ts

```ts
import {singleton} from 'tsyringe';

type Listener = () => void;

@singleton()
export default class CounterStore {
  count = 0;
  listeners = new Set<Listener>();

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

  increment() {
    this.count++;
    this.publish();
  }

  decrement() {
    this.count--;
    this.publish();
  }
}
```

사용 예시

```tsx
// Counter.tsx
import useCounterStore from '../hooks/useCounterStore';

export default function CountControl() {
  const store = useCounterStore();

  const handleClickIncrease = () => {
    store.increment();
  };

  const handleClickDecrease = () => {
    store.decrement();
  };

  return (
    <div>
      <p>{store.count}</p>
      <button onClick={handleClickIncrease}>increase</button>
      <button onClick={handleClickDecrease}>decrease</button>
    </div>
  );
}

// useCounterStore.ts
import {useEffect} from 'react';
import {container} from 'tsyringe';

import CounterStore from '../stores/CounterStore';
import useForceUpdate from './useForceUpdate';

export default function useCounterStore() {
  const store = container.resolve(CounterStore);
  const forceUpdate = useForceUpdate();
  useEffect(() => {
    store.addListener(forceUpdate);
    return () => {
      store.removeListener(forceUpdate);
    };
  }, []);

  return store;
}

```

테스트 코드에서 사용시 초기화를 해줘야 한다.

```ts
import {container} from 'tsyringe';

beforeEach(() => {
  container.clearInstances();
});
```

## 의존성 주입(Dependency Injection)

소프트웨어 엔지니어링에서 의존성 주입(dependency injection)은 하나의 객체가 다른 객체의 의존성을 제공하는 테크닉이다.
객체의 생성과 사용을 분리하여 가독성, 재사용성, 유지보수성을 향상시킨다.

- 객체의 생성과 사용을 분리하여 가독성, 재사용성, 유지보수성을 향상시킨다.
- 클라이언트입장에서는 서비스를 주입받기 때문에 책임을 주입받은 서비스에게 위임한다.
- React로 생각해보면 컴포넌트에서 외부에서 객체를 주입받아 사용하는 것을 의미한다.
  - 외부에서 값을 주입받는 것을 의존성 주입이라고 볼 수 있다.
- 순수함수를 만들때 순수함수는 주어진 인자값으로만 결과를 반환해야 하므로, 외부에서 값을 주입받아 사용해야 한다.
  - 외부에서 값을 주입받는 것을 의존성 주입이라고 볼 수 있다.

## [reflect-metadata](https://www.npmjs.com/package/reflect-metadata)

TSyringe에서 사용하는 데코레이터를 사용하기 위해 사용하는 라이브러리  

- 데코레이터는 클래스, 메서드, 프로퍼티, 파라미터에 사용할 수 있다.
- 데코레이터는 함수로 호출되며, 데코레이터가 적용된 대상을 인자로 전달받는다.
- 데코레이터는 실행 시점에 실행된다.
- 데코레이터는 실행 시점에 실행되기 때문에, 런타임에 동적으로 데코레이터를 추가할 수 있다.

## singleton (싱글톤)

이름 그대로 하나의 인스턴스만 생성하는 디자인 패턴이다.  
즉, 하나의 객체만 생성하고, 이 객체를 공유해서 사용한다.

- 하나의 인스턴스만 생성한다.
  - 메모리를 절약할 수 있다.
  - 객체의 상태를 공유하기 쉽다.
- 단점으로는 의존성이 높아 결합도가 높아진다.
  - 의존성이 높아지면, 유닛 테스트가 어려워진다.
  - 의존성이 높아지면, 유지보수가 어려워진다.
- 이런 단점들을 해결하기 위해 의존성 주입을 사용한다.
  - 의존성을 주입받기 때문에 결합도가 낮아진다.
  - 의존성을 주입받기 때문에 유닛 테스트가 쉬워진다.

## 마무리 글

TSyringe를 사용해보았고 의존성주입, 싱글톤에 대해 알아보았다.
TSyringe로 store를 구현해보면서 싱글톤 데코레이터를 사용해보았는데 아마 인스턴스를 생성할 때마다 새로운 인스턴스를 생성하지 않고, 기존에 생성된 인스턴스를 활용하기위해 싱글톤 데코레이터를 사용한 것 같다.  
그리고 싱글톤을 사용하면 결합도가 높아지는 문제가 있고 이를 해결하기 위해 의존성 주입으로 결합도를 낮추는 방법이 있다는걸 알았다.  
하지만 항상 만능은 없듯이 의존성 주입을 사용하면 그만큼 관리포인트가 늘어나고, 코드가 복잡해지는 문제도 알 수 있었다.  
의존성을 낮춰 테스트와 유지보수를 쉽게 만들고, 싱글톤을 사용하여 인스턴스가 중복으로 생성되는 것을 방지한다.  
결국 이런 방법들을 사용하는것은 코드의 유지보수를 쉽게 만들기 위함인데, 저번강의에서 배웠던 관심사의 분리가 생각이났고 모든것은 유지보수를 쉽게 만들기 위함이라는 생각이 들었다.  
하지만, 의존성을 낮추면서 코드가 복잡해지지는 않아야하고, 싱글톤을 사용하면서 결합도가 높아지지 않아야한다는 말인데 이 둘의 균형을 잘 맞추는것이 역량이고 어려운 부분이라고 생각한다.  

- TSyringe를 사용해보았고 의존성주입, 싱글톤에 대해 알아보았다.
- 싱글톤을 사용하면 하나의 인스턴스를 사용하여 메모리 절약, 객체 상태 공유가 쉬워지는 등의 장점이 있다.
- 의존성 주입을 사용하면 결합도가 낮아지고, 유닛 테스트가 쉬워지는 등의 장점이 있다.
- 싱글톤은 결합도가 높아지는 단점이 있고, 의존성주입은 코드가 복잡해지는 단점이 있다.
- 결국 모든것은 유지보수를 쉽게하기위해..?
