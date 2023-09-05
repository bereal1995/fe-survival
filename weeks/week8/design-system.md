---
description: Design System에 대해 알아보자
---

# Design System

## [반응형 웹 디자인(Responsive web design)](https://alistapart.com/article/responsive-web-design/)

뷰포트 너비에 따라 레이아웃이 변경되는 웹 디자인 기법  
반응형 웹 디자인은 미디어 쿼리를 사용하여 뷰포트 너비에 따라 레이아웃을 변경한다.  
반응형 웹 디자인은 뷰포트 너비에 따라 레이아웃을 변경하는 것 외에도 미디어 쿼리를 사용하여 뷰포트 너비에 따라 이미지 크기를 변경하거나, 뷰포트 너비에 따라 다른 이미지를 보여주는 등의 다양한 방법으로 반응형 웹 디자인을 구현할 수 있다.

- Fluid Grid
  - 뷰포트 너비에 따라 컨텐츠의 너비가 변경되는 레이아웃 기법
  - 뷰포트 너비가 변경되면 컨텐츠의 너비가 변경되므로 컨텐츠의 너비가 뷰포트 너비에 따라 유동적으로 변한다.
- Flexible Images
  - 뷰포트 너비에 따라 이미지의 크기가 변경되는 기법
  - 뷰포트 너비가 변경되면 이미지의 크기가 변경되므로 이미지의 크기가 뷰포트 너비에 따라 유동적으로 변한다.
- Media Queries
  - 미디어 쿼리를 사용하여 뷰포트 너비에 따라 레이아웃, 이미지 크기를 변경하거나, 뷰포트 너비에 따라 다른 이미지를 보여주는 등의 다양한 방법으로 반응형 웹 디자인을 구현할 수 있다.

## [디자인 시스템(Design System)](https://24ways.org/2012/design-systems/)

타이포그래피, 레이아웃, 색상 등의 핵심 디자인 요소와 이를 구현하는 방법, 코드 등을 문서화한 것  
핵심 디자인 요소를 중심으로 생성된 디자인 패턴을 문서화하여 디자인 시스템을 구축한다.

- 항상 일관된 디자인을 구현할 수 있다.
  - 모든 디바이스에서 일관된 디자인을 구현할 수 있다.
- 실용적인 디자인을 구현할 수 있다.
  - 디자인 시스템을 구축하면 디자인 시스템을 사용하는 모든 디자이너와 개발자가 항상 일관된 디자인을 구현할 수 있다.

### 관련 아티클

- 디자인 시스템
  - [Laura Kalbag의 “Design Systems” 소개](https://24ways.org/2012/design-systems/)
  - [Laura Kalbag의 “Design Systems” 슬라이드](https://speakerdeck.com/laurakalbag/design-systems-1)
  - [Nielsen Norman Group의 “Design Systems 101”](https://www.nngroup.com/articles/design-systems-101/)
- UX분야에서 전설적인 인물들
  - [제이콥 닐슨](https://ko.wikipedia.org/wiki/제이콥_닐슨)
  - [도널드 노먼](https://ko.wikipedia.org/wiki/도널드_노먼)
  - [앨런 쿠퍼](https://en.wikipedia.org/wiki/Alan_Cooper)
- 디자인시스템 사례
  - [Atlassian Design System](https://atlassian.design/)
  - [Material Design (Google)](https://material.io/)
  - [Base Web (Uber)](https://baseweb.design/)
  - [Polaris (Shopify)](https://polaris.shopify.com/)
  - [Lightning Design System (Salesforce)](https://www.lightningdesignsystem.com/)
  - [Mailchimp Pattern Library](https://ux.mailchimp.com/patterns)
  - [Ant Design](https://ant.design/)
- 디자인시스템 Gallery
  - [Design Systems Gallery](https://designsystemsrepo.com/design-systems/)
  - [Design Systems](https://www.designsystems.com/open-design-systems/)

## [Atomic Design](https://bradfrost.com/blog/post/atomic-web-design/)

> [Atomic Design 전자책](https://atomicdesign.bradfrost.com/)

페이지를 구성하는 모든 요소를 아토믹(Atomic)한 단위로 분류하고 이를 조합하여 인터페이스를 구축하는 방법론  
아토믹(Atomic)한 단위는 화학에서 원자, 분자, 유기체, 유기체 집합체 등과 같이 더 이상 분할할 수 없는 단위를 의미한다.  
아토믹디자인은 디자인시스템을 보다 체계적으로 구성할 수 있는지를 고민하던 중 나오게 된 방법론이다.

### 아토믹의 5단계

- Atoms
  - 원자 단위의 UI 요소
  - 버튼, 입력 필드, 레이블, 헤딩 등과 같은 HTML 태그
  - 색상 팔레트, 글꼴 같은 눈에 보이지 않는 요소도 포함
- Molecules
  - 원자 단위의 UI 요소들의 결합
  - 검색 폼, 로그인 폼 등
  - 복잡할 수 있지만 재사용을 고려하여 설계
- Organisms
  - 분자 단위의 UI 요소들의 집합
  - 헤더, 푸터, 사이드바 등
  - 독립적으로 존재할 수 있지만 페이지를 구성하는 데 사용
- Templates
  - 페이지의 레이아웃을 구성하는 컴포넌트들의 레이아웃
  - 헤더, 푸터, 사이드바 등이 포함된 페이지의 레이아웃
- Pages
  - 실제 컨텐츠가 표시되는 페이지
  - 뉴스 페이지, 블로그 페이지 등
