1. 작성자와 사용자의 관점으로 코드 바라보기

- 타입 시스템 종류

  - 컴파일러에게 사용하는 타입을 명시적으로 지정하는 시스템
  - 컴파일러가 자동으로 타입을 추론하는 시스템

- 타입스크립트의 타입 시스템

  - 타입을 명시적으로 지정 가능
  - 타입을 명시적으로 지정하지 ㅇ낳으면, 타입스크립트 컴파일러가 자동으로 타입을 추론

- 함수의 사용자와 구현자

  - 사용자 : 자신의 코드에서 해당 함수를 사용
  - 구현자 : 해당 함수를 구현

- 타입이란 해당 변수가 할 수 있는 일을 결정

```
// JavaScript

// f1 이라는 함수의 body 에서는 a 를 사용할 것
// a 가 할 수 있는 일은 a 의 타입이 결정

function f1(a) {
  return a;
}
```

- 함수 사용법에 대한 오해를 야기하는 JS
  - f2()의 파라미터로 문자열을 넣으면 안되지만, JS에서는 필수 타입을 코딩 시점에 알려주지 않으므로 이를 알 수 없음

```
// JavaScript

// (f2 실행의 결과가 NaN을 의도한 것이 아니라면)
// 이 함수의 작성자는 매개변수 a가 number 타입이라는 가정으로 함수를 작성함

function f2(a) {
  return a * 38;
}

// 사용자는 사용법을 숙지하지 않은 채, 문자열을 사용하여 함수를 실행함

console.log(f2(10)); // 380
console.log(f2('Mark')); // NaN
```

- 타입스크립트의 추론에 의지하는 경우
  - 타입스크립트 코드지만,
  - a의 타입을 명시적으로 지정하지 않은 경우이므로 a는 any로 추론
  - 함수의 리턴 타입음 number로 추론됨(NaN도 number의 하나)
  - 사용자는 a가 any이므로, 사용법에 맞게 문자열을 사용하여 함수를 실행했다고 볼 수 있으나
  - 구현자는 f3()의 리턴이 NaN이 나오도록 의도한게 아니라면 f3()는 올바르지 않다고 볼 수 있음
  - 즉, 구현자는 f3()의 올바른 사용법을 사용자에게 제대로 전달하지 못한 것

```
function f3(a) {
  return a * 38;
}

console.log(f3(10)); // 380
console.log(f3('Mark')); // NaN
```

- noImplicitAny 옵션 설정

  - 위와 같은 문제를 피하기 위해 tsconfig.json에서 noImplicitAny 옵션 설정을 켤 수 있음
  - 타입을 명시적으로 지정하지 않은 경우,
  - 타입스크립트가 추론 중론 'any'라고 판단하게 되면,
  - 컴파일 에러를 발생시켜 명시적으로 타입을 지정하도록 유도함

- noImplicitAny에 의한 방어
  - 아래 코드와 같이 f3()의 파라미터 a가 any 타입으로 추론되는 경우 다음의 컴파일 에러 발생
  - error TS7006: Parameter 'a' implicitly has an 'any' type.
  - 사용자의 코드를 실행할 수 없음
  - 컴파일이 정상적으로 마무리 될 수 있도록 수정 필요

```
function f3(a) {
  return a * 38;
}

// 사용자의 코드를 실행할 수 없습니다. 컴파일이 정상적으로 마무리 될 수 있도록 수정해야 합니다.

console.log(f3(10));
console.log(f3('Mark') + 5);
```

- number 타입으로 추론된 리턴 타입
  - 매개변수의 타입은 명시적으로 number 지정
  - 명시적으로 지정하지 않은 함수의 리텀 타입은 number로 추론
  - 사용자는 사용법에 맞게 숫자형을 사용하여 함수를 실행함
  - 해당 함수의 리턴 타입은 number이기 때문에, 타입에 따르면 이어진 연산이 가능
  - 하지만 실제 undefined + 5가 실행되어 NaN이 출력됨
  - 이는 타입스크립트가 primitive 타입에 기본적으로 undefined가 subtype으로 존재하기 때문

```
function f4(a: number) {
  if (a > 0) {
    return a * 38;

  }
}

console.log(f4(5)); // 190
console.log(f4(-5) + 5); // NaN
```

- stringNullChecks 옵션 설정

  - 위와 같은 문제를 피하기 위해 tsconfig.json에서 stringNullChecks 옵션 설정 가능함
  - 옵션 설정 시 모든 타입에 subtype으로 포함되어 있는 null과 undefined를 제거함

- number | undefined 타입으로 추론된 리턴 타입
  - 사용자는 사용법에 맞게 숫자형을 사용하여 함수를 실행
  - 해당 함수의 리턴 타입은 number | undefined 이기 때문에,
  - 타입에 따르면 이어진 연산을 바로 할 수 없음
  - 컴파일 에러를 고쳐야하기 하기 때문에 사용자와 작성자가 의논 후 수정해야함
  - 아래와 같은 경우, 다음과 같은 컴파일 에러 발생
    - error TS2532: Object is possibly 'undefined'.

```
function f4(a: number) {
  if (a > 0) {
    return a * 38;
  }
}

console.log(f4(5));
console.log(f4(-5) + 5);
```

- 명시적으로 리턴 타입을 지정 할 경우
  - 매개변수의 타입과 함수의 리턴 타입을 명시적으로 지정
  - 실제 함수 구현부의 리턴 타입과 명시적으로 지정한 타입이 일치하지 않아 컴파일 에러가 발생
  - 컴파일 에러
    - error TS2366: Function lacks ending return statement and return type does not include 'undefined'.

```
function f5(a: number): number {
  if (a > 0) {
    return a * 38;
  }
}
```

- noImplicitReturns 옵션 설정 시
  - 아래 코드에서 if 조건이 true가 아닌 경우 return을 직접 하지 않고 코드가 종료되므로
  - 함수 내에서 모든 코드가 값을 리턴하지 않으면, 컴파일 에러 발생
    - error TS7030: Not all code paths return a value.

```
function f5(a: number) {
  if (a > 0) {
    return a * 38;
  }
}
```

- 매개변수에 object가 들어오는 경우
  - JS 기준으로 아래 코드와 같이 f6()의 파라미터로 어떤 타입이든 들어올 수 있으므로
  - f6('Mark')와 같이 이름은 undefined이고, 연령대는 NaN대입니다. 와 같은 잘못된 결과 발생 가능

```
// JavaScript

function f6(a) {
  return `이름은 ${a.name} 이고, 연령대는 ${
    Math.floor(a.age / 10) * 10
  }대 입니다.`;
}

console.log(f6({ name: 'Mark', age: 38 })); // 이름은 Mark 이고, 연령대는 30대 입니다.
console.log(f6('Mark')); // 이름은 undefined 이고, 연령대는 NaN대 입니다.
```

- object literal type
  - TypeScript에서 아래 코드와 같이 object literal로 f7()의 파라미터 a의 타입을 지정했으며, 리턴 타입도 string으로 지정함
    - 인풋의 타입 { name: string; age: number }
  - f7()을 사용한 두번째 케이스의 경우, 아래와 같이 컴파일 에러 발생
  - 그러나 object liter을 모두 기재하면 매우 불편하기때문에 나만의 타입을 만드는 방법을 사용함

```
function f7(a: { name: string; age: number }): string {
  return `이름은 ${a.name} 이고, 연령대는 ${
    Math.floor(a.age / 10) * 10
  }대 입니다.`;
}

console.log(f7({ name: 'Mark', age: 38 })); // 이름은 Mark 이고, 연령대는 30대 입니다.
console.log(f7('Mark')); // error TS2345: Argument of type 'string' is not assignable to parameter of type '{ name: string; age: number; }'.
```

- 나만의 타입을 만드는 방법

  - interface

  ```
  interface PersonInterface {
    name: string;
    age: number;
  }

  function f8(a: PersonInterface): string {
    return `이름은 ${a.name} 이고, 연령대는 ${
      Math.floor(a.age / 10) * 10
    }대 입니다.`;
  }

  console.log(f8({ name: 'Mark', age: 38 })); // 이름은 Mark 이고, 연령대는 30대 입니다.
  console.log(f8('Mark')); // error TS2345: Argument of type 'string' is not assignable to parameter of type 'PersonInterface'.
  ```

  - Type Alias

  ```
  type PersonTypeAlias = {
    name: string;
    age: number;
  }

  function f8(a: PersonInterface): string {
    return `이름은 ${a.name} 이고, 연령대는 ${
      Math.floor(a.age / 10) * 10
    }대 입니다.`;
  }

  console.log(f8({ name: 'Mark', age: 38 })); // 이름은 Mark 이고, 연령대는 30대 입니다.
  console.log(f8('Mark')); // error TS2345: Argument of type 'string' is not assignable to parameter of type 'PersonTypeAlias'.
  ```

2. Structural Type System VS Nominal Type System

- Structural Type System
  - 구조가 같으면, 같은 타입
  - TypeScript는 Structural Type System을 따름

```
interface IPerson {
  name: string;
  age: number;
  speak(): string;
}

type PersonType = {
  name: string;
  age: number;
  speak(): string;
}

let personInterface: IPerson = () as any;
let personType: PersonOType = {} as any;

personInterface = personType;
personType = personInterface;
```

- Nominal Type System
  - 구조가 같아도 이름이 다르면, 다른 타입
  - Java, C 등

```
type PersonID = string & { readonly brand: unique symbol };

function PersonID(id: string): PersonID {
  return id as PersonID;
}

function getPersonById(id: PersonID) {}

getPersonById(PersonID('id-aaaaaa'));
getPersonById('id-aaaaaa'); // error TS2345: Argument of type 'string' is not assignable to parameter of type 'PersonID'. Type 'string' is not assignable to type '{ readonly brand: unique symbol; }'.
```

- Duck Typing
  - 런타임에 발생하는 타이핑 처리 방식
  - 만약 어떤 새가 오리처럼 걷고, 헤엄치고, 꽥꽥거리는 소리를 낸다면 나는 그 새를 오리라고 부를 것이다.
  - Python

```
class Duck:
   def sound(self):
      print u"꽥꽥"

class Dog:
   def sound(self):
      print u"멍멍"

def get_sound(animal):
   animal.sound()

def main():
   bird = Duck()
   dog = Dog()
   get_sound(bird)
   get_sound(dog)
```

3. 타입 호환성(Type Compatibility)

- 서브타입 예시1

  - sub1 : literal type으로 숫자 '1'을 타입으로 지정
    - 값은 무조건 1만 가능
  - sup1 : number 타입 지정
  - sub1 타입은 sup1 타입의 서브 타입
  - sub1에 sup1 할당 시 컴파일 에러 발생
    - error! Type 'number' is not assignable to type '1'.

  ```
  let sub1: 1 = 1;
  let sup1: number = sub1;
  sub1 = sup1; // error! Type 'number' is not assignable to type '1'.
  ```

- 서브타입 예시2

  - sub2 : number[] 타입
  - sup2 : object 타입
  - array도 object이므로 sub2 타입은 sup2 타입의 서브 타입
  - sub2에 sup2 할당 시 컴파일 에러 발생
    - error! Type '{}' is missing the following properties from type 'number[]': length, pop, push, concat, and 16 more.

  ```
  let sub2: number[] = [1];
  let sup2: object = sub2;
  sub2 = sup2; // error! Type '{}' is missing the following properties from type 'number[]': length, pop, push, concat, and 16 more.
  ```

- 서브타입 예시3

  - sub3 : tuple 타입
  - sup3 : number[] 타입(array)
  - sub3의 tuple 타입이 [number, number]이므로 이는 number[]의 서브 타입
  - sub3에 sup3 할당 시 컴파일 에러 발생
    - error! Type 'number[]' is not assignable to type '[number, number]'. Target requires 2 element(s) but source may have fewer.

  ```
  // sub3 타입은 sup3 타입의 서브 타입이다.
  let sub3: [number, number] = [1, 2];
  let sup3: number[] = sub3;
  sub3 = sup3; // error! Type 'number[]' is not assignable to type '[number, number]'. Target requires 2 element(s) but source may have fewer.
  ```

- 서브타입 예시4

  - sub4 : number 타입
  - sup4 : any 타입
  - any에는 어떤 타입이든 할당할 수 있으므로 sub4에 sub4를 할당하는 것은 물론
  - sub4에도 sub4를 할당할 수 있음

  ```
  let sub4: number = 1;
  let sup4: any = sub4;
  sub4 = sup4;
  ```

- 서브타입 예시5

  - sub5 : never 타입
  - sup5 : number 타입
  - sub5의 never 타입은 number 타입의 서브 타입
  - sub5에 sup5 할당 시 컴파일 에러 발생
    - error! Type 'number' is not assignable to type 'never'.

  ```
  let sub5: never = 0 as never;
  let sup5: number = sub5;
  sub5 = sup5; // error! Type 'number' is not assignable to type 'never'.
  ```

- 서브타입 예시6

  - sub6 : Dog 타입
  - sup6 : Animal 타입
  - sub6의 Dog 타입은 Animal 타입의 서브 타입
  - sub6에 sup6 할당 시 컴파일 에러 발생
    - error! Property 'eat' is missing in type 'SubAnimal' but required in type 'SubDog'.

  ```
  class Animal {}
  class Dog extends Animal {
    eat() {}
  }

  let sub6: Dog = new Dog();
  let sup6: Animal = sub6;
  sub6 = sup6; // error! Property 'eat' is missing in type 'SubAnimal' but required in type 'SubDog'.
  ```

- 타입 호환성 규칙

  - 타입스크립트는 기본적으로 공변 규칙을 따른다.
  - 그러나 예외적으로 함수의 경우 반병 규칙을 따르는 경우가 존재한다.

  - 규칙 1. 공변 : 같거나 서브 타입인 경우, 할당이 가능

    - primitive type

    ```
    let sub7: string = '';
    let sup7: string | number = sub7;
    ```

    - object
      - 각각의 프로퍼티가 대응하는 프로퍼티와 같거나 서브타입이어야 함

    ```
    let sub8: { a: string; b: number } = { a: '', b: 1 };
    let sup8: { a: string | number; b: number } = sub8;
    ```

    - array
      - object 와 마찬가지

    ```
    let sub9: Array<{ a: string; b: number }> = [{ a: '', b: 1 }];
    let sup9: Array<{ a: string | number; b: number }> = sub8;
    ```

  - 규칙 2. 반병 : 함수의 매개변수 타입만 같거나 슈퍼타입인 경우, 할당이 가능

    1. 인풋 : Developer 타입 -> 리턴 : Developer 타입
    2. 인풋 : Person(슈퍼타입) 타입 -> 리턴 : Developer 타입
    3. 인풋 : StartupDeveloper(서브타입) 타입 -> 리턴 : Developer 타입

    - 반병 규칙 존재 이유
      - 공병 규칙 상, 서브타입에 슈퍼타입을 할당할 시 컴파일 에러가 발생하고 슈퍼타입에 서브타입을 할당할 경우 컴파일 에러가 발생하지 않지만
      - 함수의 매개변수 클래스를 사용할 경우 이와는 반대 규칙이 적용됨(반병)
      - 그 이유는 상속 개념의 특성에 대한 문제 때문인데
      - 상속을 받을 경우, 슈퍼타입(부모클래스)에서 생성한 함수를 서브타입(자식클래스)에서는 상속받아 사용 가능하며, 그 외에 서브타입(자식클래스)에서
      - 생성한 함수 또한 사용 가능하므로 슈퍼타입(부모클래스)에서는 서브타입(자식클래스)의 함수에 접근이 불가능함
    - 아래 코드의 경우,

      - tellme()의 매개변수로 함수를 사용하면서 sToD()의 매개변수로 최하위 서브타입인 StartupDeveloper 클래스를 매개변수로 사용하여
      - Developer 클래스를 리턴할 경우, Developer에는 StartupDeveloper에서 생성한 burning()가 없기때문에 논리적으로 맞지 않음

    - TypeScript는 기본적으로 3번의 경우 컴파일 에러를 발생시키지 않지만 tsconfig.json에서 설정을 통해 제한 가능함
      - 옵션 : strictFunctionTypes
      - 함수를 할당할 시에 함수의 매개변수 타입이 같거나 슈퍼타입인 경우가 아닌 경우, 에러를 통해 경고

    ```
    class Person {}
    class Developer extends Person {
      coding() {}
    }
    class StartupDeveloper extends Developer {
      burning() {}
    }

    function tellme(f: (d: Developer) => Developer) {}

    // Developer => Developer 에다가 Developer => Developer 를 할당하는 경우
    tellme(function dToD(d: Developer): Developer {
      return new Developer();
    });

    // Developer => Developer 에다가 Person => Developer 를 할당하는 경우
    tellme(function pToD(d: Person): Developer {
      return new Developer();
    });

    // Developer => Developer 에다가 StartipDeveloper => Developer 를 할당하는 경우
    tellme(function sToD(d: StartupDeveloper): Developer {
      return new Developer();
    });
    ```

4. 타입 별칭(Type Alias)

- Interface와 유사함
- Primitive, Union Type, Tuple, Function 및 기타 직접 작성해야 하는 타입을 다른 이름으로 지정 가능
- 만들어진 타입의 refer로 사용하는 것이지 실제 타입을 만드는 것은 아님

- 사용 예

  - Aliasing Primitive(string)

  ```
  type MyStringType = string;
  const str = 'world';
  let myStr: MyStringType = 'hello';
  mystr = str;
  ```

  - Aliasing Union Type

  ```
  let person: string | number = 0;
  person = 'Mark';

  type StringOrNumber = string | number;

  let another: StringOrNumber = 0;
  another = 'Anna';

  ```

  - Aliasing Tuple

  ```
  let person: [string, number] = ['Mark', 35];

  type PersonTuple = [string, number];

  let another: PersonTuple = ['Anna', 24];

  ```

  - Aliasing Function

  ```
  type EatType = (food: string) => void;
  ```

- Interface VS TypeAlias
  - Interface : 어떤 타입이 타입으로서 목적이나 존재가치가 명확할 떄
  - TypeAlias : 그냥 대상을 가리킬 뿐이거나 별칭으로서만 존재할 때 사용
