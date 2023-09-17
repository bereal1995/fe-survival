# 장바구니에 상품 담기

## UI 구현

### AddToCartForm

```tsx
export default function AddToCartForm() {
  return (
    <div>
      <Options />
      <Quantity />
      <Price />
      <SubmitButton />
    </div>
  );
}

// test
test('AddToCartForm', async () => {
  container.clearInstances();

  const [product] = fixtures.products;

  const productDetailStore = container.resolve(ProductDetailStore);

  await productDetailStore.fetchProduct({ productId: product.id });

  render(<AddToCartForm />);

  fireEvent.click(screen.getByText('장바구니에 담기'));

  await waitFor(() => {
    screen.getByText(/장바구니에 담았습니다/);
  });
});
```

- 장바구니에 상품을 담는 폼을 렌더링한다.

### Quantity

```tsx
const Container = styled.div`
  input {
    width: 5rem;
    text-align: center;
  }
`;

export default function Quantity() {
  const [{ quantity }, store] = useProductFormStore();

  const handleClickDecrease = () => {
    store.changeQuantity(quantity - 1);
  };

  const handleClickIncrease = () => {
    store.changeQuantity(quantity + 1);
  };

  return (
    <Container>
      <Button onClick={handleClickDecrease}>
        -
      </Button>
      <input type="text" value={quantity} readOnly />
      <Button onClick={handleClickIncrease}>
        +
      </Button>
    </Container>
  );
}

// test
const store = {
  quantity: 7,
  changeQuantity: jest.fn(),
};

jest.mock('../../../hooks/useProductFormStore', () => () => [store, store]);

const context = describe;

describe('Quantity', () => {
  beforeEach(() => {
    jest.clearAllMocks();
  });

  it('renders quantity', () => {
    render(<Quantity />);

    expect(screen.getByRole('textbox')).toHaveValue('7');
  });

  context('when "+" button is clicked', () => {
    it('calls changeQuantity', () => {
      render(<Quantity />);

      fireEvent.click(screen.getByRole('button', { name: '+' }));

      expect(store.changeQuantity).toBeCalledWith(7 + 1);
    });
  });
});
```

- 수량을 입력할 수 있는 컴포넌트를 렌더링한다.
- 수량을 변경할 수 있는 버튼을 렌더링한다.

### Button

```tsx
const Button = styled.button.attrs({
  type: 'button',
})`
  border: .1rem solid #888;
  background: transparent;
  color: ${(props) => props.theme.colors.primary};
  cursor: pointer;
`;

export default Button;
```

### useProductFormStore

```tsx
export default function useProductFormStore() {
  const store = container.resolve(ProductFormStore);
  return useStore(store);
}
```

### Price

```tsx
const Container = styled.div`
  margin-block: .2rem;
`;

export default function Price() {
  const [, productFormStore] = useProductFormStore();

  return (
    <Container>
      {numberFormat(productFormStore.price)}
      원
    </Container>
  );
}

// test
const [product] = fixtures.products;

describe('Price', () => {
  const quantity = 2;

  beforeEach(() => {
    const productFormStore = iocContainer.resolve(ProductFormStore);

    productFormStore.setProduct(product);
    productFormStore.changeQuantity(quantity);
  });

  it('renders price as formatted number', () => {
    const { container } = render(<Price />);

    const price = numberFormat(product.price * quantity);
    expect(container).toHaveTextContent(`${price}원`);
  });
});
```

- 상품 가격을 렌더링한다.

### SubmitButton

```tsx
export default function SubmitButton() {
  const [{ done }, store] = useProductFormStore();

  const handleClick = () => {
    store.addToCart();
  };

  if (done) {
    return (
      <p>장바구니에 담았습니다</p>
    );
  }

  return (
    <Button onClick={handleClick}>
      장바구니에 담기
    </Button>
  );
}

// test
let done = false;

const store = {
  get done() {
    return done;
  },
  addToCart: jest.fn(),
};

jest.mock('../../../hooks/useProductFormStore', () => () => [store, store]);

const context = describe;

describe('SubmitButton', () => {
  beforeEach(() => {
    jest.clearAllMocks();
  });

  context('when the button is ready', () => {
    beforeEach(() => {
      done = false;
    });

    it('renders a submit button', () => {
      render(<SubmitButton />);

      expect(screen.getByRole('button')).toHaveTextContent('장바구니에 담기');
    });

    context('when the button is clicked', () => {
      it('calls addToCart action', () => {
        render(<SubmitButton />);

        fireEvent.click(screen.getByRole('button'));

        expect(store.addToCart).toBeCalled();
      });
    });
  });

  context('when submitting is done', () => {
    beforeEach(() => {
      done = true;
    });

    it('renders a done message', () => {
      render(<SubmitButton />);

      screen.getByText(/장바구니에 담았습니다/);
    });
  });
});
```

- 장바구니에 담기 버튼을 렌더링한다.

### Options

```tsx
export default function Options() {
  const [{ product, selectedOptionItems }, store] = useProductFormStore();

  const handleChange: ChangeFunction = ({ optionId, optionItemId }) => {
    store.changeOptionItem({ optionId, optionItemId });
  };

  return (
    <div>
      {product.options.map((option, index) => (
        <Option
          key={option.id}
          option={option}
          selectedItem={selectedOptionItems[index]}
          onChange={handleChange}
        />
      ))}
    </div>
  );
}

// test
const [product] = fixtures.products;
const { options } = product;

const store = {
  product,
  selectedOptionItems: options.map((i) => i.items[0]),
  quantity: 1,
  changeOptionItem: jest.fn(),
};

jest.mock('../../../hooks/useProductFormStore', () => () => [store, store]);

const context = describe;

describe('Options', () => {
  beforeEach(() => {
    jest.clearAllMocks();
  });

  it('renders comboboxes', () => {
    render(<Options />);

    expect(screen.getAllByRole('combobox')).toHaveLength(options.length);
  });

  context('when selection is changed', () => {
    it('calls "changeOptionItem" action', () => {
      render(<Options />);

      const [option] = options;
      const [, item] = option.items;

      const [combobox] = screen.getAllByRole('combobox');

      fireEvent.change(combobox, {
        target: { value: item.id },
      });

      expect(store.changeOptionItem).toBeCalledWith({
        optionId: option.id,
        optionItemId: item.id,
      });
    });
  });
});
```

- 옵션 목록을 렌더링한다.

### Option

```tsx
type OptionProps = {
  option: ProductOption;
  selectedItem: ProductOptionItem;
  onChange: ChangeFunction;
}

export default function Option({
  option, selectedItem, onChange,
}: OptionProps) {
  const handleChange = (item: ProductOptionItem | null) => {
    if (!item) {
      return;
    }

    onChange({
      optionId: option.id,
      optionItemId: item.id,
    });
  };

  return (
    <ComboBox
      label={option.name}
      selectedItem={selectedItem}
      items={option.items}
      itemToId={(item) => item.id}
      itemToText={(item) => item.name}
      onChange={handleChange}
    />
  );
}

// test
const [product] = fixtures.products;
const { options } = product;

const context = describe;

describe('Option', () => {
  const option = options[0];
  const onChange = jest.fn();

  const renderOption = () => render((
    <Option
      option={option}
      selectedItem={option.items[0]}
      onChange={onChange}
    />
  ));

  it('renders option', () => {
    renderOption();

    screen.getByText(option.name);
  });

  context('when option is selected', () => {
    it('calls onChange', () => {
      render((
        <Option
          option={option}
          selectedItem={option.items[0]}
          onChange={onChange}
        />
      ));

      fireEvent.change(screen.getByRole('combobox'), {
        target: { value: option.items[0].id },
      });

      expect(onChange).toBeCalledWith({
        optionId: option.id,
        optionItemId: option.items[0].id,
      });
    });
  });
});
```

### ComboBox

```tsx
const Container = styled.div`
  label {
    margin-right: .5rem;
  }
`;

type ComboBoxProps<T> = {
  label: string;
  selectedItem: T;
  items: T[];
  itemToId: (item: T) => string;
  itemToText: (item: T) => string;
  onChange: (item: T | null) => void;
}

export default function ComboBox<T>({
  label, selectedItem, items, itemToId, itemToText, onChange,
}: ComboBoxProps<T>) {
  const id = useRef(`combobox-${Math.random().toString().slice(2)}`);

  const handleChange = (event: React.ChangeEvent<HTMLSelectElement>) => {
    const { value } = event.target;
    const selected = items.find((item) => itemToId(item) === value);
    onChange(selected ?? null);
  };

  return (
    <Container>
      <label htmlFor={id.current}>
        {label}
      </label>
      <select
        id={id.current}
        onChange={handleChange}
        value={itemToId(selectedItem)}
      >
        {items.map((item) => (
          <option key={itemToId(item)} value={itemToId(item)}>
            {itemToText(item)}
          </option>
        ))}
      </select>
    </Container>
  );
}
```

## 상태관리

### ProductFormStore

```ts
@singleton()
@Store()
export default class ProductFormStore {
  product: ProductDetail = nullProductDetail;

  selectedOptionItems: ProductOptionItem[] = [];

  quantity = 1;

  done = false;

  async addToCart() {
    this.resetDone();

    await apiService.addProductToCart({
      productId: this.product.id,
      options: this.product.options.map((option, index) => ({
        id: option.id,
        itemId: this.selectedOptionItems[index].id,
      })),
      quantity: this.quantity,
    });

    this.complete();
  }

  @Action()
  setProduct(product: ProductDetail) {
    this.product = product;
    this.selectedOptionItems = this.product.options.map((i) => i.items[0]);
    this.quantity = 1;
    this.done = false;
  }

  @Action()
  changeQuantity(quantity: number) {
    if (quantity <= 0) {
      return;
    }
    if (quantity > 10) {
      return;
    }
    this.quantity = quantity;
  }

  @Action()
  changeOptionItem({ optionId, optionItemId }: {
  optionId: string;
  optionItemId: string;
}) {
    this.selectedOptionItems = this.product.options.map((option, index) => {
      const item = this.selectedOptionItems[index];
      return option.id !== optionId
        ? item
        : option.items.find((i) => i.id === optionItemId) ?? item;
    });
  }

  @Action()
  resetDone() {
    this.done = false;
  }

  @Action()
  complete() {
    this.quantity = 1;
    this.done = true;
  }

  get price() {
    return this.product.price * this.quantity;
  }
}
```

### ProductFormStore.test

```ts
import fixtures from '../../fixtures';
import ProductFormStore from './ProductFormStore';

const addProductToCart = jest.fn();

jest.mock('../services/ApiService', () => ({
  get apiService() {
    return {
      addProductToCart,
    };
  },
}));

const context = describe;

describe('ProductFormStore', () => {
  let store: ProductFormStore;

  beforeEach(() => {
    jest.clearAllMocks();

    store = new ProductFormStore();
  });

  describe('setProduct', () => {
    const [product] = fixtures.products;

    it('sets state', () => {
      store.setProduct(product);

      expect(store.product).toBe(product);
      expect(store.selectedOptionItems).toHaveLength(product.options.length);
      expect(store.quantity).toBe(1);
    });
  });

  describe('changeOptionItem', () => {
    const [product] = fixtures.products;
    const [, option] = product.options;

    beforeEach(() => {
      store.setProduct(product);
    });

    context('with correct IDs', () => {
      it('changes option item', () => {
        const prevItems = store.selectedOptionItems;

        store.changeOptionItem({
          optionId: option.id,
          optionItemId: option.items[1].id,
        });

        expect(store.selectedOptionItems).toEqual([
          prevItems[0],
          {
            id: option.items[1].id,
            name: option.items[1].name,
          },
        ]);
      });
    });

    context('with incorrect IDs', () => {
      it("doesn't changes option item", () => {
        const prevItems = store.selectedOptionItems;

        store.changeOptionItem({
          optionId: option.id,
          optionItemId: 'xxx',
        });

        expect(store.selectedOptionItems).toEqual(prevItems);
      });
    });
  });

  describe('changeQuantity', () => {
    context('with correct value', () => {
      it('changes quantity', () => {
        store.changeQuantity(3);

        expect(store.quantity).toBe(3);
      });
    });

    context('with incorrect value', () => {
      it("doesn't changes quantity", () => {
        store.changeQuantity(-1);
        store.changeQuantity(11);

        expect(store.quantity).toBe(1);
      });
    });
  });

  describe('addToCart', () => {
    const [product] = fixtures.products;

    beforeEach(() => {
      store.setProduct(product);
    });

    it('calls "addProductRoCart" request', () => {
      store.addToCart();

      expect(addProductToCart).toBeCalled();
    });
  });
});
```

### addProductToCart

```ts
async addProductToCart({ productId, options, quantity }: {
  productId: string;
  options: {
    id: string;
    itemId: string;
  }[];
  quantity: number;
}): Promise<void> {
  await this.instance.post('/cart/line-items', {
    productId, options, quantity,
  });
}
```

## E2E 테스트

코드 작성이 마무리하면 잘 동작하는지 확인해야 하기 때문에 E2E 테스틀 활용한다.

### 테스트 설정

backdoor를 사용하기 위해

```ts
// codecept.conf.ts
export const config = {
  // ...
  include: {
    I: './tests/steps_file',
    backdoor: './tests/backdoor.ts',
  },
  // ...
};


// steps.d.ts
// ...
type backdoor = typeof import('./backdoor');

declare namespace CodeceptJS {
  interface SupportObject {
    I: I;
    backdoor: backdoor;
  }

  // ...
}
```

### cart_test.ts

```ts
Feature('Cart');

Before(({ backdoor }) => {
  backdoor.setupDatabase();
});

Scenario('Empty cart', ({ I }) => {
  I.amOnPage('/cart');

  I.see('장바구니가 비었습니다');
});

Scenario('Add to cart', ({ I }) => {
  I.amOnPage('/products');

  I.click('CBCL 하트자수맨투맨');

  I.click('컬러');
  I.selectOption('컬러', 'blue');

  I.seeElement('//button[contains(., "+")]');

  I.click('장바구니에 담기');

  I.waitForText('장바구니에 담았습니다');

  I.amOnPage('/cart');

  I.see('CBCL 하트자수맨투맨\n(컬러: blue');
  I.see('합계\t128,000원');
});
```
