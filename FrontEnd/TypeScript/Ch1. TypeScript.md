1. TypeScript란 무엇인가?

- TypeScript란?
  - JavaScript에 Type을 추가해서 JS를 확장시킨 것
  - 코드 실행 전 컴파일 에러로 에러 잡기 편함
  - 오픈소스이며 어떤 OS나 Browser에서도 사용 가능
  - Programming Language
  - Compiled Language(Transpile Language로 부르기도 함)
    - JS는 Interpreted Language
  - Compiled VS Interpreted
    - Compiled
      1. 컴파일 필요
      2. 컴파일러 필요
      3. 컴파일 타임 필요
      4. 컴파일된 결과물 실행
      5. 컴파일된 결과물을 실행
    - 즉, 컴파일 언어는 Editor에서 작성한 코드를 실제 Browser, Node.js와 같은 실행환경에서<br/>
      실행하기 위해 Compiler가 Compile을 해야하며 그 시점이 필요함

2. TypeScript 설치 및 사용

- 자바스크립트 실행 환경 설치

  - node.js

    - Chrome's V8 JavaScript Engine을 사용하여, 자바스크립트를 해석하고 OS 레벨에서의 API를 제공하는 서버사이드용 JS 런타임환경
    - node.js는 nvm 이용해서 설치

      1. https://github.com/nvm-sh/nvm#install--update-script
      2. Install & Update Script
      3. curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
      4. ~/.zshrc에 아래 환경설정 추가 되었는지 확인
         - macOS 10.15 카탈리나부터 기본 쉘이 bash에서 zsh(Z shell)로 변경

      ```
      export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
      [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
      ```

  - browser

    - HTML을 통적으로 만들기 위해 브라우저에서 JS를 해석하고, DOM을 제어할 수 있도록 하는 JS 런타임 환경

  - typescript 컴파일러 설치

    - 전역 설치

      ```
      > npm i typescript -g
      ```

      컴파일러 설치 경로 : node_modules/.bin/tsc

      컴파일러 실행 명령어 : tsc "파일명".ts

      ```
      > tsc test.ts
      ```

      js 파일 생성 확인

      ```
      > ls

      test.ts test.js
      ```

      실행 확인

      ```
      > cat test.ts
      > cat test.js
      ```

      특정 프로젝트 안의 전체 .ts 파일을 모두 .js로 컴파일 하려면 "tsc" 명령어만 입력하면 되지만, 설정파일 생성이 필요함

      ```
      > tsc --init
      > ls
      test.ts tsconfig.json

      > tsc
      > ls
      test.js test.ts tsconfig.json
      ```

      watch모드 실행하여 ts 파일 생성 시마다 자동 컴파일

      ```
      > tsc -w
      ```

      전역 설치 삭제

      ```
      > npm uninstall typescript -g
      > rm -rf "삭제대상프로젝트"
      ```

    - 해당 프로젝트에 설치 : npm 프로젝트 생성

      ```
      > mkdir tsc-project
      > cd tsc-project
      > npm init -y

      {
        "name": "tsc-project",
        "version": "1.0.0",
        "description": "",
        "main": "index.js",
        "scripts": {
          "test": "echo \ "Error: no test specified\" && exit 1"
        },
        "keywords": [],
        "author": "",
        "license": "ISC",
      }

      > ls
      package.json

      > npm i typescript

      > cat package.json
      {
        "name": "tsc-project",
        "version": "1.0.0",
        "description": "",
        "main": "index.js",
        "scripts": {
          "test": "echo \ "Error: no test specified\" && exit 1"
        },
        "keywords": [],
        "author": "",
        "license": "ISC",
        "dependencies": {
          "typescript": "^4.6.4"
        }
      }

      ```

      typescript 설치 경로 상펴보기

      ```
      > ls
      node_modules		package-lock.json	package.json

      > ls node_modules
      typescript

      > ls -al node_module
      total 0
      drwxr-xr-x   4 jn.park  staff  128  5 11 23:09 .
      drwxr-xr-x   8 jn.park  staff  256  5 11 23:15 ..
      drwxr-xr-x   4 jn.park  staff  128  5 11 23:09 .bin
      drwxr-xr-x  13 jn.park  staff  416  5 11 23:09 typescript

      > ls -al node_module/.bin
      total 0
      drwxr-xr-x  4 jn.park  staff  128  5 11 23:09 .
      drwxr-xr-x  4 jn.park  staff  128  5 11 23:09 ..
      lrwxr-xr-x  1 jn.park  staff   21  5 11 23:09 tsc -> ../typescript/bin/tsc
      lrwxr-xr-x  1 jn.park  staff   26  5 11 23:09 tsserver -> ../typescript/bin/tsserver

      ```

      - tsc가 상위 폴더의 bin의 tsc를 가리키고 있음

      - tsc 실행

      ```
      > node_modules/.bin/tsc

      > node_modules/typescript/bin/tsc
      ```

      - 명령어가 길기때문에 현재는 npx 명령어를 이용하여 실행

      ```
      tsconfig 추가
      > npx tsc --init
      > ls
      node_modules package-lock.json package.json tsconfig.json

      test.ts 파일 생성 후 tsc 실행
      > npx tsc
      > ls
      node_modules package-lock.json package.json test.ts test.js tsconfig.json

      ```

      watch모드 실행

      ```
      > npx tsc -w
      ```

      - package.json 파일 수정하여 npm 명령어로 tsc 실행하기

      ```
      /package.json

        원본
        "scripts": {
          "test": "echo \ "Error: no test specified\" && exit 1"
        },

        변경

        "scripts": {
          "build": "tsc"
        },

        > npm run build

        아래 자동 실행
        > tsc-project@1.0.0 build
        > tsc

      ```

      watch 모드 실행

      ```
      > npm run build:watch
      ```

3. VS Code 설치 및 설정

- VS Code
  - TypeScript Compiler 포함
  - 내장된 컴파일러 버전은 VS Code가 업데이트 되면서 올라가기 때문에 컴퍼일러 버전과 VS Code 버전은 상관관계가 있음
  - 내장된 컴파일러를 선택할 수 있고, 직접 설치한 컴파일러를 선택할 수 있음

4. First Type Annotation

- typescript는 값을 할당 받으면, type을 지정함

```
let a = "s";
=> let a: string;

let b = 1;
=> let a: number;

let c;
=> let c: any;

function d(b: number) {

}

d(number만 가능);
```
