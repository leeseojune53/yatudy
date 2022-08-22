# ContextApi란?

기존에 Prop drilling로 불필요한 컴포넌트에도 값을 넘겨주는 것은 props가 변경되면 불필요하게 수정해야하는 공수가 너무 많이 들게된다.

그것을 해결하는 방법으로 React에서 Context를 만들었다.

### 😎 예시

```tsx
// Context 생성
import { createContext } from "react";

export interface AppContextInterface {
  isDark: boolean;
  setIsDark: Function;
}

export const AppContext = createContext<AppContextInterface | null>(null);

```

```tsx
// Context 사용
import { useContext } from "react";
import { AppContext } from "../context/contextApiContext";

function Header() {
  const appContext = useContext(AppContext);

  return (
    <div style={{ backgroundColor: appContext?.isDark ? "black" : "white" }}>
      HIHI
    </div>
  );
}

export default Header;

```

App.tsx에서는 Props로 하위 컴포넌트에 값을 넘길 필요가 없다.

```tsx
// App.tsx
import React, { useState } from "react";
import logo from "./logo.svg";
import "./App.css";
import { AppContext, AppContextInterface } from "./context/contextApiContext";
import Header from "./component/Header";
import Content from "./component/Content";

function App() {
  const [isDark, setIsDart] = useState(false);

  const appContext: AppContextInterface = {
    isDark: isDark,
    setIsDark: setIsDart,
  };

  const changeMode = (): void => {
    setIsDart(!isDark);
  };

  return (
    <AppContext.Provider value={appContext}>
      <Header></Header>
      <Content></Content>
      <button onClick={changeMode}></button>
    </AppContext.Provider>
  );
}

export default App;

```

### ⚠ 주의점

"No Sliver Bullet(은탄환은 없다)"라는 말이 있듯이, 좋아보이는 Context를 남용하면, Component를 재사용하기 어려워 질 수 있다.

따라서, Prop drilling을 피하기 위한 목적이라면 Context가 아닌 Component Composition(합성)으로 문제를 해결하는 것이 더 바람직하다.