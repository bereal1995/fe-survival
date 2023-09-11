---
description: props와 attrs에 대해 알아보자
---

# props와 attrs

## [props](https://styled-components.com/docs/basics#passed-props)

styled-components는 props를 통해 스타일을 변경할 수 있다.

```tsx
import styled, {css} from 'styled-components';
import {useBoolean} from 'usehooks-ts';

type ButtonProps = {
  active: boolean;
};

const Button = styled.button<ButtonProps>`
  background: #FFF;
  color: #000;
  border: 1px solid ${props => props.active ? '#F00' : '#888'};
  cursor: pointer;

  ${props => props.active && css`
    background: #00F;
    color: #FFF;
  `}
`;

export default function Switch() {
  const {value: active, toggle} = useBoolean(false);

  return (
    <Button
      type='submit'
      onClick={toggle}
      active={active}
    >
      On/Off
    </Button>
  );
}
```

- 버튼을 클릭할면 active 상태가 변경되고, styled-components는 active 상태에 따라 스타일을 변경한다.
- props안에 있는 값을 사용할 때, `props => props.active`와 같이 사용할 수 있다.
- props를 사용하여 스타일을 변경할 때, `css` 함수를 사용하면 가독성이 좋게 표현할 수 있다.
  - `css` 함수는 스타일을 표현하는 함수이다.

## [attrs](https://styled-components.com/docs/basics#attaching-additional-props)

styled-components는 `attrs` 함수를 통해 속성을 추가할 수 있다.

```tsx
import styled, {css} from 'styled-components';
import {useBoolean} from 'usehooks-ts';

type ButtonProps = {
  type?: 'button' | 'submit' | 'reset';
  active: boolean;
};

const Button = styled.button.attrs<ButtonProps>(props => ({
  type: props.type ?? 'button',
}))<ButtonProps>`
  background: #FFF;
  color: #000;
  border: 1px solid ${props => props.active ? '#F00' : '#888'};
  cursor: pointer;

  ${props => props.active && css`
    background: #00F;
    color: #FFF;
  `}
`;

const PrimaryButton = styled(Button)`
  background: #EEE;

  ${props => props.active && css`
    background: #FCF;
  `}
`;

export default function Switch() {
  const {value: active, toggle} = useBoolean(false);

  return (
    <PrimaryButton
      type='submit'
      onClick={toggle}
      active={active}
    >
      On/Off
    </PrimaryButton>
  );
}
```

- 이전 props 예제에서 `type` 속성을 추가하였다.
- `attrs` 함수는 속성을 추가할 때 사용한다.
  - `attrs` 함수는 `attrs<T>` 형태로 사용한다.
  - `T`는 속성의 타입을 의미한다.
- `attrs` 함수는 `props => ({type: props.type ?? 'button'})`와 같이 사용할 수 있다.
  - `props`는 `T` 타입으로 추론된다.
