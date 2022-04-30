<h1>React Router</h1>

1. Create React App
<center>전통적 방식의 CS Web</center>

![traditionalCSWebLayer](../img/traditionalCSWebLayer.png)

<center>* url이 변경될때마다 서버에서 데이터 요청</center>
<br/>

<center>리액트 이후 Web</center>

![singlePageApplication](../img/singlePageApplication.png)

<center>* 하나의 큰 앱을 받아온 후 클라이언트 단에서 변경요청을 처리하는 방식</center> <br/>

- SPA 라우팅 과정

  1. 브라우저에서 최초에 '/' 경로로 요청을 하면
  2. React Web App을 내려줌
  3. 내려받은 React App에서 '/' 경로에 맞는 컴포넌트를 보여줌
  4. React App에서 다른 페이지로 이동하는 동작을 수행하면,
  5. 새로운 경로에 맞는 컴포넌트를 보여줌

<br/>

- React는 컴포넌트의 생성 및 render만 할 뿐,<br/>
  브라우저의 요청(url의 변경)에 따라 어떤 컴포넌트를 보여줄지는 라우팅의 역할

<br/>

- React의 범주를 넘어서는 것이므로 별도의 react router를 설치해야 함

  - 명령어 : npm i react-router-dom

    - cra 기본 내장 패키지 아님
    - react-router-dom은 Facebook의 공식 패키지 아님
    - 가장 대표적인 라우팅 패키지

  - 설치
    - 프로젝트 생성
      ```
      > npx create-react-app@latest react-router-example
      ```
    - react-router-dom 설치
      ```
      > cd react-router-example
      > npm i react-router-dom
      ```
  - 특정 경로에서 보여줄 컴포넌트 준비
    - '/' : Home 컴포넌트
    - '/profile' : Profile 컴포넌트
    - '/about' : About 컴포넌트
    - /src 하위에 pages 폴더 생성 후 Home.jsx, Profile.jsx, About.jsx 생성
  - App.js 작성 시 주의사항

    - 강의 시점과 라우터 버전이 달라져 사용 문법도 변경이 있음<br/>

    <center>21.11 이전</center>

          ```
          <App.js >

          <br/>
              <Route path="/" component={Home} />
              <Route path="/profile" component={Profile} />
              <Route path="/about" component={About} />
          </BrowserRouter>
          ```

    <center>21.11 이후</center>

          ```
          App.js

          <BrowserRouter>
            <Routes>
              <Route path="/" element={<Home />} />
              <Route path="/profile" element={<Profile />} />
              <Route path="/about" element={<About />} />
            </Routes>
          </BrowserRouter>
          ```

    - 변경된 이유는 이전 버전의 경우, "/profile"과 "/about"은 Home의 path인 "/"와 중첩되므로<br/>
      이전 버전의 경우 "/profile" 또는 "/about"으로 조회할 경우, 아래와 같이 Home페이지와 중첩 조회된다.<br/>
      그래서 이전 버전의 경우 Home 페이지의 component props에 exact 명기를 해줬는데<br/>
      현재 버전은 <Routes>의 모든 child인 <Route>의 요소를 살펴보고 가장 일치하는 항목을 찾아 UI의 해당 분기를 렌더링함

<br/>

<center>react-routerPrev</center>

![react-routerPrev](../img/react-routerPrev.png)

<br/><br/>

2. Dynamic 라우팅

<br/><br/>

3. Switch와 notFound

<br/><br/>

4. JSX 링크로 라우팅 이동하기

<br/><br/>

5. JS로 라우팅 이동하기

<br/><br/>

6. Redirect

<br/><br/>

<center>빌드 파일구조</center>

![buildFileTree](../img/buildFileTree.png)
