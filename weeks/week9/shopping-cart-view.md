# 장바구니 보기

## 데이터 연동 및 상태 관리

### CartPage

```tsx

export default function CartPage() {
  const { cart } = useFetchCart();

  if (!cart) {
    return null;
  }

  return (
    <div>
      <h2>장바구니</h2>
      <CartView cart={cart} />
    </div>
  );
}
```

- 장바구니 페이지를 만들고 useFetchCart 훅을 사용하여 장바구니 정보를 얻는다.
- 예제에서는 단순하게 처리하기 위해 useFetchCart 훅에서 cart만 반환한다.

### useFetchCart

```tsx
export default function useFetchCart() {
  const store = container.resolve(CartStore);

  const [{ cart }] = useStore(store);

  useEffectOnce(() => {
    store.fetchCart();
  });

  return { cart };
}
```

- useFetchCart 훅을 만들어서 어떤 컴포넌트에서도 CartStore를 사용할 수 있도록 한다.
- useEffectOnce 훅을 사용하여 컴포넌트가 처음 렌더링될 때만 장바구니 정보를 가져온다.

### CartStore

```tsx
@singleton()
@Store()
export default class CartStore {
  cart: Cart | null = null;

  async fetchCart() {
    this.setCart(null);

    const cart = await apiService.fetchCart();

    this.setCart(cart);
  }

  @Action()
  setCart(cart: Cart | null) {
    this.cart = cart;
  }
}

// fetchCart
async fetchCart(): Promise<Cart> {
  const { data } = await this.instance.get('/cart');
  return data;
}
```

- CartStore를 만들고 fetchCart 메서드를 만들어서 장바구니 정보를 가져온다.
- 장바구니 정보를 가져오기 전에는 null을 설정하고, 장바구니 정보를 가져오면 설정한다.
  - null 보다는 nullObject 만들어서 사용하는것이 더 좋다!

## UI 구현

### CartView

```tsx
const Container = styled.div`
  table {
    width: 100%;
  }

  th, td {
    padding: .5rem;
    text-align: left;
  }
`;

type CartViewProps = {
  cart: Cart;
};

export default function CartView({ cart }: CartViewProps) {
  if (!cart.lineItems.length) {
    return (
      <p>장바구니가 비었습니다</p>
    );
  }

  return (
    <Container>
      <table>
        <thead>
          <tr>
            <th>제품</th>
            <th>단가</th>
            <th>수량</th>
            <th>금액</th>
          </tr>
        </thead>
        <tbody>
          {cart.lineItems.map((lineItem) => (
            <LineItemView
              key={lineItem.id}
              lineItem={lineItem}
            />
          ))}
        </tbody>
        <tfoot>
          <tr>
            <th colSpan={3}>
              합계
            </th>
            <td>
              {numberFormat(cart.totalPrice)}
              원
            </td>
          </tr>
        </tfoot>
      </table>
    </Container>
  );
}

// test
describe('CartView', () => {
  context('when the cart is empty', () => {
    it('renders empty message', () => {
      render(<CartView cart={{ lineItems: [], totalPrice: 0 }} />);

      screen.getByText('장바구니가 비었습니다');
    });
  });

  context('when the cart has a line item', () => {
    const { cart } = fixtures;

    it('renders a line item', () => {
      render(<CartView cart={cart} />);

      screen.getByText(cart.lineItems[0].product.name);
    });
  });
});
```

- 장바구니가 비었을 때는 비었다는 메시지를 렌더링한다.
- 장바구니에 상품이 있을 때는 상품 목록을 렌더링한다.

### LineItemView

```tsx
type LineItemViewProps = {
  lineItem: LineItem;
};

export default function LineItemView({ lineItem }: LineItemViewProps) {
  return (
    <tr>
      <td>
        <Link to={`/products/${lineItem.product.id}`}>
          {lineItem.product.name}
        </Link>
        <Options options={lineItem.options} />
      </td>
      <td>
        {numberFormat(lineItem.unitPrice)}
        원
      </td>
      <td>{lineItem.quantity}</td>
      <td>
        {numberFormat(lineItem.totalPrice)}
        원
      </td>
    </tr>
  );
}

// test
describe('LineItemView', () => {
  it('renders a line item', () => {
    const [lineItem] = fixtures.cart.lineItems;

    render((
      <table>
        <tbody>
          <LineItemView lineItem={lineItem} />
        </tbody>
      </table>
    ));

    screen.getByText(lineItem.product.name);
  });
});
```

- 장바구니에 담긴 상품 목록을 렌더링한다.

### Options

```tsx
const Container = styled.div`
  margin-top: .5rem;
  font-size: 1.4rem;
`;

type OptionsProps = {
  options: OrderOption[];
}

export default function Options({ options }: OptionsProps) {
  if (!options.length) {
    return null;
  }

  const text = options
    .map((option) => `${option.name}: ${option.item.name}`)
    .join(', ');

  return (
    <Container>
      (
      {text}
      )
    </Container>
  );
}

// test
describe('Options', () => {
  context('when options is empty', () => {
    const options: OrderOption[] = [];
    it('renders nothing', () => {
      const { container } = render(<Options options={options} />);

      expect(container).toBeEmptyDOMElement();
    });
  });

  context('when options is not empty', () => {
    const [lineItem] = fixtures.cart.lineItems;
    const { options } = lineItem;

    it('renders options text', () => {
      const { container } = render(<Options options={options} />);

      const optionName = options[0].name;
      const itemName = options[0].item.name;

      expect(container).toHaveTextContent(`${optionName}: ${itemName}`);
    });
  });
});
```

- 장바구니에 담긴 상품의 옵션을 렌더링한다.
- 옵션이 없으면 렌더링하지 않는다.