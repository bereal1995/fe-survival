---
description: usehooks-ts에 대해 알아보자
---

# useHooks-ts

## usehooks-ts

typescript로 작성된 custom hooks를 모아놓은 라이브러리이다.  
여러가지 custom hooks를 잘 만들어놓은 라이브러리이다.  
이 라이브러리를 사용하면서 custom hooks를 어떻게 만들어야 하는지 공부해보자.  
docs: [https://usehooks-ts.com/](https://usehooks-ts.com/)  

### useBoolean

- boolean 값을 관리할 수 있는 custom hook이다.
- `true`와 `false`를 관리할 수 있다.

```tsx
import { useBoolean } from 'usehooks-ts'

export default function Component() {
  const { value, setValue, setTrue, setFalse, toggle } = useBoolean(false)

  // Just an example to use "setValue"
  const customToggle = () => setValue((x: boolean) => !x)

  return (
    <>
      <p>
        Value is <code>{value.toString()}</code>
      </p>
      <button onClick={setTrue}>set true</button>
      <button onClick={setFalse}>set false</button>
      <button onClick={toggle}>toggle</button>
      <button onClick={customToggle}>custom toggle</button>
    </>
  )
}
```

### useEffectOnce

- `useEffect`를 한 번만 실행할 수 있는 custom hook이다.
- `useEffect`를 한 번만 실행하고 싶을 때 사용할 수 있다.

```tsx
import { useEffectOnce } from 'usehooks-ts'

export default function Component() {
  useEffectOnce(() => {
    console.log('useEffectOnce')
  })

  return <p>Component</p>
}

// 구현
import { EffectCallback, useEffect } from 'react'

export function useEffectOnce(effect: EffectCallback) {
  // eslint-disable-next-line react-hooks/exhaustive-deps
  useEffect(effect, [])
}
```

### useFetch

- `fetch`를 사용할 수 있는 custom hook이다.
- `fetch`를 사용할 때 `loading`, `error`, `data`를 관리할 수 있다.
- 기본 사용 사례 및 학습 목적을 위해 만들어졌기 때문에, 프로젝트에서 사용할 목적이라면 다른 라이브러리나 훅을 사용하는 것이 좋다.

```tsx
import { useFetch } from './useFetch'

const url = `http://jsonplaceholder.typicode.com/posts`

interface Post {
  userId: number
  id: number
  title: string
  body: string
}

export default function Component() {
  const { data, error } = useFetch<Post[]>(url)

  if (error) return <p>There is an error.</p>
  if (!data) return <p>Loading...</p>
  return <p>{data[0].title}</p>
}
```

### useInterval

- `setInterval`을 사용할 수 있는 custom hook이다.
- `setInterval`을 사용할 때 `delay`를 동적으로 변경할 수 있다.
- 참고내용: [https://overreacted.io/making-setinterval-declarative-with-react-hooks/](https://overreacted.io/making-setinterval-declarative-with-react-hooks/)

```tsx
import { ChangeEvent, useState } from 'react'

import { useInterval } from 'usehooks-ts'

export default function Component() {
  // The counter
  const [count, setCount] = useState<number>(0)
  // Dynamic delay
  const [delay, setDelay] = useState<number>(1000)
  // ON/OFF
  const [isPlaying, setPlaying] = useState<boolean>(false)

  useInterval(
    () => {
      // Your custom logic here
      setCount(count + 1)
    },
    // Delay in milliseconds or null to stop it
    isPlaying ? delay : null,
  )

  const handleChange = (event: ChangeEvent<HTMLInputElement>) => {
    setDelay(Number(event.target.value))
  }

  return (
    <>
      <h1>{count}</h1>
      <button onClick={() => setPlaying(!isPlaying)}>
        {isPlaying ? 'pause' : 'play'}
      </button>
      <p>
        <label htmlFor="delay">Delay: </label>
        <input
          type="number"
          name="delay"
          onChange={handleChange}
          value={delay}
        />
      </p>
    </>
  )
}
```

### useEventListener** (강력추천)

- `addEventListener`를 사용할 수 있는 custom hook이다.
- `addEventListener`를 사용할 때 `target`, `type`, `listener`, `options`를 설정할 수 있다.
- dispatchEvent로 전달되는 커스텀 이벤트에 반응할 수 있다.

```tsx
import { useRef } from 'react'

import { useEventListener } from 'usehooks-ts'

export default function Component() {
  // Define button ref
  const buttonRef = useRef<HTMLButtonElement>(null)
  const documentRef = useRef<Document>(document)

  const onScroll = (event: Event) => {
    console.log('window scrolled!', event)
  }

  const onClick = (event: Event) => {
    console.log('button clicked!', event)
  }

  const onVisibilityChange = (event: Event) => {
    console.log('doc visibility changed!', {
      isVisible: !document.hidden,
      event,
    })
  }

  // example with window based event
  useEventListener('scroll', onScroll)

  // example with document based event
  useEventListener('visibilitychange', onVisibilityChange, documentRef)

  // example with element based event
  useEventListener('click', onClick, buttonRef)

  return (
    <div style={{ minHeight: '200vh' }}>
      <button ref={buttonRef}>Click me</button>
    </div>
  )
}
```

### useLocalStorage

- `localStorage`를 사용할 수 있는 custom hook이다.
- `localStorage`를 사용할 때 `key`, `initialValue`를 설정할 수 있다.
- `localStorage`를 사용할 때 `value`를 관리할 수 있다.
- 훅을 사용하는 곳이 많더라도 localStorage에 저장된 값을 통해 동기화된다.
- JSON.stringify와 JSON.parse를 자동으로 처리해준다.

```tsx
import { useLocalStorage } from 'usehooks-ts'

// Usage
export default function Component() {
  const [isDarkTheme, setDarkTheme] = useLocalStorage('darkTheme', true)

  const toggleTheme = () => {
    setDarkTheme((prevValue: boolean) => !prevValue)
  }

  return (
    <button onClick={toggleTheme}>
      {`The current theme is ${isDarkTheme ? `dark` : `light`}`}
    </button>
  )
}

```

### useDarkMode

- 내부적으로 useLocalStorage를 사용하여 구현되어 있다.
- 추가적으로 OS의 다크모드를 감지하여 사용할 수 있다.

```tsx
import { useDarkMode } from 'usehooks-ts'

export default function Component() {
  const { isDarkMode, toggle, enable, disable } = useDarkMode()

  return (
    <div>
      <p>Current theme: {isDarkMode ? 'dark' : 'light'}</p>
      <button onClick={toggle}>Toggle</button>
      <button onClick={enable}>Enable</button>
      <button onClick={disable}>Disable</button>
    </div>
  )
}
```

## [swr](https://swr.vercel.app/ko)

"SWR"이라는 이름은 HTTP RFC 5861(opens in a new tab)에 의해 알려진 HTTP 캐시 무효 전략인 stale-while-revalidate에서 유래되었습니다. SWR은 먼저 캐시(stale)로부터 데이터를 반환한 후, fetch 요청(revalidate)을 하고, 최종적으로 최신화된 데이터를 가져오는 전략입니다.

- `React`에서 `data fetching`을 쉽게 할 수 있도록 해주는 `React Hooks`이다.
- `Stale-While-Revalidate`의 약자이다.
- 대표적으로 `useSWR`이라는 훅을 사용하여 데이터를 가져온다.
- cache, stale-time 등 최적화된 기능들도 제공한다.
- react-query와 비슷한 기능을 제공한다.
- vercel에서 만든 라이브러리이다.

## [react-query](https://tanstack.com/query/v3/docs/react/overview)

세분화된 상태 관리, 수동 리페칭, 비동기 스파게티 코드의 끝없는 반복은 이제 그만하세요. TanStack Query는 항상 최신 상태로 자동 관리되는 선언적 쿼리와 변형을 제공하여 개발자와 사용자 경험을 직접적으로 개선합니다.

- `React`에서 `data fetching`을 쉽게 할 수 있도록 해주는 `React Hooks`이다.
- 대표적으로 `useQuery`라는 훅을 사용하여 데이터를 가져온다.
- cache, stale-time 등 최적화된 기능들도 제공한다.
- swr와 비슷한 기능을 제공한다.
- TanStack에서 만든 라이브러리이다.