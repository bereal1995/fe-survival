# 개발환경

## [Node.js](https://nodejs.org/en)

### Node.js란?

Node.js는 Chrome V8 JavaScript 엔진으로 빌드된 JavaScript 런타임입니다.

- 설치할 때는 최신 버전을 확인해야 한다.
  - 혹시모르니 영어버전으로 한번씩 체크하는걸 추천
- LTS (Long Term Support)
  - 장기적으로 지원되는 버전
  - 취약점 패치, 버그 수정, 보안 업데이트 등이 지속적으로 이루어짐
  - Active / Maintenance
    - Active LTS는 현재 사용 가능한 버전
    - Maintenance LTS는 Active LTS가 종료되고, 1년간 지원되는 버전
  - Long Term은 30개월을 의미함
- 최신버전 (Current)
  - 최신 기능을 사용할 수 있음
  - 새로운 버전이 나오면 LTS로 이동함

## [fnm](https://github.com/Schniz/fnm)

Rust에 내장된 빠르고 간단한 Node.js 버전 관리자

- Rust에 내장된 빠르고 간단한 Node.js 버전 관리자
- 단일 파일, 쉬운 설치, 즉시 시작
- 속도를 염두에 두고 제작
- .node-version및 .nvmrc파일 과 함께 작동

### 설치 및 사용방법

```shell
# fnm 설치
brew install fnm

# 노드js 설치
fnm install --lts 
fnm install {version}

# 목록 확인
fnm list

# zshrc에 추가
eval "$(fnm env --use-on-cd)"

# fnm 사용
fnm use {version}
```

## NPM(Node Package Manager)

이름 그대로 Node.js의 패키지를 관리하는 도구

### package.json

- 프로젝트의 정보를 담고 있는 파일
- 프로젝트의 이름, 버전, 라이센스, 의존성 등을 명시
- npm init 명령어로 생성 가능
- npm install 명령어로 의존성 설치 가능
- npm install --save-dev 명령어로 개발 의존성 설치 가능
- npm install --global 명령어로 전역 설치 가능

### package-lock.json

- npm 명령어를 통해 패키지를 설치하면 자동으로 생성되는 파일
- package.json에 ~(틸드), ^(캐럿) 등의 표시가 있으면 package-lock.json에는 정확한 버전의 패키지를 다운받기 위한 정보가 담김
  > [major, minor, patch] 버전에서  
  > 틸드(~)는 patch 버전까지만 업데이트 허용  
  > 캐럿(^)은 minor 버전까지만 업데이트 허용  
  > [정확하게 알아보기](https://semver.npmjs.com/)
- package-lock.json은 버전을 고정시키는 역할을 하기 때문에 버전을 업데이트하고 싶다면 삭제 후 다시 설치해야 함
- 따라서 여러환경에서 작업을 하고 있다면 깃허브에 올릴 때 package-lock.json을 함께 올리는 것이 좋음

### node_modules

- npm 명령어를 통해 패키지를 설치하면 자동으로 생성되는 폴더
- package.json에 명시된 의존성 패키지들이 설치되는 폴더
- 디렉토리 안에는 정말 많은 모듈들이 들어가있기 때문에 .gitignore에 추가하는 것이 좋음

### npx (Node Package Execute)

- npm registry에 올라가있는 패키지를 설치하지 않고 바로 실행할 수 있음
- npx [패키지명] [옵션] [인자] 형식으로 사용

## ES Modules vs CommonJS

### CommonJS

- 브라우저 외에서도 사용가능한 범용적인 모듈 시스템
- 동기적으로 모듈을 로드
- require, module.exports 키워드를 사용
- Node.js에서 사용
- 동기적으로 모듈을 로드하기 때문에 모듈 로드가 끝날 때까지 다른작업을 할 수 없다. 때문에 성능이 저하될 수 있다.

### AMD(Asynchronous Module Definition), UMD(Universal Module Definition)

- ES Modules 이전에 사용되던 모듈 시스템으로 AMD 이후에 UMD가 등장했다.
- AMD는 브라우저에서 비동기적으로 모듈을 로드할 수 있는 모듈 시스템을 제공하기 위해 개발되었다.
- UMD는 CommonJS와 AMD를 모두 지원하는 모듈 시스템으로, 브라우저와 서버에서 모두 사용할 수 있는 특징이 있다.

### ES Modules

- 브라우저와 Node.js에서 모두 사용가능
- 동기적으로 모듈을 로드
- import, export 키워드를 사용
- CommonJS와 다르게 스크립트를 바로 실행하지 않고 import, export구문을 찾아 스크립트를 실행한다.
- 모듈을 병렬로 로드하지만, 실행은 순차적으로 진행한다.
- 브라우저에서 지원하지만 통상 번들러를 통해 사용한다.

## 컨닝페이퍼

```shell
mkdir {프로젝트이름}
cd {프로젝트이름} 

code .

# pakage.json 추가
npm init -y
# .gitignore 추가!!!

# typescript 설치
npm i -D typescript
# typescript 기본 셋팅
npx tsc --init

# ESLint 설치
npm i -D eslint
# ESLint 기본 셋팅
npx eslint --init
# .eslint.js 수정 및 jest:true 설정

# react 설치
npm i react react-dom
npm i -D @types/react @types/react-dom

# 테스팅 도구 설치
npm i -D jest @types/jest @swc/core @swc/jest \
    jest-environment-jsdom \
    @testing-library/react @testing-library/jest-dom
# jest.config.js 파일을 작성해 테스트에서 SWC를 사용하자.
# 참고 https://github.com/ahastudio/CodingLife/blob/main/20220726/react/jest.config.js

# Parcel 설치
npm i -D parcel
# package.json scripts 수정
# 참고 https://github.com/ahastudio/CodingLife/blob/main/20220726/react/package.json

```
