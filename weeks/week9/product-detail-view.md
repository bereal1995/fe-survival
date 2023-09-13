# 상품 상세

## ProductDetailPage

```tsx
export default function ProductDetailPage() {
  const params = useParams();

  const { loading, error } = useFetchProduct({
    productId: String(params.id),
  });

  if (loading) {
    return (
      <p>Loading...</p>
    );
  }

  if (error) {
    return (
      <p>Error!</p>
    );
  }

  return (
    <ProductDetail />
  );
}
```

- 상품 상세 페이지를 만들고 useFetchProduct 훅을 사용하여 상품 상세 정보를 얻는다.
- 예제에서는 단순하게 처리하기 위해 useFetchProduct 훅에서 loading과 error만 반환한다.

## ProductDetail

```tsx
const Container = styled.div`
  display: flex;
  justify-content: space-between;

  aside {
    width: 38%;
  }

  article {
    width: 60%;
  }
`;

export default function ProductDetail() {
  const [{ product }] = useProductDetailStore();

  return (
    <Container>
      <aside>
        <Images images={product.images} />
      </aside>
      <article>
        <h2>{product.name}</h2>
        <Description value={product.description} />
      </article>
    </Container>
  );
}

// test
const [product] = fixtures.products;

test('ProductDetail', async () => {
  const store = container.resolve(ProductDetailStore);

  await store.fetchProduct({ productId: product.id });

  render(<ProductDetail />);

  screen.getByText(product.name);
});
```

- prop drilling을 피하기 위해 상위에서 product를 내려주지 않고 useProductDetailStore 훅을 사용하여 상품 상세 정보를 얻는다.
- Images와 Description 컴포넌트에 product 정보를 전달한다.
- 테스트코드에서는 msw를 사용하여 api를 호출하지 않고 store에 저장된 상품 상세 정보를 사용한다.

## useProductDetailStore

```tsx
export default function useProductDetailStore() {
  const store = container.resolve(ProductDetailStore);
  return useStore(store);
}
```

- useProductDetailStore 훅을 만들어서 어떤 컴포넌트에서도 ProductDetailStore를 사용할 수 있도록 한다.

## ProductDetailStore

```tsx
@singleton()
@Store()
export default class ProductDetailStore {
  product: ProductDetail = nullProductDetail;

  loading = true;

  error = false;

  async fetchProduct({ productId }: {
    productId: string;
  }) {
    this.startLoading();

    try {
      const product = await apiService.fetchProduct({ productId });
      this.setProduct(product);
    } catch {
      this.setError();
    }
  }

  @Action()
  private startLoading() {
    this.product = nullProductDetail;
    this.loading = true;
    this.error = false;
  }

  @Action()
  private setProduct(product: ProductDetail) {
    this.product = product;
    this.loading = false;
    this.error = false;
  }

  @Action()
  private setError() {
    this.product = nullProductDetail;
    this.loading = false;
    this.error = true;
  }
}
```

- 상품 상세 정보를 얻어서 store에 저장한다.
- 기존에 있던 products store와 비슷하다.
- nullProductDetail를 사용하여 초기화한다.

## [nullProductDetail](https://refactoring.com/catalog/introduceSpecialCase.html)

```tsx
export const nullProductDetail: ProductDetail = {
  id: '',
  category: { id: '', name: '' },
  images: [],
  name: '',
  price: 0,
  options: [],
  description: '',
};
```

- 타입을 잡을때 null을 사용하면 계속해서 null 체크를 해야하는데 이를 피하기 위해 null 대신에 특수한 값을 사용한다.

## useFetchProduct

```tsx
export default function useFetchProduct({ productId }: {
  productId: string
}) {
  const [{ loading, error }, store] = useProductDetailStore();

  useEffect(() => {
    store.fetchProduct({ productId });
  }, [store, productId]);

  return { loading, error };
}
```

- useFetchProduct 훅을 만들어서 상품 상세 정보를 얻는 로직을 분리한다.
- 기존에 만들었던 useProductDetailStore 훅을 사용하여 store를 얻는다.

## Images

```tsx
const Thumbnail = styled.img.attrs({
  alt: 'Product Image',
})`
  display: block;
  width: 100%;
  aspect-ratio: 1/1;
`;

type ImagesProps = {
  images: Image[];
}

export default function Images({ images }: ImagesProps) {
  const [image] = images;

  if (!image) {
    return null;
  }

  return (
    <Thumbnail src={image.url} />
  );
}

// test
const context = describe;

describe('Images', () => {
  context('when images is Empty', () => {
    const images: Image[] = [];

    it('renders nothing', () => {
      const { container } = render(<Images images={images} />);

      expect(container).toBeEmptyDOMElement();
    });
  });

  context('when images is not Empty', () => {
    const images: Image[] = [{ url: 'https://example.com/test.jpg' }];

    it('renders image', () => {
      render(<Images images={images} />);

      screen.getByRole('img');
    });
  });
});
```

- 상품 이미지가 없으면 렌더링하지 않는다.
- 상품 이미지가 있으면 첫번째 이미지를 렌더링한다.
- 테스트코드에서는
  - 상품 이미지가 없을때 렌더링하지 않는지 확인한다.
  - 상품 이미지가 있을때 첫번째 이미지를 렌더링하는지 확인한다.

## Description

```tsx
function key(value: string, index: number) {
  return `${index}-${value}`;
}

const Container = styled.div`
  li {
    min-height: 1rem;
    line-height: 1.4;
  }
`;

type DescriptionProps = {
  value: string;
}

export default function Description({ value }: DescriptionProps) {
  if (!value.trim()) {
    return null;
  }

  const lines = value.split('\n');

  return (
    <Container>
      <ul>
        {lines.map((line, index) => (
          <li key={key(line, index)}>
            {line}
          </li>
        ))}
      </ul>
    </Container>
  );
}

// test
describe('Description', () => {
  context('when text is empty', () => {
    const text = '';
    it('renders nothing', () => {
      const { container } = render(<Description value={text} />);

      expect(container).toBeEmptyDOMElement();
    });
  });

  context('when text is not empty', () => {
    const lines = ['1st line', '2nd line', '3rd line'];
    const text = lines.join('\n');

    it('renders multi-line text', () => {
      render(<Description value={text} />);

      lines.forEach((line) => {
        screen.getByText(line);
      });
    });
  });
});
```

- 상품 설명이 없으면 렌더링하지 않는다.
- 상품 설명이 있으면 줄바꿈을 기준으로 나누어서 렌더링한다.
- key라는 함수를 만들어서 린트에 걸리지 않고, index를 react element의 key로 사용한다.
- 테스트코드에서는
  - 상품 설명이 없을때 렌더링하지 않는지 확인한다.
  - 상품 설명이 있을때 줄바꿈을 기준으로 나누어서 렌더링하는지 확인한다.
