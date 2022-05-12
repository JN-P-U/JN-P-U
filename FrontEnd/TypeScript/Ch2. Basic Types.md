1. TypeScript Types VS JavaScript Types

- TS Types : Static Types
  - 개발 중간에 타입 체크
- JS Types : Dynamic Types

  - 실제 run 시에 타입 체크

- Type과 관련한 TypeScript의 사상

  - 프로그램이 유용하려면, 가장 간단한 데이터 단위로 작업할 수 있어야 함
  - numbers, strings, structures, boolean 등
  - TS는 JS에서 기대하는 것과 동일한 타입을 지원하며, 엺거타임(enumeration type)도 제공

- TypeScript 제공 데이터 타입
  - User가 생성한 Type은 기본 자료형으로 쪼개짐
  - JS의 기본 자료형 포함(superset)
    - ECMAScript 표준에 따른 기본 자료형 6가지(1 ~ 6)
      1. Boolean
      2. Number
      3. String
      4. Null
      5. Undefined
      6. Symbol(ECMAScript 6에 추가)
      7. Array: object형
  - TypeScript 추가 제공 Type
    1. Any, Void, Never, Unknown
    2. Enum
    3. Tuple : object형

2. Primitive Types

- 실제 값을 저장하는 자료형(오브젝트와 레퍼런스 아님)
- ECMAScript2015 기준 6가지
  1. Boolean
  2. Number
  3. String
  4. Null
  5. Undefined
  6. Symbol(ECMAScript 6에 추가)
- literal 값(값 그 자체를 문자로 값을 할당하는 것 ex. true)으로 Primitive 타입의 서브타입을 나타낼 수 있음
  1. boolean : true => true / false 중 하나인 subtype
  2. 'hello' => 전체 문자열의 subtype
- Wrapper 객체 생성 가능

  ```
  new Boolean(false); // typeof new Boolean(false) : 'object'
  new String('world'); // typeof new Boolean('world') : 'object'
  new Number(1); // typeof new Number(1) : 'object'
  ```

  - TypeScript는 이를 사용하지 말라 권고 : 즉, Primitive Type은 무조건 소문자

  ```
  잘못된 사용 예

  function reverse(s: String): String {
    return s.split("").reverse().join("");
  }

  reverse("hello world");

  정상 사용 예

  function reverse(s: string): string {
    return s.split("").reverse().join("");
  }

  reverse("hello world");
  ```

3. boolean

- 예시

  ```
  let isDone: boolean = false;

  isDone = true;

  console.log(typeof isDone); // 'boolean'

  let isOk: Boolean = true;

  let isNotOk: boolean = new Boolean(true);

  => isNotOk : 컴파일 에러

  ```

4. number

- 예시

  ```
  let decimal: number = 6; // 10진수

  let hex: number = 0xf00d; // 16진수

  let binary: number = 0b1010; // 2진수

  let octal: number = 0o744; // 8진수

  let notANumber: number = NaN;

  let underscoreNum: number = 1_000_000;

  ```

5. string

- 예시

  ```
  let myName: string = "Mark";

  myName = "Anna";

  ```

- Template String

  - 행에 걸쳐 있거나, 표현식을 넣을 수 있는 문자열
  - ``(backtice) 기호 안에 ${ expr }로 작성
  - `${ expr }`
  - 사용 예

  ```

  let fullName: string = 'Hayden Park';

  let age: number = 40;

  let sentence: string = `Hi. My name is ${fullName}.

  I'll be ${age + 1} years old next month.
  `;
  console.log(sentence);

  > npx tsc
  > node 파일명.js

  Hi. My name is Hayden Park.

  I'll be 41 years old next month.

  ```

6. symbol

- Primitive Type 값을 담아서 사용
- 고유하고 수정 불가능한 값으로 생성
- 주로 접근 제어 등에 사용
- new Symbol이 라니라 Symbol() 함수를 이용하여 symbol 타입 생성
- 예시

  ```
  console.log(Symbol("foo") === Symbol("foo")); // false

  let sym = Symbol();
  let obj = {
    [sym]: "value"
  };

  console.log(obj[sym]); // "value"

  만약, obj["sym"]과 같은 형태로 사용할 시 컴파일 에러가 발생한다.

  ```

- 문자열 등으로 직접 접근을 막고 Symbol을 통해서만 접근 가능하도록 할 떄 사용

7. null & undefined

- 예시

  ```

  ```

8. object

- 예시

  ```

  ```

9. Array

- 예시

  ```

  ```

10. Tuple

- 예시

  ```

  ```

11. any

- 예시

  ```

  ```

12. unknown

- 예시

  ```

  ```

13. never

- 예시

  ```

  ```

14. void

- 예시

  ```

  ```
