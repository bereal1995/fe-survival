---
description: Parcel & ESLint에 대해 알아보자
---

# Parcel & ESLint

## Parcel

- Parcel은 웹 애플리케이션 번들러이다.
- zero-configuration 즉 config 설정 없이 바로 사용할 수 있다.
- 내부적으로 SWC를 사용해 기존 도구보다 빠르다고 한다.

## SWC

- SWC는 Rust로 작성된 TypeScript, JavaScript 컴파일러이다.
- SWC 이전에는 Babel(트랜스파일)과 Terser(경량화)를 사용했는데, SWC는 이 둘을 합쳐서 사용한다.
- SWC는 Rust로 작성되어 있기 때문에 속도가 빠르다고 한다. 이벤트 루프기반의 싱글 스레드 자바스크립트와는 다르게 Rust는 멀티 스레드를 지원하기 때문에 빠르다고 한다.
- SWC는 싱글스레드 환경에서도 Babel보다 빠르다고 한다. [벤치마크 결과](https://swc.rs/docs/benchmarks)

## ESLint

- ESLint는 자바스크립트 코드에서 문제점을 검사하고, 코드 스타일을 정하는 데 도움을 준다.
- 정적 분석 도구라고 하는데 저번에 알아봤던 테스트 개념중에서 정적 분석 도구로 볼 수 있다.
- Editor에서 파일 저장시 자동으로 코드를 수정해주는 Fix 기능을 사용하면 굉장히 편하다.
- 사용하다보면 ESLint에서 권장하는 코드 스타일을 따르게 되어, 코드 스타일을 일관성 있게 유지할 수 있다.
- eslintrc 파일을 통해 커스텀하게 설정을 할 수 있다.

## 컨닝페이퍼

Parcel

```json
// parcel 빌드 시작점
"source": "./index.html",

// 정적 파일들 같이 빌드
{
  "extends": ["@parcel/config-default"],
  "reporters":  ["...", "parcel-reporter-static-files-copy"]
}
```

```shell
# parcel 빌드
npx parcel build

# 정적 서버 실행
npx servor ./dist
```

VSCode 기본 셋팅

```json
{
  "editor.rulers": [
    80
  ],
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "trailing-spaces.trimOnSave": true
}
```
