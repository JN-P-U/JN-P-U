1. 현재 폴더를 npm 프로젝트로 만들기 : 터미널 > npm init -y

2. 현재 폴더를 루트로 파일서버 만들기(웹서버 형식) : 터미널 > npx serve

3. React.createElement(타입)
  React.createElement(
    type, // 태그 이름 문자열 | 리액트 컴포넌트 | React.Fragment
    [props], // 리액트 컴포넌트에 넣어주는 데이터 객체
    [...children] // 자식으로 넣어주는 요소들
  );

4. babel
  우리가 작성한 어떤 코드 => 순수하게 실행할 수 있는 자바스크립트
  위 문장의 => 부분을 해주는 것이 babel
  JSX 문법으로 작성된 코드는 순수한 JavaScript로 컴퍼일하여 사용한다.
  그걸 해주는 것이 babel
  즉, 원래는 render 함수 내에 createElement를 사용해야하는 것을 직관적인 문법으로 사용할 수 있게 해준다.
  ReactDOM.render(
    React.createElement(
      "div", 
      null, 
      React.createElement("h1", null, "주제")
    ),
    document.querySelector("#root")
  )
  위와 같은 형태를 아래와 같은 형태로 사용할 수 있게 해준다
  ReactDOM.render(
    <div a="a">
      <h1>주제</h1>
    </div>,
    document.querySelector("#root")
  );


5. JSX의 형태
  ReactDOM.render(
    <div a="a">
      <h1>주제</h1>
    </div>,
    document.querySelector("#root")
  );

  위에서 <div> 의 children은 <h1>
  <div>의 a="a" 가 Props(전달 객체) 
  그 형태가 {a: "a"}로 <div>와 그 자손인 <h1>까지 전달된다.

6. 왜 JSX를 사용하는가?
  React.createElement VS JSX
    가독성이 JSX의 완승임(위의 사례에서 볼 수 있음)
  babel과 같은 컴파일 과정에서 문법적 오류를 인지하기 쉬움

7. JSX 문법
  최상위 요소가 하나여야 함
    그래서 간혹 보면 어떤 속성도 없이 <></>같은 Fragment를 사용하곤 한다
  최상위 요소를 리턴하는 경우, ()로 감싸야 함
  자식들을 바로 렌더링하고 싶으면, <>자식들</>를 사용
    => Fragment
  자바스크립트 표현식을 사용하려면, {표현식}을 이용
    const title = "주제!!!";
    <h1>{title}</h1>
    => <h1>주제!!!</h1>
  if문 사용 불가
    삼항 연산자 혹은 &&를 사용
  style을 이용해 인라인 스타일링이 가능
    Props 사용
  class 대신 className을 사용해 class를 적용
  자식요소가 있으면, 꼭 닫아야 하고, 자식요소가 없으면 열면서 닫아야 함
    <p>어쩌구</p>
    <br />

8. Props와 State
  Props : 컴포넌트 외부에서 컴포넌트에게 주는 데이터
  State : 컴포넌트 내부에서 변경할 수 있는 데이터
  둘 다 변경이 발생하면, 렌더가 다시 일어날 수 있음