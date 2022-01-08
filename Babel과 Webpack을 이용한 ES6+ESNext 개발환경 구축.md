# 49. Babel과 Webpack을 이용한 ES6+/ESNext 개발환경 구축

## 0. 서론

- 모던 브라우저의 ES6 지원율은 약 98%로 거의 대부분의 ES6 사양을 지원한다.
- 하지만 IE의 지원율을 11%이다.
- 따라서 ES6+와 ESNext의 최신 ECMAScript 사양을 사용하여 프로젝트를 진행하려면 최신 사양으로 작성된 코드를 겨우에 따라 IE를 포함한 구형 브라우저에서 문제 없이 동작시키기 위한 개발 환경을 구축하는것이 필요하다.
- 또한 대부분의 프로젝트가 모듈을 사용하므로 모듈 로더도 필요하다.
- ES6 모듈(ESM)은 대부분의 모던 브라우저에서 사용할 수 있지만 여러 이유로 아직까지는 ESM 보다는 별도의 모듈 로더를 사용하는 것이 일반적이다.
- **모듈 로더 사용 이유**
    - IE를 포함한 구형 브라우저는 ESM을 지원하지 않는다.
    - ESM을 사용하더라도 트랜스파일링이나 번들링이 필요한 것은 변함이 없다.
    - ESM이 아직 지원하지 않는 기능이 있고 점차 해결되고는 있지만 아직 몇가지 이슈가 존재한다.
- 트랜스파일러인 Babel과 모듈 번들러인 Webpack을 이용하여 ES6+/ESNext 개발 환경을 구축해보자.
- Webpack을 통해 Babel을 로드하여 ES6+/ESNext 사양의 소스코드를 IE같은 구형 브라우저에서도 동작하도록 ES5 사양의 소스코드로 트랜스파일링하는 방법도 알아보자.

## 1. Babel

- 다음 예제에서는 ES6의 화살표 함수와 ES7의 지수 연산자를 사용하고 있다.

```jsx
[1, 2, 3].map(n => n ** n);
```

- IE 같은 구형 브라우저에서는 ES6의 화살표 함수와 ES7의 지수 연산자를 지원하지 않을 수 있다.
- Babel을 사용하면 위 코드를 다음과 같이 ES5 사양으로 변환할 수 있다.

```jsx
"use strict";

[1, 2, 3].map(function (n) {
  return Math.pow(n, n);
});
```

- 이처럼 Babel은 ES6+/ESNext로 구현된 최신 사양의 소스코드를 IE 같은 구형 브라우저에서도 동작하는 ES5 사양의 소스코드로 변환할 수 있다.

### 1.1 Babel 설치

- npm을 사용하여 Babel을 설치할 수 있다.

```bash
# package.json 생성
$ npm init -y
# babel-core, babel-cli 설치
$ npm install --save-dev @babel/core @babel/cli
```

- 특정 버전을 그대로 설치하고 싶으면 @과 설치하고 싶은 버전을 지정한다.

```bash
# 버전 지정 설치
npm install --save-dev @babel/core@7.10.3 @babel/cli@7.10.3
```

### 1.2 Babel 프리셋 설치와 babel.config.json 설정 파일 작성

- Babel을 사용하려면 @babel/preset-env를 설치해야 한다.
- @babel/preset-env → 함께 사용하는 Babel 플러그인을 모아 둔 것으로 Babel프리셋이라고 부른다.
- Babel이 제공하는 공식 Babel 프리셋은 다음과 같다.
    - @babel/preset-env
    - @babel/preset-flow
    - @babel/preset-react
    - @babel/preset-typescript
- @babel/preset-env는 필요한 플러그인들을 프로젝트 지원 환경에 맞춰서 동적으로 결정해준다.

```bash
# @babel/preset-env 설치
$ npm install --save-dev @babel/preset-env
```

- 설치가 완료되면 프로젝트 루프 폴더에 babel.config.json 설정 파일을 생성하고 아래와 같이 작성한다. → @babel/preset-env를 사용하겠다는 의미이다.

```bash
{
  "presets": ["@babel/preset-env"]
}
```

### 1.3 트랜스파일링

- Babel을 사용하여 ES6+/ESNext 사양의 소스코드를 ES5 사양의 소스코드로 트랜스 파일링할 수 있다.
- Bable CLI 명령어를 사용할 수도 있지만 매번 사용하기 번거로움으로 npm scripts에 Babel CLI 명령어를 등록하면 편리하다.
- package.json에 scripts를 추가한다.

```json
{
  "name": "esnext-project",
  "version": "1.0.0",
  "scripts": {
    "build": "babel src/js -w -d dist/js"
  },
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3",
    "@babel/preset-env": "^7.10.3"
  }
}
```

- npm scripts의 build는 src/js 폴더(타깃 폴더)에 있는 모든 자바스크립트 파일들을 트랜스파일링한 후, 그 결과물을 dist/js 폴더에 저장한다.
- 옵션의 의미
    - -w : 타깃 폴더에 있는 모든 자바스크립트 파일들의 변경을 감지하여 자동으로 트랜스파일한다.(—watch 옵션의 축약형)
    - -d : 트랜스파일링된 결과물이 저장될 폴더를 지정한다. 만약 지정된 폴더가 존재하지 않으면 자동 생성한다.(—out-dir 옵션의 축약형)
- 트랜스 파일링한 코드를 작성하고 src/js폴더를 생헝후 lib.js와main.js를 추가한다.

```jsx
// src/js/lib.js
export const pi = Math.PI; // ES6 모듈

export function power(x, y) {
  return x ** y; // ES7: 지수 연산자
}

// ES6 클래스
export class Foo {
  #private = 10; // stage 3: 클래스 필드 정의 제안

  foo() {
    // stage 4: 객체 Rest/Spread 프로퍼티 제안
    const { a, b, ...x } = { ...{ a: 1, b: 2 }, c: 3, d: 4 };
    return { a, b, x };
  }

  bar() {
    return this.#private;
  }
}
```

```jsx
// src/js/main.js
import { pi, power, Foo } from './lib';

console.log(pi);
console.log(power(pi, pi));

const f = new Foo();
console.log(f.foo());
console.log(f.bar());
```

- 위 코드를 갖고 터미널에 명령어를 입력한다.

```bash
$ npm run build

> esnext-project@1.0.0 build /Users/leeungmo/Desktop/esnext-project
> babel src/js -w -d dist/js

SyntaxError: /Users/leeungmo/Desktop/esnext-project/src/js/lib.js: Support for the experimental syntax 'classPrivateProperties' isn't currently enabled (10:3):

   8 | // ES6 클래스
   9 | export class Foo {
> 10 |   #private = 10; // stage 3: 클래스 필드 정의 제안
     |   ^
  11 |
  12 |   foo() {
  13 |     // stage 4: 객체 Rest/Spread 프로퍼티 제안

...
```

- private 필드 정의 제안 에서 에러가 발생한다.
- @babel/preset-env가 현재 제안 단계에 있는 사양에 대한 플러그인은 지원하지 않아서다.
- 별도의 플러그인을 설치해야 한다.

### 1.4 Babel 플러그인 설치

- 설치가 필요한 Babel 플러그인은 Babel 홈페이지에서 검색할 수 있다.
- 위의 경우 @babel/plugin-proposal-class-properties 플러그인이 필요하다.

```bash
$ npm install --save-dev @babel/plugin-proposal-class-properties
```

- 플러그인 설치 후에는 babel.config.json 설정 파일에 추가해야 한다.

```bash
{
  "presets": ["@babel/preset-env"],
  "plugins": ["@babel/plugin-proposal-class-properties"]
}
```

- 플러그인 추가 설치후 다시 트랜스파일링을 진행한다.

```bash
$ npm run build

> esnext-project@1.0.0 build /Users/leeungmo/Desktop/esnext-project
> babel src/js -w -d dist/js

Successfully compiled 2 files with Babel (954ms).
```

- 트랜스파일링에 성공하면 프로젝트 루트 폴더에 dis/js폴더가 자동으로 생성되고 트랜스 파일링된 main.js와 lib.js가 저장된다.
- main.js를 실행해 보자.

```bash
$ node dist/js/main
3.141592653589793
36.4621596072079
{ a: 1, b: 2, x: { c: 3, d: 4 } }
10
```

### 1.5 브라우저에서 모듈 로딩 테스트

- 위의 예제의 모듈기능은 node.js 환경에서 동작한 것이고 Babel이 모듈을 트랜스파일링한 것도 node.js가 기본 지원하는 CommonJS 방식의 모듈 로딩 시스템에 따른 것이다.
- 다음은 src/js/main.js가 Babel에 의해 트랜스파일링된 결과다.

```jsx
// dist/js/main.js
"use strict";

var _lib = require("./lib");

// src/js/main.js
console.log(_lib.pi);
console.log((0, _lib.power)(_lib.pi, _lib.pi));
var f = new _lib.Foo();
console.log(f.foo());
console.log(f.bar());
```

- 브라우저는 CommonJS 방식의 require 함수를 지원하지 않으므로 위에서 트랜스파일링된 결과를 그대로 브라우저에서 실행하면 에러가 발생한다.
- 프로젝트 루트 폴더에 다음과 같이 index.html을 작성하여 트랜스파일링된 자바스크립트 파일을 브라우저에서 실행해보자.

```jsx
<!DOCTYPE html>
<html>
<body>
  <script src="dist/js/lib.js"></script>
  <script src="dist/js/main.js"></script>
</body>
</html>
```

- ReferenceError가 발생한다.
- 브라우저의 ES6모듈을 사용하도록 Babel을 설정할 수도 있으나 ESM을 사용하는 것은 문제가 있고, Webpack을 통해 문제해결이 가능하다.

## 2. Webpack

- **Webpack** → 의존 관계에 있는 자바스크립트, CSS, 이미지 등의 리소스들을 하나(또는 여러개)의 파일로 번들링하는 모듈 번들러다.
- Webpack을 사용하면 의존 모듈이 하나의 파일로 번들링되므로 별도의 모듈 로더가 필요 없다.
- 그리고 여러개의 자바스크립트 파일을 하나로 번들링하므로 HTML 파일에서 script 태그로 여러 개의 자바스크립트 파일을 로드해야 하는 번거로움이 사라진다.
- Webpack이 자바스크립트 파일을 번들링하기 전에 Babel을 로드하여 ES6+/ESNext 사양의 소스코드를 ES5 사양의 소스코드로 트랜스파일링하는 작업을 실행하도록 설정할 것이다.

### 2.1 Webpack 설치

- 터미널에서 다음과 같이 입력한다.

```bash
$ npm install --save-dev webpack webpack-cli
```

### 2.2 babel-loader 설치

- Webpack이 모듈을 번들링할 때 Babel을 사용하여 ES6+/ESNext 사양의 소스코드를 ES5 사양의 소스코드로 트랜스파일링하도록 babel-loader를 설치한다.

```bash
$ npm install --save-dev babel-loader
```

- npm scripts를 변경하여 Babel 대신 Webpack을 실행하도록 수정한다.

```json
{
  "name": "esnext-project",
  "version": "1.0.0",
  "scripts": {
    "build": "webpack -w"
  },
  "devDependencies": {
    "@babel/cli": "^7.10.3",
    "@babel/core": "^7.10.3",
    "@babel/plugin-proposal-class-properties": "^7.10.1",
    "@babel/preset-env": "^7.10.3",
    "babel-loader": "^8.1.0",
    "webpack": "^4.43.0",
    "webpack-cli": "^3.3.12"
  }
}
```

### 2.3 webpack.config.js 설정 파일 작성.

- webpack.config.js는 Webpack이 실행될 떄 참조하는 설정파일이다.
- 프로젝트 루트 폴더에 webpack.config.js 파일을 생성하고 다음과 같이 작성한다.

```jsx
const path = require('path');

module.exports = {
  // entry file
  // https://webpack.js.org/configuration/entry-context/#entry
  entry: './src/js/main.js',
  // 번들링된 js 파일의 이름(filename)과 저장될 경로(path)를 지정
  // https://webpack.js.org/configuration/output/#outputpath
  // https://webpack.js.org/configuration/output/#outputfilename
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'js/bundle.js'
  },
  // https://webpack.js.org/configuration/module
  module: {
    rules: [
      {
        test: /\.js$/,
        include: [
          path.resolve(__dirname, 'src/js')
        ],
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env'],
            plugins: ['@babel/plugin-proposal-class-properties']
          }
        }
      }
    ]
  },
  // 소스 맵(Source Map)은 디버깅을 위해 번들링된 파일과 번들링되기 이전의 소스 파일을 연결해주는 파일이다.
  devtool: 'source-map',
  // https://webpack.js.org/configuration/mode
  mode: 'development'
};
```

- Webapck을 실행하여 트랜스파일링 및 번들링을 실행해보자.
- 트랜스파일링은 Babel이 수행하고 번들링은 Webpack이 수행한다.

```bash
$ npm run build

> esnext-project@1.0.0 build /Users/leeungmo/Desktop/esnext-project
> webpack -w

webpack is watching the files…

Hash: 912e4ad621459698288f
Version: webpack 4.43.0
Time: 1263ms
Built at: 2020. 06. 27. 오후 3:45:33
        Asset      Size  Chunks                   Chunk Names
    bundle.js  8.55 KiB    main  [emitted]        main
bundle.js.map  5.09 KiB    main  [emitted] [dev]  main
Entrypoint main = bundle.js bundle.js.map
[./src/js/lib.js] 3.69 KiB {main} [built]
[./src/js/main.js] 165 bytes {main} [built]
```

- Webpack을 실행한 결과 dist/js 폴더에 bundle.js가 생성되었다.
- 이 파일은 main.js/lib.js 모듈이 하나로 번들링된 결과물이다.
- index.html을 다음과 같이 수정하고 브라우저에서 실행해보자.

```jsx
<!DOCTYPE html>
<html>
<body>
  <script src="./dist/js/bundle.js"></script>
</body>
</html>
```

- main.js , lib.js 모듈이 하나로 번들링된 bundle.js가 브라우저에서 문제없이 실행된 것을 확인할 수 있다.

![49-1](https://user-images.githubusercontent.com/76714485/148637571-5f906157-a717-4274-9335-85a07611fc3d.png)


### 2.4 babel-polyfill 설치

- @babel-polyfill 대신(deprecated) @babel/plugin-transform-runtime을 사용해 폴리필 추가해야 한다.
- Babel을 사용하여 ES6+/ESNext 사양의 소스코드를 ES5사양의 소스코드로 트랜스파일링해도 브라우저가 지원하지 않는 코드가 남아 있을 수 있다.
- ES6에서 추가된 Promise, Object.assign, Array.from등은 ES5로 트랜스파일링해도 ES5 사양에 대체할 기능이 없기 때문에 트랜스파일링되지 못하고 그대로 남는다.
- src/js/main.js코드로 Promise, Object.assign, Array.from이 어떻게 트랜스파일링되는지 보자.

```jsx
// src/js/main.js
import { pi, power, Foo } from './lib';

console.log(pi);
console.log(power(pi, pi));

const f = new Foo();
console.log(f.foo());
console.log(f.bar());

// polyfill이 필요한 코드
console.log(new Promise((resolve, reject) => {
  setTimeout(() => resolve(1), 100);
}));

// polyfill이 필요한 코드
console.log(Object.assign({}, { x: 1 }, { y: 2 }));

// polyfill이 필요한 코드
console.log(Array.from([1, 2, 3], v => v + v));
```

- 다시 트랜스 파일링과 번들링을 실행한 다음, dist/js/bundle.js를 확인해 보자.

```jsx
...
// 190 line
console.log(new Promise(function (resolve, reject) {
  setTimeout(function () {
    return resolve(1);
  }, 100);
})); // polyfill이 필요한 코드

console.log(Object.assign({}, {
  x: 1
}, {
  y: 2
})); // polyfill이 필요한 코드

console.log(Array.from([1, 2, 3], function (v) {
  return v + v;
}));
...
```

- 이처럼 ES5 사양으로 대체할 수 없는 기능은 트랜스파일링되지 않는다.
- 따라서 구형 브라우저에서 (IE) 사용하려면 @babel/plugin-transform-runtime을 설치해야한다.

```bash
npm install @babel/plugin-transform-runtime
```
