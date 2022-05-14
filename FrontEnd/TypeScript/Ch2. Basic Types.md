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

- TypeScript에서, undefined와 null은 각각 undefined와 null이라는 타입을 가짐
- type과 값 모두 소문자이며
- 값은 각각 undefined와 null만 가짐
- tsconfig.ts의 설정을 하지 않으면, undefined와 null은 다른 모든 타입의 서브타입으로 존재함

  - 즉, number type에 null 또는 undefined를 할당할 수 있다는 의미
  - tsconfig.ts의 `--stringNullCheks` 사용 시, null과 undefined는 void나 자기 자신에게만 할당 가능
  - null과 undefined를 할당하려면, union type을 이용해야 함

- 예시

  ```
  컴파일 에러

  // let MyName: string = undefined;

  // let u: undefined = null;

  void에 할당(undefined만 할당 가능함)
  let v: void = undefined;

  union type 사용(string | null 부분)
  let union: string | null = null;

  union = 'Mark';


  ```

- JS에서 null

  - null이라는 값으로 할당된 것
  - 무언가 있지만, 사용할 준비는 덜 된 상태
  - null type은 null 값만 가질 수 있음
  - 런타임 시 typeof 연산자를 이용하여 확인 시, object

  ```
  let n: null = null;

  console.log(n); // null

  console.log(typeof n); // object
  ```

- JS에서 undefined

  - 값을 할당하지 않은 변수
  - 변수를 선언만 하고 값을 할당하지 않은 상태
  - object의 property가 없을 때
  - 런타임에서 typeof 연산자를 이용하여 확인 시, undefined

  ```
  let n: undefined = undefined;

  console.log(n); // undefined

  console.log(typeof n); // undefined
  ```

8. object

- 일반적인 object와는 조금 다르게 사용됨
- Object라는 전역 내장 객체가 존재하며, 이를 통해 object 객체를 생성할 수 있음
- primitive 타입이 아닌 것

- 예시

  ```
  // create by object literal

  // person1 is not "object" type.
  // person1 is "{name: string, age: number}" type.

  const person1 = {name: 'Mark', age: 39};

  // 위 코드의 person1의 type은 object literal type
  //  {
  //    name: string;
  //    age: number;
  //  }
  // 위와 같은 모양이 object literal

  // Object.create로 객체 생성
  // Object : 런타임에 제공되는 전역 내장객체
  // create 함수 안의 object literal을 통해서 객체를 만들어냄
  // create 함수의 인자는 object | null의 union type임
  const person2 = Object.create({name: 'Mark', age: 39});
  ```

9. Array

- object의 일종
- 배열 안의 요소는 모두 같은 type임
- 사용 방법

  - Array<타입>
  - 타입[]

- 데이터의 순서별 type과 index가 정해져 있을 경우, Tuple 타입을 사용해야함

- 예시

  ```
  let list: (number | string)[] = [1, 2, 3, '4']; // 이 방식을 권유
  // list의 모든 인덱스는 number 또는 string이 들어갈 수 있음

  let list2: Array<number> = [1, 2, 3]; // jsx, tsx에서 충돌날 수 있어서 자제

  ```

10. Tuple

- 각 인덱스별 type 지정

- 예시

  ```
  let x: [string, number];

  x = ['hello', 39]; // 순서, 타입, 길이가 맞아야 함

  // x = [39, 'hello']; 컴파일 에러 : 순서, 타입, 길이가 맞아야 함

  // x[2] = 'workd'; 컴파일 에러 : 인덱스 1까지만 정의되어있으므로 2 이후는 undefined

  const person: [string, number] = ['Mark', 39];

  const [first, second] = person;
  // const [first2, second2, third2] = person; 컴파일 에러 : 인덱스 1까지만 정의되어있으므로 2 이후는 undefined

  ```

11. any

- 어떤 타입이어도 상관없는 타입
- 최대한 사용하지 말 것
  - 컴파일 타입에 타입 체크가 정상적으로 이뤄지지 않기 때문
- 컴파일 옵션 중 any를 명기하지 않을 경우 컴파일 에러 뱉도록 하는 옵션 존재
  - noImplicitAny
- any 타입은 객체를 통해 전파됨
- any 사용 시 타입 안전성을 잃게됨
- 타입 안전성은 TypeScript를 사용하는 주요 동기 중 하나이므로 필요하지 않은 경우 외에는 any를 사용하지 말 것
  - 로그로 message를 출력할 경우, 어떤 타입이든 가능하다 정도로 사용할때만 사용할 것

<br/>

- 예시

  ```
  function returnAny(message: any): any {
    console.log(message);
  }

  const any1 = returnAny('리턴은 아무거나');

  any1.toString();

  let looselyTyped: any = {};

  // any가 개체를 통해 전파되는 예(아래 .a.b.c.d 부분)
  const d = looselyTyped.a.b.c.d;

  function leakingAny(obj: any) {
    const a = obj.num;
    const b = a + 1;
    return b;
  }

  const c = leakingAny({ num: 0 });

  c.indexOf('0');

  ```

- 위 코드에서 변수 "c"는 number type이여야 하는데 any임
  - 누수를 막기 위해서 num이 들어올 경우 다음과 같은 방식으로 사용해야함
  - const a: number = obj.num;

12. unknown

- any가 가진 타입의 불안전성을 대체하기 위해 사용
  - any보다 type-safe함
- 프로그램 작성 시 모르는 변수의 타입 묘사를 위해 사용
  - 기존에는 any 사용
- 컴파일러 및 타인에게 이 변수가 어떤 것이든 될 수 있음을 알려줄 시 unknown 타입 사용
- unknown 타입의 값을 다른 타입을 가진 변수에 직접 할당할 수 없음
  - 아래 예시 처럼 타입 가드를 사용하여 할당해야 함
  - 그래서 any보다 타입 안전성이 높은 것(runtime error 감소 가능)

<br/>

- 예시

  ```
  declare const maybe: unknown;

  if (typeof maybe === 'number') {
    const aNumber: number = maybe;
  }

  if (maybe === true) {
    const aBoolean: boolean = maybe;

    // const aString: string = maybe; // maybe가 boolean이므로 string 타입 변수에 할당 불가
  }

  if (typeof maybe === 'string') {
    const aString: string = maybe;

    // const aBoolean: boolean = maybe; // maybe가 string이므로 boolean 타입 변수에 할당 불가
  }

  ```

13. never

- 일반적으로 return에서 사용
- return에서 사용하므로 함수에서 사용
- 어떤 것도 리턴되지 않을 때 함수에서 never type 사용
  - error에서 throw 던질 떄
  - 무한 루프일 떄 등
- 모든 타입의 subtype이므로 모든 타입에 할당 가능
- never에는 다른 type은 할당 불가
  - any 타입도 할당 불가
- 잘못된 타입을 넣는 실수를 막기 위해서도 사용

- 예시

  ```
  function error(message: string): never {
    throw new Error(message);
  }

  function fail() {
    return error('failed');
  }

  function infiniteLoop(): never {
    while (true) {}
  }

  // string type을 가진 변수 a에서 typeof 타입가드를 이용하여 string type을 제했으므로 type이 never가 됨
  let a: string = 'hello';

  if (typeof a !== 'string') {
    a;
  }

  declare const b: string | number;

  if (typeof b !== 'string') {
    b;
  }

  // 조건부 타입 지정 시 never type 활용
  type Indexable<T> = T extends string ? T & { [index: string]: any } : never;

  type ObjectIndexable = Indexable<{}>;

  // const c: Indexable<{}> = '';

  ```

14. void

- 어떤 타입도 가지지 않은 빈 상태
- JS 이외에서는 리턴이 없는 void를 많이 사용하지만 JS는 undefined가 있으므로 원래 필요가 없는 타입
  - 다른 언어에서 사용하므로 의미상 명확화를 위해 도입
- void 함수의 리턴
  - return 미작성
  - return;
  - return undefined;

<br/>

- 예시

  ```
  function returnVoid(message: string) {
    console.log(message);

    return undefined;
  }

  returnVoid('리턴이 없다.');

  ```
