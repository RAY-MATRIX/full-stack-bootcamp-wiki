# 27 Sep 2023 - Lecture
## React哲学Part 3 (State & Component Life Cycle & Function Component)

### React Basics
- State
- Component Lifecycle
- Function Component
- Hooks(useState & useEffect)
- Handling Events

- Exercise
```javascript
import React, {Component} from "react";

export default class Excricise extends Component {
    render() {
        return (
            <div>{this.props.name ? `Hello ${this.props.name}` : "Hello World"}</div>
        );
    }
}
```

- Render Method (ReactDOM.render & root.render)
    - Syntax
        `ReactDOM.render(element, container[, callback])`
    - Render a React element into the DOM in the supplied container and return a reference to the component.
    - If the React element was previously rendered into container, this will perform an update on it and only mutate the DOM as necessary to reflect the latest React element.
    - If the optional callback is provided, it will be executed after the comoponent is rendered or updated.

    - `ReactDOM.render`
    Synchronous Rendering: The React updates block the main thread, ma aking it non-interactive while an update is in progress. Legacy Mode: Uses the traditional rendering algorithm that React has used since its inception. Batch Updates: Batches updates but without priorities or suspense.

        No Built-in Prioritization: All updates have the same priority level, unle less manually managed with a library or custom logic. API: Simpler API, just call ReactDOM.render(<App />, rootNode).
    - `ReactDOM.createRoot`
    Concurrent Rendering: Allows React to work on multiple tasks at ( once without blocking the main thread. Concurrent Mode: Uses the new concurrent rendering algorithm f or better performance.

        Batch Updates with Prioritization: Can batch updates and prioritize e them based on the type of the update.

        Suspense Support: Improved support for React Suspense, allowing g you to easily perform code splitting and data fetching with proper loading states.

        API: A bit different API. First create a root and then rengler into it.
        
    - State
    React components will often need dynamic information in order to render. For example, imagine a component that displays the score of a football game. The score of the game might change over time, meanint that the score is dynamic. Our component needs to know the changes of the score in oreder to render in a useful way.
    A component changes its state by calling function `this.setState()`
    `this.setState()` takes two arguments: an object that will update the components state, and a callback called when the update is done.
    
### Component Lifecycle    
- Initialization
- Mounting, loading the component for the first time.
    - constructor: 组建的构造函数，用于初始化状态
    - static getDerivedStateFromProps: 在渲染前，用于从新的属性中获取和更新状态
    - render: 返回JSX元素
    - componentDidMount: 组件挂载后调用，常用于发起网络请求
- Updating, resulting from changed state or changed props
    - getDerivedStateFromProps
    - shouldComponentUpdate： 返回布尔值以决定组建是否应该更新
    - static getDerivedStateFromProps： 同挂载
    - render：同挂载
    - getSnapshotBeforeUpdate: 在DOM更新之前获取一些信息
    - componentDidUpdate: 组件更新后调用
- Unmounting, removing the component from the DOM
    - componentWillUnmount: 组件即将卸载时调用，常用于清理比如取消网络请求，清楚计时器等等

    
### Function Component 
A Function Component, often referred to simply as a functional component, is one of the ways to define a component in React.

- **Simplicity**: Function components are generally more concise than class components. 
- **Stateless** (Initially): Before the introduction of H looks in React 16.8, function components were typically stateless; that is, they didn't mana ge their own state. This has changed with the introduction of the useState Hook.
- **No Lifecycle Methods**: Prior to Hooks, function components didn't have lifecycle methods. With the useEffect Hook, you can now replicate the effects you would achieve with lifecycle methods in class components.
- **React Hooks**: React Hooks can be used exclus ively inside function components to mimic features like state, lifecycle methods, context, ar Id other React features which were originally available only in class components.

> Tips: 快捷键 `rcc` `rfc`快捷生成一个class component/functional component



### Hooks
Hooks are new to React 16.8. They enable you to use state and other React features without having to write a class.
- `useState`: To manage states. Returns a stateful value and an updater function to update it.
- `useEffect`: To manage side-effects like API calls, subscriptions, timers, mutations, and more.
- `useContext`: To return the current value for a context.
- Hooks让我们的函数组件拥有了类组件的特性，例如组件内的状态、生命周期

- Example
```javascript
import React,{ usestate } from "react";

export default function FunctionComponent(props) {
    const [message,setMessage]= useState(props.name);
    console.log(props.name);
    
    const btnHandleClick =() =>{
        setMessage("Say Hello" + props.name);
    };
    
    return (
        <div>
            <div>{message}</div>
            <button onclick={btnHandleClick}>Say Hello</button>
        </div>
    );
}
```
> 平时做练习/小demo可以找一些online IDE，更方便快捷