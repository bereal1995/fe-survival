---
description: Fetch API & CORS에 대해 알아보자
---

# Fetch API & CORS

## Fetch API 란

[Fetch API](https://developer.mozilla.org/ko/docs/Web/API/Fetch\_API/Using\_Fetch)는 HTTP 요청을 보내는 API이다.

* Fetch API는 Promise를 반환한다.
* Promise를 사용하면 비동기 작업을 동기 작업처럼 사용할 수 있다.
* 콜백 기반 API였던 XMLHttpRequest에서 Fetch API는 서비스 워커에서도 쉽게 사용할 수 있도록 Promise 기반으로 개선된 대체 API이다.

기본적인 사용법은 다음과 같다.

```js
await fetch('https://jsonplaceholder.typicode.com/todos/1');
// → Response

const response = await fetch('https://jsonplaceholder.typicode.com/todos/1');
// → response.body는 ReadableStream

const reader = response.body.getReader();

const chunk = await reader.read();
// → chunk.value는 Uint8Array 타입.
// → 원래는 chunk.done이 true일 때까지 반복해야 한다.
// value와 done 구조를 갖고 있다면 iterator를 활용한 작업들이 있지 않을까 ?

const body = new TextDecoder().decode(chunk.value);
```

이런식으로 사용하면 데이터를 받아오는데 ReadableStream을 사용해야 해서 또 JSON.parse를 해야하는 등 번거롭다. 그래서 다음과 같이 사용하는 것이 좋다.

```js
const response = await fetch('https://jsonplaceholder.typicode.com/todos/1');
const data = await response.json();
```

2번째 인자로 [여러가지 옵션](https://developer.mozilla.org/ko/docs/Web/API/fetch)을 줄 수 있다.

```js
const response = await fetch('https://jsonplaceholder.typicode.com/todos/1', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    title: 'foo',
    body: 'bar',
    userId: 1,
  }),
});
```

## Promise

[Promise](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global\_Objects/Promise)는 프로미스가 생성된 시점에는 알려지지 않았을 수도 있는 값을 위한 대리자로, 비동기 연산이 종료된 이후에 결과 값과 실패 사유를 처리하기 위한 처리기를 연결할 수 있습니다.(`비동기 처리를 위해 콜백 함수를 사용할 수 있습니다.`)\
프로미스를 사용하면 비동기 메서드에서 마치 동기 메서드처럼 값을 반환할 수 있습니다.(`return`)\
다만 최종 결과를 반환하는 것이 아니고, 미래의 어떤 시점에 결과를 제공하겠다는 '약속'(프로미스)을 반환합니다.(`Promise 객체`)

* 크게 3가지 상태를 가진다.
  * 대기(pending): 이행하거나 거부되지 않은 초기 상태.
  * 이행(fulfilled): 연산이 성공적으로 완료됨.
  * 거부(rejected): 연산이 실패함.

## ReadableStream

Fetch API는 Response 객체의 body 속성을 통하여 [ReadableStream](https://developer.mozilla.org/ko/docs/Web/API/ReadableStream)의 구체적인 인스턴스를 제공한다고 한다.

* ReadableStream은 읽기 가능한 스트림을 나타내는 인터페이스이다.
* properties
  * locked: 스트림이 잠겨있는지 여부를 나타낸다. (getter)
* Methods
  * cancel(): 스트림을 취소한다.
  * getReader(): 스트림의 reader를 반환한다.
  * pipeThrough(): 스트림을 다른 스트림과 파이프라인으로 연결한다.
  * pipeTo(): 스트림을 다른 스트림으로 연결한다.
  * tee(): 스트림을 복제한다.

## Unicode

[Unicode](https://developer.mozilla.org/ko/docs/Glossary/Unicode)는 전 세계의 모든 문자를 컴퓨터에서 일관되게 표현하고 다룰 수 있도록 설계된 산업 표준이다.\
하나의 데이터가 여러 언어를 한번에 가지기 어려웠고 오류에도 취약했었다.\
이를 해결하기 위해 유니코드가 등장했다.

> 어떤 글이 `占쏙옙`처럼 말도 안되는 글자로 표시되는걸 본 적이 있다면,\
> 실제로 프로그램이 문자를 적절히 처리하지 못한 예시를 확인한 것입니다.

* UTF-8, UTF-16, UTF-32 등이 있다.
* UTF-8은 가변 길이 문자 인코딩 방식이다.
  * 웹에서 제일 널리 쓰이는 유니코드 인코딩 방식이다.\*\*
* UTF-16은 고정 길이 문자 인코딩 방식이다.
* UTF-32는 고정 길이 문자 인코딩 방식이다.

## CORS 란

[CrossOriginResourceSharing(교차 출처 리소스 공유)](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS)의 약자로, 다른 출처의 리소스를 공유할 수 있도록 해주는 메커니즘이다.

* 웹 애플리케이션은 리소스가 자신의 출처와 다를 때 교차 출처 HTTP 요청을 실행한다.
* 실제로 통신이 성공했어도 CORS 제약조건을 위반했다면, 브라우저에서 응답을 차단한다.
* CORS를 사용하는 요청
  * XMLHttpRequest, Fetch API
  * Web Fonts (CSS에서 `@font-face`를 사용할 때)
  * WebGL 텍스처
  * drawImage()를 사용하여 `<canvas>` 요소에 그릴 때
  * 이미지/비디오의 픽셀 데이터를 읽을 때

## SOP

[SameOriginPolicy(동일 출처 정책)](https://developer.mozilla.org/ko/docs/Web/Security/Same-origin\_policy)의 약자로, 동일 출처 정책은 웹 브라우저 보안을 위해 만들어진 정책이다.\
한 origin으로부터 로드된 문서나 스크립트가 다른 origin의 리소스와 상호작용하는 것을 제한하는 보안 방식이다.

* 동일 출처 정책은 웹 브라우저 보안을 위해 만들어진 정책이다.
* 동일 출처 정책은 웹 브라우저의 스크립트에서 온라인으로 불러온 문서나 스크립트가 다른 출처의 자원과 통신하지 못하도록 제한하는 보안 방식이다.
* 동일 출처 정책은 다른 출처의 자원을 불러오는 것을 제한한다.

### SameOrigin

<table><thead><tr><th width="140.33333333333331">SameOrigin</th><th width="394">예시</th><th>비고</th></tr></thead><tbody><tr><td>O</td><td><a href="https://www.hhxdragon.com/page.html">https://www.hhxdragon.com/page.html</a><br><a href="https://www.hhxdragon.com/page.html">https://www.hhxdragon.com:443/page.html</a></td><td>-</td></tr><tr><td>X</td><td><a href="https://www.bereal1995.hhxdragon.com/">https://www.bereal1995.hhxdragon.com/</a><br><a href="https://www.bereal1995.hhxdragon.com:8443/">https://www.bereal1995.hhxdragon.com:8443/</a></td><td>둘이 포트가 다르기 때문에 SameOrigin이 아니다</td></tr></tbody></table>

## 추가로 궁금한 것들

* 웹 서버에서는 비동기 통신을 어떻게 구현할까?
* fetch에서 작업을 취소하려면 ?
  * AbortController
  * AbortSignal
* SOP, CORS 문서 자세히 읽어보기
