**Chapter #5 ~ #6**

# React App with nodejs

## Create React App

Install nodejs and create React App using npx.

```
npx create-react-app [App Name]
```

All other files in src/ except index.js and App.js can be removed.

To run the app,

```
npm start
```

For more information, you can refer README.md in the App folder.

<br>

## Divide and Conquer

- App.js

```
import Button from "./Button";

function App() {
  return (
    <div>
      <Button text="Continue" />
    </div>
  );
}

export default App;
```

<br>

- Button.js

```
import PropTypes from "prop-types";
import styles from "./Button.module.css";

function Button({text}) {
    return <button
                className={styles.btn}
            >
                {text}
            </button>;
}

Button.propTypes = {
  text: PropTypes.string.isRequired,
};

export default Button;
```

<br>

- Button.module.css

```
.btn {
    color: white;
    background-color: tomato;
}
```

> css의 경우 style 속성으로 넣기 보다 className(class)으로 지정하면 깔끔하게 구현 가능.

<br>
<br>

# States & Effects

state에 변동이 있을 경우 해당 컴포넌트의 자식 컴포넌트들까지 모든 코드가 리랜더링되는 것이 원칙이지만 처음 랜더링된 이후 다시 실행할 필요가 없는 함수는 useEffect()를 이용해 처리하면 된다.
useEffect()의 첫번째 매개변수는 재실행이 필요없는 함수, 두번째 매개변수는 재실행 조건이 되는 변수 리스트. (해당 리스트에 있는 변수에 상태 변경이 발생할 경우 리랜더링)

> _Example_
>
> - **Call APIs** : No need to execute repeatedly after getting data when initiallized.

<br>

- App.js

```
import styles from "./App.module.css";
import { useState, useEffect } from "react";


function App() {
  const [counter, setValue] = useState(0);
  const [keyword, setKeyword] = useState("");

  const onClick = () => setValue((prev) => prev + 1);
  const onChange = (event) => setKeyword(event.target.value);

  console.log("I run all the time.")
  useEffect(() => {
    console.log("Call the API...")
  }, []);

  useEffect(() => {
    if (keyword !== "" && keyword.length > 5) {
      console.log("Search for", keyword);
    }
  }, [keyword]);

  return (
    <div>
        <input
            value={keyword}
            onChange={onChange}
            type="text"
            placeholder="Search here..."
        />

        <h1 className={styles.title}>{counter}</h1>
        <button onClick={onClick}>click me</button>
    </div>
  );
}

export default App;
```

<br>

## cleanup

컴포넌트가 제거(destroy)될 때 동작할 기능을 정의하고 싶을 경우 useEffect()의 첫번째 매개변수인 함수 끝에 return 키워드와 함께 clean-up 함수를 정의해주면 된다.

> _Example_
>
> - 메모리 누수 방지

<br>

- App.js

```
import { useState, useEffect } from "react";


function Hello() {
  function byeFn() {
    console.log("Bye");
  }
  function hiFn() {
    console.log("Hi!");
    return byeFn;
  }

  useEffect(() => {
    console.log("Hi!");
    return () => console.log("Bye");
  }, []);
  return <h1>Hello</h1>;
}

function App() {
  const [showing, setShowing] = useState(false);
  const onClick = () => setShowing((prev) => !prev);

  return (
    <div>
        {showing ? <Hello /> : null}
        <button onClick={onClick}>{showing ? "Hide" : "Show"}</button>
    </div>
  );
}

export default App;

```

<br>
<br>
<br>
