---
description: usestore-ts에 대해 알아보자
---

# usestore-ts

## [usestore-ts?](https://github.com/seed2whale/usestore-ts)

상태관리를 위한 라이브러리  
여러개의 스토어를 사용할 수 있고 데코레이터를 사용하여 간편하게 store를 구성할 수 있다.

### 예시

스토어 구성

```typescript
import { singleton } from 'tsyringe';

import { Store, Action } from 'usestore-ts';

@singleton()
@Store()
export default class CounterStore {
  count = 0;

  @Action()
  increase(step = 1) {
    this.count += step;
  }

  @Action()
  decrease() {
    this.count -= 1;
  }
}
```

비동기 사용시

```typescript
@singleton()
@Store()
class PostStore {
  posts: Post[] = [];

  async fetchPosts() {
    this.startLoading();

    const posts = await fetchPosts();

    this.completeLoading(posts);
  }

  @Action()
  startLoading() {
    this.posts = [];
  }

  @Action()
  completeLoading(posts: Post[]) {
    this.posts = posts;
  }
}
```

- 비동기 함수에서는 `startLoading`과 `completeLoading`을 사용하여 로딩 상태를 관리한다.
- 만약 사용하지 않으면 Promise를 바로 반환해서 예상하지 못한 결과를 얻을 수 있다.

## [useSyncExternalStore](https://react.dev/reference/react/useSyncExternalStore)

외부 스토어를 동기화 시키는 훅

- 외부 스토어의 변경을 감지하여 리렌더링을 한다.
- 외부 스토어의 변경을 감지하여 내부 스토어를 변경한다.
- 2개의 매개변수를 필수로 받는다.
  - subscribe: 스토어를 구독하고 구독을 취소하는 함수
  - getSnapshot: 스토어의 상태를 반환(데이터의 스냅샷)하는 함수
- 1개의 매개변수를 선택적으로 받는다.
  - getServerSnapshot: store에 있는 데이터의 초기 스냅샷을 반환하는 함수

## 마무리 글

usestore-ts에 대해 알아보았다.  
상태관리를 위한 라이브러리인데 데코레이터를 사용하여 간편하게 store를 구성할 수 있다.  
아직 강의를 보면서 따라해본 정도이지만, 간단하게 스토어를 구성할 수 있어서 좋았다.  
아쉬운점으로는 리액트에서 보통 store에 접근할때 커스텀 훅을 사용해서 접근하는데, 스토어에 접근할때 tsyringe의 `container.resolve`를 사용해야했는데 useStore로 이부분을 추상화 시켜서 사용할 수 있었으면 좋았을 것 같다.  
<br>
usestore-ts도 사용목적은 관심사의 분리를 위한 것이기 때문에, 사용할때 항상 이부분을 염두해두고 사용해야겠다.
