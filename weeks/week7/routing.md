---
description: routing에 대해 알아보자
---

# Routing

## [HTML DOM API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API)

HTML DOM API는 HTML 문서의 구조를 표현하는 객체 모델이다.  
각 요소의 기능을 정의하는 인터페이스와 해당 요소가 의존하는 지원 유형 및 인터페이스로 구성된다.

- DOM을 통해 HTML 요소에 접근하고 조작할 수 있다.
- 양식 데이터에 접근하고 조작할 수 있다.
- 2D 이미지의 콘텐츠 및 HTML의 Canvas의 컨텍스트와 상호 작용
- HTML 미디어 요소(오디오, 비디오)에 연결된 미디어 관리
- 웹 페이지에서 콘텐츠 드래그 앤 드롭
- 브라우저 탐색 기록에 접근
- 웹 컴포넌트, 웹 스토리지, 웹 워커, 웹소켓 및 서버 전송 이벤트와 같은 기타 API에 대한 지원 및 연결

### [Location](https://developer.mozilla.org/ko/docs/Web/API/Location)

객체가 연결된 장소(URL)를 표현한다. Document와 Window 인터페이스는 Location 객체를 통해 현재 문서의 URL을 가져오거나 변경할 수 있다.

#### 속성

- Location.href : URL 전체를 반환
- Location.protocol : URL의 프로토콜을 반환 (ex. `https:`)
- Location.host : URL의 호스트를 반환 (ex. `www.google.com`), 포트 번호를 포함한다.
- Location.hostname : URL의 호스트 이름을 반환 (ex. `www.google.com`), 포트 번호를 포함하지 않는다.
- Location.port : URL의 포트 번호를 반환 (ex. `:80`)
- Location.pathname : URL의 경로를 반환 (ex. `/search`)
- Location.search : URL의 쿼리를 반환 (ex. `?q=hello`)
  - `URLSearchParams`를 사용하여 쉽게 파싱할 수 있다.
- Location.hash : URL의 해시를 반환 (ex. `#test`)
- Location.username : 도메인 이름 이전에 명시된 사용자 이름을 반환
- Location.password : 도메인 이름 이전에 명시된 비밀번호를 반환
- Location.origin(읽기전용) : URL의 프로토콜, 호스트, 포트를 반환 (ex. `https://www.google.com:80`)

#### 메소드

- Location.assign(url) : 주어진 URL의 리소스를 불러온다.
- Location.reload() : 현재 문서를 다시 불러온다. 매개변수로 true를 전달하면 캐시를 무시하고 서버에서 리소스를 다시 불러온다.
- Location.replace(url) : 현재 문서를 주어진 URL의 리소스로 대체한다. 뒤로가기를 눌러도 이전 페이지로 돌아갈 수 없다.
- Location.toString() : URL 전체를 반환한다. 값을 수정할수는 없지만 `window.location.href`와 같은 결과를 반환한다.

### pathname

pathname은 URL의 경로를 나타내는 부분이다. 예를 들어 `https://www.google.com/search?q=hello`라는 URL이 있다면, pathname은 `/search`가 된다.

```js
// Let's say we are on the URL https://developer.mozilla.org/en-US/docs/Web/API/Location/pathname#examples
console.log(location.pathname); // '/en-US/docs/Web/API/Location/pathname'
```

- `window.location.pathname` : 현재 페이지의 경로를 반환
- `window.location.pathname = '/hello'` : 현재 페이지의 경로를 `/hello`로 변경
