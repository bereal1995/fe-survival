---
description: Playwright에 대해 알아보자
---

# Playwright

## E2E(End to End) Test

End to End 테스트라고 하며 애플리케이션의 흐름을 처음부터 끝까지(End to End) 테스트하는 방법이다.  
E2E 테스트는 사용자 관점에서 테스트를 진행한다.

- 사용자가 실제로 사용하는 환경(브라우저)에서 테스트를 진행한다.
- 사용자가 실제로 사용하는 환경이기 때문에 비교적 테스트가 느리다.
  - 이부분 때문에 병렬테스트를 하기위한 도구들을 사용한다.
- 사용자 관점에서 테스트를 진행하기 때문에 테스트 코드를 작성하기 어렵다.
- 테스트 도구 설치는 대체로 간단하지만, 테스트 작성이나 유지보수가 어렵기때문에 애물단지가 되기 쉽다.

## [Headless Chrome](https://developer.chrome.com/blog/headless-chrome/)

Headless Chrome은 브라우저의 UI가 없는 Chrome이다.  
Headless Chrome이전에는 PhantomJS가 있었으나,  
Chrome에서 headless모드를 추가한 후로 PhantomJS는 더이상 개발이 되지 않고 있다.

- 브라우저의 UI가 없기 때문에 브라우저를 직접 사용하는 것보다 빠르다.
- puppeteer, playwright 등의 라이브러리에서도 headless모드를 지원한다.

## [Puppeteer](https://developer.chrome.com/docs/puppeteer/)

[Puppeteer는 DevTools 프로토콜](https://chromedevtools.github.io/devtools-protocol/)을 통해 헤드리스 Chrome 또는 Chromium을 제어하기 위한 고급 API를 제공하는 노드 라이브러.  
Chrome 또는 Chromium을 사용하도록 구성할 수도 있습니다.

- 페이지를 렌더링하고 스크린샷 및 PDF를 생성하는 등의 작업을 수행할 수 있다.
- SPA를 크롤링하고 미리 렌더링된 콘텐츠를 생성한다.
- UI 테스트 같은 작업을 자동화 한다.
- Chrome 확장 프로그램을 테스트 할 수 있다.

## Playwright?

웹 브라우저 기반 E2E 테스트 자동화 도구.  
Headless Chrome을 기반으로 한 Puppeteer를 계승하면서, 더 많은 웹 브라우저를 지원한다.

- Puppeteer와 비슷한 라이브러리이다.  
- Puppeteer는 Chrome만 지원하는 반면, Playwright는 Chrome, Firefox, Webkit을 지원한다.  
- Puppeteer는 Chrome의 DevTools 프로토콜을 사용하지만, Playwright는 DevTools 프로토콜을 사용하지 않는다.  
- Playwright는 DevTools 프로토콜을 사용하지 않기 때문에 Chrome, Firefox, Webkit을 지원할 수 있다.  
- DevTools 프로토콜을 사용하지 않기 때문에 Puppeteer보다 빠르다.  

### Playwright 간단 설정

설치

```shell
npm i -D @playwright/test eslint-plugin-playwright
```

파일 설정

```ts
// playwright.config.ts
import { PlaywrightTestConfig } from '@playwright/test';

const config: PlaywrightTestConfig = {
  testDir: './tests',
  retries: 0,
  use: {
    baseURL: 'http://localhost:8080',
    headless: !!process.env.CI,
    screenshot: 'only-on-failure',
  },
};

export default config;

// tests/.eslintrc.js
module.exports = {
  env: {
    jest: false,
  },
  extends: ['plugin:playwright/playwright-test'],
  rules: {
    'import/no-extraneous-dependencies': 'off',
  },
};

// tests/home.spec.ts
import { test, expect } from '@playwright/test';

test('Filter products', async ({ page }) => {
  await page.goto('/');

  await expect(page.getByText('Apple')).toBeVisible();

  const searchInput = page.getByLabel('Search');

  await searchInput.fill('a');

  await expect(page.getByText('Apple')).toBeVisible();

  await searchInput.fill('aa');

  await expect(page.getByText('Apple')).toBeHidden();
});

test('Click the “Increase” button', async ({ page }) => {
  await page.goto('/');

  const count = 13;

  await Promise.all((
    [...Array(count)].map(async () => {
      await page.getByText('Increase').click();
    })
  ));

  await expect(page.getByText(`${count}`)).toBeVisible();
});
```

사용

```shell
npx playwright test
```

{% hint style="info" %}
`test-results` 폴더에는 에러가 발생한 경우에만 스크린샷 등이 저장된다.  
gitignore에 추가해주는 것이 좋다.
{% endhint %}

## CodeceptJS

인간 친화적인 E2E 테스트 프레임워크.  

- Puppeteer, Playwright, WebDriverIO 등의 라이브러리를 지원한다.
- BDD, TDD, BDT 등의 테스트 스타일을 지원한다.
- 테스트를 작성하기 위한 도구들을 제공한다.
- 테스트를 작성하기 쉽다.
  - 테스트를 작성하기 쉽기 때문에 비개발자도 작성하거나, 테스트의 흐름을 이해하기 쉽다.
- Codecept 참고용
  - [CodeceptJS](https://codecept.io/)
  - [CodeceptJS 3 시작하기](https://github.com/ahastudio/til/blob/main/test/20201207-codeceptjs.md)
  - [CodeceptJS 사용](https://github.com/ahastudio/CodingLife/tree/main/20211012/react#codeceptjs-사용)