# 01 Oct 2023 - Lecture
## React 哲学 Part 4(Handling Event & Hooks)

### State Tips
In React, you shouldn't directly modify the value of **state**. 

For exal mple, `this.state.counter = 1` is incorrect. The right way is to use `this.setState({counter: 1 })`. This is because React uses state updates to detect whether it's a component.

The `this.setState()` method is used to update the state of a component. When you need to compute the new state based on the previous state or props, you can use a function instead of an object as the first argument to `this.setState()`. 

Function Argument: Instead of passing an object directly to `this.setStafe()`, you're passing a function. This function receives two parameters:

```javascript
handleIncrement = () => {
    this.setState((prevState, props) => {
        return (count: prevState.count + 1);
    });
}
```

- prevState: This is a reference to the state of the component before the update.
- props: This is a reference to the current props of the component. (**Note**:In newer versions of React, you can get the props using `this.props` directly inside the function, so you often won't see this parameter being used.)

Why use a function?: There are cases when the next state depends on the current state. Because `this.setState()` is asynchronous (for performance reasons), if you call `this.setState()` multiple times in succession, you can't be sure about the order in which they'll finish or what the current state will be when they execute. By using the function form of `this.setState()`, you're ensuring that you're working with the most recent state and props.

- Example

```javascript
import React, { Component } from "react";
//定义一个名为Counter的React组件
class Counter extends React.Component {
    //构造函数用于初始化组件的状态
  constructor(props) {
    super(props);//调用父类的构造函数，传入props
    this.state = {count: 0};//初始化组件的state为count:0
  }

//定义一个名为increment的方法用于增加数值
  increament = () => {
  // 使用this.setState方法更新state.基于prevState(之前的状态)来更新数值
    this.setState(prevState => ({
      count: prevState.count + 1//让count增加1
    }));
  };

// render方法定义了组件的UI如何显示
  render() {
    return (
    // 返回一个包含段落和按钮的div元素
      <div>
        <p>{this.state.count}</p>//显示当前的计数值
        <button onClick={this.increment}>Increment</button>
      </div>
    );
  }

}

```


- Handling Event
    1. React events are named using camelCase, rather than lowercase.
    2. With JSX you pass a function as the event handler, rather than a string.
    3. Some general events that you would see in and out when dealing with React-based websites:
        - Clicking an element
        - Submitting a form
        - Scrolling page
        - Hovering an element
        - Loading a webpage
        - Input field change
        - User stroking a key
        - Image loading

    ```javascript
    // YES
    <button onClick={this.handleClick}>Click me</button>
    // NO
    <button onClick={"this.handleClick()"}>Click me</button>
    ```
    
    ```javascript
    //非构造函数
    import React, { Component } from "react";

    class Counter extends React.Component {
      handleClick() {
        alert('Button was clicked!');
      }
    
      render() {
        return (
          <button onClick={this.handleClick}>Click me</button>
        );
      }
    }
    ```
    
    推荐使用组件方法：
    ```javascript
    import React, { Component } from "react";

    class InputComponentTest extends React.Component {
      constructor(props) {
        super(props);
        this.state = { value: '' };
      }
    
      handleInputChange = (event) => {
        this.setState({ value: event.target.value });
        console.log(event.target.value);
      };
    
      render() {
        return (
          <input type="text" value={this.state.value} onChange={this.handleInputChange} />
        );
      }
    }
    
    export default InputComponentTest;
    ```
    
- Conditional Rendering
    Conditional rendering is a programming concept where a pieve of UI is shown or hidden based on certain conditions. In the context of frontend development, it means rendering components or elements depending on the state or props that a component reveives.
    
    Different frameworks and libraries have their own syntax and methodologies for conditional rendering.
    
    - example
    ```javascript
    import React, { useState } from'react';

    function ToggleGreeting() {
      const [isUserLoggedIn,setIsUserLoggedIn]= useState(false);
    
      return (
        <div>
          {isUserLoggedIn ? <h1>welcomback!</h1> : <h1>Please sign up</h1>}
          <button onclick={() => setIsUserLoggedIn(!isUserLoggedIn)}>
            {isUserLoggedIn ? 'Log out' : 'Log in'}
          </button>
        </div>
      );
    }
    
    export default ToggleGreeting;
    ```
    
- Lists
    In React, lists are a common pattern used to display a collection of similar items, like a list of
comments, products, or todos. Lists in React are typically rendered using arrays of data with the help of the `map()` function
    - Basic List Components
    ```javascript
    funtion NumberList(props) {
        const numbers = props.numbers;
        const listItems = numbers.map(number) => 
            <li key={number.toString()}>{number}</li>
        );
    }
    
    const numbers = [1, 2, 3, 4, 5];
    ReactDOM.render(
        <NumberList numbers={numbers} />,
        document.getElementById('root')
    );
    ```
    
- Keys
    Keys in the list, 目的：正确和高效的识别和更新列表中的数据
    The key should be a string that uniquely identifies a list item amongs its slblings. Most often you would use IDs from you data as key. Jeys should be stable, predictable, and unique.
    eg:
    ```javascript
    const todoItems = todos.map((todo) => 
        <li key={todo.id}>
            {todo.text}
        </li>
    ```
    
    - Keys: While rendering a list, each item should have a unique key prop. It is a special string attribute needed to help React identify which items have changed, are added, or are removed. Keys should be given to the elements inside the array and should be unique among their siblings. However, they dont need to be globally unique.

    - Avoid using indexes as keys: If the items in your list do not have a stable ID, you may use the item index as a key as a last resort. However, this can cause issues with performance and component state, especially if the order of items can change.
    
    - Keys must only be unique among siblings: Keys used within arrays should be unique among their siblings. They don't need to be unique across the entire React application.
    
    - Extracting components: If you find yourself often writing a lot of logic inside the map() callback, it might be a good idea to extract a component for better readability.

    - Rendering lists is a fundamental part of working with React. Understanding how to effectively render and manage lists will help in building efficient and user-friendly applications
    - example:
        ```javascript
        import React from "react";

        function ListItem(props) {
          return <li>{props.value}</li>;
        }
        
        function NumberList01(props) {
          return (
            <ul>
              {props.products.map((products) => (
                <ListItem key={products.toString()} value={products} />
              ))}
            </ul>
          );
        }
        export default NumberList01;
        ```

- Forms
  Controlled Components
  An input form element whose value is controlled by React in this way is called a controlled component. The input's value is always driven by the React state.
  - 处理多个属性可以通过`event.target.name`来处理
  - Handling Single Input
  ```javascript
  const NameForm = () => {
    const [value, setValue] = React.useState('');
    const handleChange = (event) => {
        setValue(event.target.value);
    }
    return (
        <form>
            <label>
                Name: <input type="text" value={value} onChange={handleChange} />
            </label>
        </form>
    );
  }
  ```  
  
- Why use Hooks
    Hooks were introduced in React 16.8 to allow you to use state and other React features without writing a class component. Here are some reasons why you might use hooks:

    - Functional Components: With hooks, you can write your components as functions instead of classes. Functional components are generally considered easier to read and write, and they avoid the complexity and boilerplate of this and class syntax.
    - Reusability&Composition: Custom hooks allow you to extract component logic into reusable functions. Instead of having higher-order components or render props for sharing behaviour, you can split your logic into small functions based on what pieces are related.
    - Reduced Side Effects: With the `useEffect` hook, it's clear when side effects occur in relation to the component's lifecycle. This makes it easier to ensure that effects happen when you want, reducing bugs.
    - Simplified State Management: `useState` provides a way to have local state without setting up a constructor and this.setState.
    - Consistent Component Structure: Components that use hooks can be more consistent and easier to read. With class components, lifecycle methods can be scattered throughout a component, making it hard to see which props and states are used by each lifecycle method. 
    - Improved Optimization:The React team has optimized hooks to reduce component re-renders, making applications more efficient. Integration with Other React Features: Some newer features of React, like Concurrent Mode, are designed with hooks in mind.
    - Reduced Bundle Size: In some cases, moving to hooks can lead to smaller bundle sizes due to the reduction in overhead from class components.
    - Easier Testing: Functional components using hooks can be easier to test in some scenarios since they are just functions without the need to deal with instances.
    - Easier Refactoring: If you need to change from a small component without state to one with state, hooks can make this transition seamless.
    - Conciseness and Clarity: Once familiar, many developers find hooks syntax and usage to be more direct and clear compared to class lifecycle methods.

- Hooks are functions that let you "hook into" React state and lifecycle features from function components. Hooks do not work inside classes.
- Type of Hooks
    - Built-in hooks
    - Custom hooks

- Built-in hooks 中的`useEffect`
    useEffect 是一个执行某些可能具有副作用的Effect函数(例如数据获取，设置/销毁定时器等等), 它也可以返回一个清理函数。
    `useEffect(effectFn, deps)` // deps是依赖数组
    
    React 会在每次渲染后都运行Effect，而以来数组就是用来控制是否该触发effect,从而能够减少不必要的计算，优化性能。
    具体而言，只要以来数组中的每一项与上一次渲染相比都没有改变，那么就跳过本次Effect的执行。
    ```javascript
    useEffect(() => {
        console.log('组件渲染后的代码');
    });
    
    useEffect(() => {
        console.log('仅在组件首次渲染后执行');
    }, []);
    
    useEffect(() => {
        console.log('count变化后运行');
    }, [count]);
    
    useEffect(() => {
        if(condition){
            // 执行
        }
    }, [Dependency]);
    ```
    
- Custom hook
    A custom hook in React is essentially a JavaScript function who ose name starts with "use" and that may call other React hooks. Custom hooks allow developers to extract component logic into reusable functi ions. This makes it easier to share and manage stateful logic across components.

    - Benefits:
        - Reusability: Custom hooks let you reuse stateful logic between different components without re-writing the same logic.
        - Separation of Concerns: By using custom hooks, you can break down complex components into smaller, more manageable pieces of functionality.
        - Cleaner Codebase: It leads to a more organized and readable codebase as related logic can be grouped together in a custom hook.

        ```javascript
        //清除定时器
        function TimerComponentUseEffect() {
          const [time, setTime] = useState(new Date());
        
          useEffect(() => {// 启动定时器
            const timerId = setInterval(() => {
              setTime(new Date());
            }, 1000);
        
            // 清除函数:当组件卸载或者useEffect重新运行时被调用 return ( ) => {
            return () => {
              clearInterval(timerId);
            };
          }, []); // 由于我们想要定时器只启动一次，所以依赖数组为空
          
          return (
            <div>
              Current Time:{time.toLocaleTimeString()}
            </div>
          );
        }
        
        export default TimerComponentUseEffect;
        ```
        
        ```javascript
        //自定义hook
        import { useState, useEffect } from 'react';

        function useFetchData(url) {
          const [data, setData] = useState(null);
          const [loading, setLoading] = useState(true);
          const [err, setErr] = useState(null);
        
          useEffect(() => {
            fetch(url)
              .then(response => response.json())
              .then(data => {
                setData(data);
                setLoading(false);
              })
              .catch(err => {
                setErr(err);
                setLoading(false);
              });
          }, [url]);
          
          return ( data, loading, err );
        }
        
        export default useFetchData;
        ```