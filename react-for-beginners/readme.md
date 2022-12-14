# nomadcoder_ReactJS로 영화 웹 서비스 만들기

#### #2. THE BASICS OF REACT

##### # 2.3

- ReactJS는 더 편하게 Interactive한 웹 사이트를 만들게 해줌

- ReactJS는 interactivity의 원동력, ReactDOM은 React element들을 가져다가 HTML로 바꾸게 해줌

- addEventListener를 반복할 필요 없이, createElement **속성 값**으로 이벤트 발생 시 실행할 함수를 넘겨줄 수 있음 (<mark>property에서 event를 등록할 수 있음</mark>, createElement의 arguments는 여러 개고 그 중 두번째 argument로 넘겨줌(객체 형태))

- ReactJS는 똑똑해서 속성 값으로 event를 넘겨주면(on으로 시작하는) 그건 html에 띄우지 않음. 하지만 만약 속성 값으로 id: "title"을 넘겨주면 html 태그의 속성에 띄워짐.

**ex)**

```javascript
  <body>
    <div id="root"></div>
  </body>
  <script src="https://unpkg.com/react@17.0.2/umd/react.production.min.js"></script>
  <script src="https://unpkg.com/react-dom@17.0.2/umd/react-dom.production.min.js"></script>
  <script>
    const root = document.getElementById("root");
    const h3 = React.createElement("h3", {
      onMouseEnter: () => {
        console.log("mouse enter")
      },
    }, "Hello I'm h3");
    const btn = React.createElement("button", {
      onClick : () => {
        console.log("I'm clicked")
      },
    }, 
    "Click me");
    const container = React.createElement("div", null, [h3, btn]);
    ReactDOM.render(container, root);
  </script>ainer, root);
```

- body에 비어있는 div를 생성(id="root"), 이 비어있는 div는 ReactDOM이 React element들을 가져다 놓을 곳

- 그러나, 이건 기본적인 ReactJs의 동작을 설명하기 위한 것이고 다음 강의부터는 더 쉽고 보기 좋게 element를 생성하는 방식을 배움

##### # 2.5 JSX

- 이번 강의에서는 createElement를 대체하는 방법을 배움

- JSX => JavaScript를 확장한 문법

- 브라우저가 JSX를 완전히 이해할 수 있게 뭔가를 설치해줘야 함, **BABEL** 이용(브라우저가 이해할 수 있게 코드 변환 시켜줌, 일종의 transformer)

- 우리가 코드를 작성해서 Babel에 넘겨주면 Babel은 우리가 위에서 적은 createElement 등 브라우저가 이해할 수 있는 reactJS 코드로 넘겨서 head내의 script에 담아둠

```javascript
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <script type="text/babel">
    const root = document.getElementById("root");
    const Title = (
      <h3 id="title" onMouseEnter={() =>
        console.log("mouse entered")
      }>
      Hello I'm a title
      </h3>);

    const Button = <button style={{
        backgroundColor: "tomato",
      }} onClick={() => console.log("I'm clicked")}>Click me</button>

    const container = React.createElement("div", null, [Title, Button]);
    ReactDOM.render(container, root);
  </script>
```

##### # 2.6 JSX part 2

- 어떻게 하면 컴포넌트를 다른 컴포넌트 안에 넣는가

- <mark>컴포넌트의 첫글자는 반드시 대문자여야 함</mark> (만약 소문자면 React랑 JSX는 이게 평범한 html 태그라고 생각함)
  
  <img title="" src="file:///C:/Users/SSAFY_hojung/AppData/Roaming/marktext/images/2022-12-06-22-13-54-image.png" alt="" width="356">

- 우리가 직접 만든 컴포넌트를 렌더링해서 다른 곳에서 사용할 때는 항상 대문자로 시작해야 함 = 우리가 직접 만든 요소는 다 첫글자 대문자로 적어줘야함 ! *

- Arrow function으로 할 때는 const name = () => () 소괄호가 return을 전제로 함

- Arrow function 사용하지 않으면 function name() {return }

- return 하지 않을 땐 그냥 function name() { 할 일 }

```javascript
  <script type="text/babel">
    const root = document.getElementById("root");
    const Title = () => (
      <h3 id="title" onMouseEnter={() =>
        console.log("mouse entered")
      }>
      Hello I'm a title
      </h3>);

    const Button = () => (
      <button style={{
        backgroundColor: "tomato",
      }} onClick={() => console.log("I'm clicked")}>Click me</button>
    )

    const Container = () => (
      <div>
        <Title />
        <Button />
      </div>
    );

    ReactDOM.render(<Container />, root);
  </script>
```

- ReactJS는 이전에 렌더링 된 컴포넌트를 알고, 앞으로 렌더링 될 컴포넌트를 알아서 <mark>**전체 컴포넌트를 재생성하지 않고 바뀌는 부분만 바꿔줌**</mark> (개발자 도구로 html 코드 확인해보면 알 수 있음)

##### # 3 State

```javascript
  <script type="text/babel">
    const root = document.getElementById("root");
    function App() {
      const [counter, modifier] = React.useState(0);
      // console.log(data);
      return (
      <div>
        <h3>Total clicks: {counter}</h3>
        <button>Click me</button>
      </div>
      )
    };

    ReactDOM.render(<App />, root);
  </script>
```

- console.log(data) 의 결과는 [0, f] 여기서 0은 우리가 담으려는 data 값, f는 data를 바꿀 수 있는 함수임

- const [counter, modifier] = React.useState(0); 

```javascript
  <script type="text/babel">
    const root = document.getElementById("root");

    function App() {
      const [counter, setCounter] = React.useState(0);
      function countUp () {
        setCounter(counter + 1)
      };
      return (
        <div>
        <h3>Total clicks: {counter}</h3>
        <button onClick={countUp}>Click me</button>
      </div>
      )
    };

    ReactDOM.render(<App />, root);
  </script>
```

- 여기서 setCounter가 state 값이 바뀌면 자동으로 재렌더링을 해주지만, 컴포넌트가 리렌더링 될때 2번째 이벤트리스너가 등록되거나 두번째 Total clicks 전체를 다시 만드는게 아니라 알아서 바뀌는 부분만 바꿔줌

- state가 바뀌면 리렌더링이 일어남

- <mark>setCounter에서 만약 현재값을 가지고 계산해야 한다면</mark>, setCounter에는 <mark>**함수**</mark>를 넣을 수도 있음, 이 함수의 첫번째 argument는 현재 값임. 그리고 이 함수의 return값이 new state가 되는 것, 그럼 **이 함수가 뭘 return하던지 그 return 값이 새로운 state**가 됨, 이 방식이 더 안전함(react가 이 current가 확실히 현재 값이라는 걸 보장, 우리가 원하는 값으로 정확히 계산할 수 있도록)
  
  ![](C:\Users\SSAFY_hojung\AppData\Roaming\marktext\images\2022-12-10-20-30-29-image.png)

##### # 3.5 Inputs and State ~

###### Converter (변환기) 만들기

- 우린 HTML을 사용하고 있는 게 아니라 JSX를 사용하고 있음, label 과 input을 for 과 id로 연결하는 건 html 방식임
  
  - 에러가 뜨지 않는 건 production.min.js 를 사용 중이기 때문, 이걸 development.min.js로 바꾸면 에러 뜸
    
    ![](C:\Users\SSAFY_hojung\AppData\Roaming\marktext\images\2022-12-10-20-08-10-image.png)

- for, class 등은 javascript 용어임 따라서 JSX 사용 시에는 
  
  for => **<mark>htmlFor</mark>**, class=> <mark>**className**</mark>으로 바꿔서 써줘야 함
  
  ```javascript
    <script type="text/babel">
      const root = document.getElementById("root");
  
      function App() {
        const [minutes, setMinutes] = React.useState(0);
        const onChange = (event) => {
          setMinutes(event.target.value);
        };
        const reset = () => {
          setMinutes(0);
        }
        return (
          <div>
            <div>
              <h1 className="hi">Super Converter</h1>
              <label htmlFor="minutes">Minutes</label>
              <input
                value={minutes}
                id="minutes"
                placeholder="Minutes"
                type="number"
                onChange={onChange}
              />
            </div>
            <div>
              <label htmlFor="hours">Hours</label>
              <input
                id="hours"
                value={Math.round(minutes / 60)}
                placeholder="Hours"
                type="number"
                disabled
              />
            </div>
            <button onClick={reset}>Reset</button>
          </div>
        );
      }
  
      ReactDOM.render(<App />, root);
    </script>
  ```

- input 값을 외부에서도 수정해주기 위해서, state 값을 input의 value로 받음

- onChange 함수는 데이터를 업데이트

##### # 3.7 State Practice part Two

###### - flip function

```javascript
<div>
  <label htmlFor="hours">Hours</label>
  <input
    id="hours"
    value={Math.round(minutes / 60)}
    placeholder="Hours"
    type="number"
    disabled={flipped === false}
  />
</div>
```

- JSX의 힘 덕분에, 우린 JS 코드를 이렇게 저렇게 이용할 수 있음 
  
  **<mark>disabled={flipped === false}</mark>** == **<mark>disabled={flipped}</mark>** 
  
  => 만약 flipped가 false이면 hours input은 disabled

- 내가 Minutes에 값을 입력했을 땐 Hours에 변환된 시간이 떠야하고, Hours에 입력했을 땐 Hours에 입력된 값 그대로를 보여줘야 함(Minutes input에는 분으로 변환된 시간을 보여주면서) => 삼항 연산자 사용 (if를 인라인 형태로 작성한 거라고 생각하면 됨)

- inverted ? amount * 60 : amount 
  
  - inverted가 true이면 amount*60, false이면 amout

- 시간 변환기 완성 코드
  
  ```javascript
    <script type="text/babel">
      const root = document.getElementById("root");
  
      function App() {
        const [amount, setAmount] = React.useState(0);
        const [inverted, setInverted] = React.useState(false);
        const onChange = (event) => {
          setAmount(event.target.value);
        };
        const reset = () => setAmount(0);
        const onInvert = () => {
          reset();
          setInverted((current) => !current);
        };
        return (
          <div>
            <div>
              <h1>Super Converter</h1>
              <label htmlFor="minutes">Minutes</label>
              <input
                value={inverted ? amount * 60 : amount}
                id="minutes"
                placeholder="Minutes"
                type="number"
                onChange={onChange}
                disabled={inverted}
              />
            </div>
            <div>
              <label htmlFor="hours">Hours</label>
              <input
                id="hours"
                value={inverted ? amount : Math.round(amount / 60)}
                placeholder="Hours"
                type="number"
                disabled={!inverted}
                onChange={onChange}
              />
            </div>
            <button onClick={reset}>Reset</button>
            <button onClick={onInvert}>{inverted? "Turn back" : "Invert"}</button>
          </div>
        );
      }
  
      ReactDOM.render(<App />, root);
    </script>  ReactDOM.render(<App />, root);
    </script>
  ```

- <mark>**state를 바탕으로 UI를 변경할 수 있다는 게 얼마나 유용한 건지**</mark>를 알 수 있는 시간이었음
  
  

##### # 3.9 Final Practice and Recap

- 이제부터 App은 우리가 만들 여러 변환기들을 고를 수 있게 해줄거임

- 컴포넌트는 그 안에 또 다른 컴포넌트를 렌더링 할 수 있음

- {} 안에는 JS 코드를 쓸 수 있음, 그냥 쓰면 text로 인식됨
  
  ```javascript
      function KmToMiles() {
        return <h3>KM 2 M</h3>
      }
  
      function App() {
        const [index, setIndex] = React.useState("xx");
        const onSelect = (event) => {
          setIndex(event.target.value)
          // console.log(event.target)
          // console.log(event.target.value)
        };
        return (
          <div>
            <h1>Super Converter</h1>
            <select value={index} onChange={onSelect}>
              <option value="xx">Select your units</option>
              <option value="0">Minutes & Times</option>
              <option value="1">Km & Miles</option>
            </select>
            <hr />
            { index === "0" ? <MinutesToHours /> : null}
            { index === "1" ? <KmToMiles /> : null}
          </div>
        );
      }
  ```

- **modifier 함수를 실행하면, 컴포넌트들이 리렌더링 됨!**
