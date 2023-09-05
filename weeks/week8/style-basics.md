---
description: Style Basics에 대해 알아보자
---

# Style Basics

## [className](https://ko.legacy.reactjs.org/docs/faq-styling.html)

React에서는 HTML의 class 속성을 className Prop에 문자열로 전달한다.

```jsx
render() {
  return <span className="menu navigation-menu">Menu</span>
}

render() {
  let className = 'menu';
  if (this.props.isActive) {
    className += ' menu-active';
  }
  return <span className={className}>Menu</span>
}

// 인라인 스타일
const divStyle = {
  color: 'blue',
  backgroundImage: 'url(' + imgUrl + ')',
};

function HelloWorldComponent() {
  return <div style={divStyle}>Hello World!</div>;
}
```

- JSX에서는 class 속성을 className으로 사용
- 인라인 스타일은 객체로 전달해서 사용
  - 보통의 경우 CSS 클래스가 인라인 스타일보다 더 나은 성능을 보여준다.

## [의미있는 마크업](https://www.w3schools.com/html/html5_semantic_elements.asp)

의미있는 마크업 보통 시멘틱 마크업이라고 많이 부른다.  
마크업을 할 때 의미를 잘 전달할 수 있도록 하는 것이 중요하다.

- 의미있는 마크업을 위해 HTML5에서는 새로운 요소를 도입했다.
  - header, nav, section, article, aside, footer
- 의미있는 마크업을 위해 시맨틱 태그를 사용한다.
  - 시맨틱 태그는 브라우저, 검색엔진, 개발자 모두에게 의미있는 정보를 제공한다.
  - 시맨틱 태그는 레이아웃을 위한 태그가 아니다.
- 웹 접근성을 높이기 위해 사용
  - 시각장애인이 스크린리더를 사용할 때 시맨틱 태그를 사용하면 더 의미있는 정보를 전달할 수 있다.
  - 시맨틱 태그를 사용하면 스크린리더가 더 의미있는 정보를 전달할 수 있다.
- [HTML 참고서 MDN](https://developer.mozilla.org/ko/docs/Web/HTML/Reference)
- [HTML 태그들 W3C school](https://www.w3schools.com/tags/default.asp)