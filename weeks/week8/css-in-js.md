---
description: CSS in JS에 대해 알아보자
---

# CSS in JS

## [CSS in JS](https://en.wikipedia.org/wiki/CSS-in-JS) 란

CSS in JS는 JavaScript를 사용하여 구성 요소의 스타일을 지정하는 스타일 관련 기술이다.

- JavaScript가 구문 분석되면 CSS가 생성되어 DOM`<style>`에 첨부 된다.
- 선언적, 유지관리 가능, 재사용 가능한 스타일을 위해 JavaScript를 사용하여 CSS를 구성 요소 수준 자체로 추상화할 수 있다.
- 대표적으로 몇가지 라이브러리들이 있다.
  - [styled-components](https://styled-components.com/)
  - [emotion](https://emotion.sh/docs/introduction)
  - [JSS](https://cssinjs.org/?v=v10.8.0)
  - [tailwindcss](https://tailwindcss.com/)

### [React: CSS in JS (2014)](https://blog.vjeux.com/2014/javascript/react-css-in-js-nationjs.html)

1. Global Namespace
2. Dependencies
3. Dead Code Elimination
4. Minification
5. Sharing Constants
6. Non-deterministic Resolution
7. Isolation

### [A Unified Styling Language (2017)](https://blog.rhostem.com/posts/2017-06-24-unified-styling-language)

1. Scoped styles
2. Critical CSS
3. Smarter optimisations
4. Package management
5. Non-browser styling

### [All You Need To Know About CSS-in-JS (2017)](https://d0gf00t.tistory.com/22)

1. Thinking in components
2. CSS-in-JS **leverages the full power of the JavaScript ecosystem** to enhance CSS.
3. True rules isolation
4. Scoped selectors
5. Vendor Prefixing
6. Code sharing
7. **Only the styles which are currently** in use on your screen are also in the DOM (react-jss).
8. Dead code elimination
9. Unit tests for CSS

### CSS-in-JS 라이브러리 인기도

- [CSS-in-JS 라이브러리 인기도](https://npmtrends.com/aphrodite-vs-emotion-vs-glamorous-vs-jss-vs-radium-vs-styled-components-vs-styletron)

### 성능 관련

> [CSS-in-JS와 성능 (2021)](https://hyeonseok.com/blog/877)
→ CSS 파일과 JS 파일 로딩의 차이.
> [Why We're Breaking Up with CSS-in-JS (2022)](https://bit.ly/3g6QufF)

### 대안

- [Linaria](https://linaria.dev/)
  - CSS를 평범한 텍스트로 작성.
  - React에 종속적이지 않지만, React Styled Component도 지원함.
- [vanilla-extract](https://vanilla-extract.style/)
  - CSS를 오브젝트 형태로 표현. React의 Inline Style과 유사함.
  - React와 무관하게 사용 가능.

## CSS

- [css 참고서](https://developer.mozilla.org/ko/docs/Web/CSS/Reference)
- flexbox
- Grid
