# 목록 보기

## 상품 목록

### ProductListPage

```tsx
import Products from '../components/product-list/Products';

import useFetchProducts from '../hooks/useFetchProducts';

export default function ProductListPage() {
  const { products } = useFetchProducts();

  return (
    <div>
      <h2>Products</h2>
      <Products products={products} />
    </div>
  );
}
```

- 상품 목록 페이지를 만들고 useFetchProducts 훅을 사용하여 상품 목록을 얻는다.
- 상품 목록을 받아서 Products 컴포넌트에 전달한다.

### useFetchProducts

```tsx
export default function useFetchProducts() {
  type Data = {
    products: ProductSummary[];
  };

  const { data } = useFetch<Data>(`${apiBaseUrl}/products`);

  return {
    products: data?.products ?? [],
  };
}
```

- useFetchProducts 훅은 상품 목록을 얻어서 반환한다.
- useFetch 훅을 사용하여 상품 목록을 얻는다.

### Products

```tsx
const Container = styled.div`
  ul {
    display: flex;
    flex-wrap: wrap;
  }

  li {
    width: 20%;
    padding: 1rem;
  }

  a {
    display: block;
    text-decoration: none;
  }
`;

type ProductsProps = {
  products: ProductSummary[];
}

export default function Products({ products }: ProductsProps) {
  if (!products.length) {
    return null;
  }

  return (
    <Container>
      <ul>
        {products.map((product) => (
          <li key={product.id}>
            <Link to={`/products/${product.id}`}>
              <Product product={product} />
            </Link>
          </li>
        ))}
      </ul>
    </Container>
  );
}
```

- Products 컴포넌트는 상품 목록을 받아서 렌더링한다.
- 상품 목록이 없으면 렌더링하지 않는다.

### Product

```tsx
const Thumbnail = styled.img.attrs({
  alt: 'Thumbnail',
})`
  display: block;
  width: 100%;
  aspect-ratio: 1/1;
`;

type ProductProps = {
  product: ProductSummary;
}

export default function Product({ product }: ProductProps) {
  return (
    <div>
      <Thumbnail src={product.thumbnail.url} />
      <div>{product.name}</div>
      <div>
        {numberFormat(product.price)}
        원
      </div>
    </div>
  );
}
```

- Product 컴포넌트는 상품 정보를 받아서 렌더링한다.

### numberFormat

```tsx
// src/utils/numberFormat.ts
export default function numberFormat(value: number) {
  return new Intl.NumberFormat().format(value);
}

// src/utils/numberFormat.test.ts
import numberFormat from './numberFormat';

test('numberFormat', () => {
  expect(numberFormat(1)).toBe('1');
  expect(numberFormat(100)).toBe('100');
  expect(numberFormat(1_000)).toBe('1,000');
  expect(numberFormat(123_000)).toBe('123,000');
  expect(numberFormat(1_234_000)).toBe('1,234,000');
});
```

- `numberFormat` 함수는 숫자를 받아서 문자열로 변환한다.
- `Intl.NumberFormat`을 사용하여 숫자를 문자열로 변환한다.
  - `Intl.NumberFormat`은 브라우저에서 제공하는 API이다.
- 잊지말고 간단한 테스트를 작성하여 테스트를 통과하도록 구현한다.

기존에 useFetch를 사용하던 부분을 store로 관리하도록 변경한다.

### ProductsStore

```tsx
@singleton()
@Store()
export default class ProductsStore {
  products: ProductSummary[] = [];

  async fetchProducts() {
    this.setProducts([]);

    const { data } = await axios.get(`${apiBaseUrl}/products`);
    const { products } = data;

    this.setProducts(products);
  }

  @Action()
  setProducts(products: ProductSummary[]) {
    this.products = products;
  }
}

// src/hooks/useFetchProducts.ts
export default function useFetchProducts() {
  const store = container.resolve(ProductsStore);
  const [{ products }] = useStore(store);

  useEffectOnce(() => {
    store.fetchProducts();
  });

  return { products };
}
```

- ProductsStore를 생성하고 fetchProducts 메서드를 구현한다.
- fetchProducts 메서드는 상품 목록을 얻어서 store에 저장한다.
- useFetchProducts 훅은 store에서 상품 목록을 얻어서 반환한다.

## 카테고리 목록

### 헤더

```tsx
const Container = styled.header`
  margin-bottom: 2rem;

  h1 {
    font-size: 4rem;
  }

  nav {
    padding-block: 2rem;

    ul {
      display: flex;
    }

    li {
      margin-right: 2rem;
    }

    .active {
      color: ${(props) => props.theme.colors.primary};
    }
  }
`;

export default function Header() {
  const { categories } = useFetchCategories();

  return (
    <Container>
      <h1>Shop</h1>
      <nav>
        <ul>
          <li>
            <Link to="/">Home</Link>
          </li>
          <li>
            <Link to="/products">Products</Link>
            {!!categories.length && (
              <ul>
                {categories.map((category) => (
                  <li key={category.id}>
                    <Link to={`/products?categoryId=${category.id}`}>
                      {category.name}
                    </Link>
                  </li>
                ))}
              </ul>
            )}
          </li>
          <li>
            <Link to="/cart">Cart</Link>
          </li>
        </ul>
      </nav>
    </Container>
  );
}
```

- Header 컴포넌트는 카테고리 목록을 얻어서 렌더링한다.
- 카테고리 목록이 없으면 렌더링하지 않는다.
- 만약 코드가 길어지면 컴포넌트를 분리한다.

### apiService

```tsx
export default class ApiService {
  private instance = axios.create({
    baseURL: API_BASE_URL,
  });

  async fetchCategories(): Promise<Category[]> {
    const { data } = await this.instance.get('/categories');
    const { categories } = data;
    return categories;
  }

  async fetchProducts({ categoryId }: {
    categoryId?: string
  } = {}): Promise<ProductSummary[]> {
    const { data } = await this.instance.get('/products', {
      params: { categoryId },
    });
    const { products } = data;
    return products;
  }
}

export const apiService = new ApiService();
```

- 카테고리도 store를 만드려고 보니 axios를 호출하는 부분이 계속 중복되고 앞으로 더 추가될 예정이라 apiService를 추가한다.
- apiService는 axios를 사용하여 api를 호출하는 클래스이다.
- apiService는 카테고리 목록과 상품 목록을 얻는 메서드를 제공하고 추후에 더 추가될 수 있다.

### store

```tsx
import { singleton } from 'tsyringe';
import { Action, Store } from 'usestore-ts';

import { apiService } from '../services/ApiService';

import { Category } from '../types';

@singleton()
@Store()
export default class CategoriesStore {
  categories: Category[] = [];

  async fetchCategories() {
    this.setCategories([]);

    const categories = await apiService.fetchCategories();

    this.setCategories(categories);
  }

  @Action()
  setCategories(categories: Category[]) {
    this.categories = categories;
  }
}
```

- 카테고리 목록을 얻어서 store에 저장한다.
- 기존에 있던 products store와 비슷하다.

### useFetchCategories

```tsx
export default function useFetchCategories() {
  const store = container.resolve(CategoriesStore);
  const [{ categories }] = useStore(store);

  useEffect(() => {
    store.fetchCategories();
  }, [store]);

  return { categories };
}
```

- 카테고리 목록을 얻어서 반환한다.
- 기존에 있던 useFetchProducts와 비슷하다.

기존코드에서 `apiService`와 `useFetch**`은 구조가 바뀌었기 때문에 product에서도 같이 수정해서 적용을 해준다!

## codeceptjs 메서드 타입 문제

codeceptjs에서 `I` 관련된 메서드를 사용할 때, 타입 문제가 발생한다.  
`'I' 형식에 'amOnPage' 속성이 없습니다.ts(2339)` 라고 나오는데 codeceptjs github에 들어가보니 typescript boilerplate에 나와있는 설정을 적용하니 타입이 잡히고 빨간줄이 사라졌다.

```ts
// steps.d.ts
/// <reference types='codeceptjs' />

declare namespace CodeceptJS {
  interface SupportObject {
    I: I;
  }

  interface Methods extends Playwright, REST, JSONResponse {
  }

  interface I extends WithTranslation<Methods> {} // 이부분을 수정하면 된다.

  namespace Translation {
    interface Actions {
    }
  }
}

```