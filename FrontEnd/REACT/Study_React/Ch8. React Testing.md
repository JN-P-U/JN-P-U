<h1>Ch8. React Testing</h1>

1. Basic Hooks

- JavaScript Unit Test && Jest 사용하기

  - Unit Test

    - 사람을 믿을 것인가? 테스트 코드를 믿을 것인가?
      - 실제로는 사람이 아니라, 사람의 감
      - 코드는 거짓말을 하지 않는다
    - 통합테스트에 비해 빠르고 쉽다
    - 통합테스트를 진행하기 전에 문제를 찾아낼 수 있다.
      - 통합테스트의 성공 보장도 없다.
    - 테스트 코드가 살아있는(동작을 설명하는) 명세
      - 테스트를 읽고 어떻게 동작하는지도 예측 가능
    - 소프트웨어 장인이 되려면 TDD는 필수
      - 선 코딩 후, (몰아서) 단위테스트 하는 방식은 안됨.

  - Jest
    - Facebook의 제공 라이브러리
    - Easy Setup
    - Instant Feedback
      - 고친 파일만 빠르게 재테스트 가능
    - Snapshot Testing
    - 스냅샷 : 컴포넌트 테스트에 중요

.test.js, .spec.js, `/__tests/.js`는 테스팅 파일로 인식

- React Component Test

- testing-library/react 활용하기
  - 시나리오 : Button 컴포넌트 만들기
    1. 컴포넌트가 정상적으로 생성된다.
    2. "button"이라고 쓰여있는 엘리먼트는 HTMLButtonElement이다.
    3. 버튼을 클릭하면, p태그 안네 "버튼이 방금 눌렸다."라고 쓰여진다.
    4. 버튼을 클릭하기 전에는, p태그 안에 "버튼이 눌리지 않았다."라고 쓰여진다.
    5. 버튼을 클릭하고 5초 뒤에는, p 태그 안에 "버튼이 눌리지 않았다."라고 쓰여진다.
    6. 버튼을 클릭하면, 5초 동안 버튼이 비활성화 된다.
