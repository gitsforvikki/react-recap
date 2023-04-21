# react-recap


1.  ### What are the major features of React?

The major features of React are:

- Uses JSX syntax, a syntax extension of JS that allows developers to write HTML in their JS code.
- It uses VirtualDOM instead of RealDOM considering that RealDOM manipulations are expensive.
- Supports server-side rendering.
- Follows Unidirectional data flow or data binding.
- Uses reusable/composable UI components to develop the view.

2.  ### How to create components in React?

There are two possible ways to create a component.

Function Components: This is the simplest way to create a component. Those are pure JavaScript functions that accept props object as the first parameter and return React elements:
```javascript
function Greeting({ message }) {
  return <h1>{`Hello, ${message}`}</h1>;
}
```
Class Components: You can also use ES6 class to define a component. The above function component can be written as:

```javascript
class Greeting extends React.Component {
  render() {
    return <h1>{`Hello, ${this.props.message}`}</h1>;
  }
}
```
3. ### When to use a Class Component over a Function Component?


If the component needs state or lifecycle methods then use class component otherwise use function component.

However, from React 16.8 with the addition of Hooks, you could use state , lifecycle methods and other features that were only available in class component right in your function component. So, it is always recommended to use Function components, unless you need a React functionality whose Function component equivalent is not present yet, like Error Boundaries.


4. ### React life cycle

Each component in React has a lifecycle which you can monitor and manipulate during its three main phases. The three phases are: Mounting, Updating, and Unmounting.
Components are created (mounted on the DOM), grow by updating, and then die (unmount on DOM). This is referred to as a component lifecycle.

There are different lifecycle methods that React provides at different phases of a component’s life. React automatically calls the responsible method according to the phase in which the component is. 


#### ***Lifecycle Methods***


A component’s lifecycle is broadly classified into four parts:

- initialization
- mounting
- updating, and
- unmounting.


***Initialization***

This is the phase in which the component is going to start its journey by setting up the state  and the props. This is usually done inside the constructor method.

```javascript
class Initialize extends React.Component {
    constructor(props)
    {
    // Calling the constructor of
    // Parent Class React.Component
    super(props);
    // initialization process
    this.state = {
       date : new Date(),
       clickedStatus: false
     };
} 

```

***Mounting***


The name is self-explanatory. Mounting is the phase in which our React component mounts on the DOM (i.e., is created and inserted into the DOM).
In this phase, our component renders the first time. The methods that are available in this phase are:

- componentWillMount()
-  componentDidMount()


***componentWillMount()***

This method is called just before a component mounts on the DOM or the render method is called. After this method, the component gets mounted.

***Note:***  You should not make API calls or any data changes using this.setstate in this method because it is called before the render method. 


***componentDidMount()***

This method is called after the component gets mounted on the DOM. Like componentWillMount, it is called once in a lifecycle. Before the execution of this method, the render method is called (i.e., we can access the DOM). We can make API calls and update the state with the API response.

```javascript
class LifeCycle extends React.Component {
  componentWillMount() {
      console.log('Component will mount!')
   }
  componentDidMount() {
      console.log('Component did mount!')
      this.getList();
   }
  getList=()=>{
   /*** method to make api call***
  }
  render() {
      return (
         <div>
            <h3>Hello mounting methods!</h3>
         </div>
      );
   }
}
```

***Updating***

This is the third phase through which our component passes. After the mounting phase where the component has been created,and inserted into DOM, the update phase comes into the scene. This is where component’s state changes and hence, re-rendering takes place.

In this phase, the data of the component (state & props) updates in response to user events like clicking, typing and so on. This results in the re-rendering of the component.
The methods that are available in this phase are:

- shouldComponentUpdate()
- componentWillUpdate()
- ComponentDidUpdate()


***shouldComponentUpdate()***

This method determines whether the component should be updated or not. By default, it returns true. But at some point, if you want to re-render the component on some condition, then ```shouldComponentUpdate``` method is the right place.

Suppose, for example, you want to only re-render your component when there is a change in prop — then utilize the power of this method. It receives arguments like nextProps and nextState which help us decide whether to re-render by doing a comparison with the current prop value.

***componentWillUpdate()***

Like other methods, its name is also self-explanatory. It is called before the re-rendering of the component takes place. It is called once after the ‘shouldComponentUpdate’ method. If you want to perform some calculation before re-rendering of the component and after updating the state and prop, then this is the best place to do it.

***ComponentDidUpdate()***

This method is called just after the re-rendering of the component. After the new (updated) component gets updated on the DOM, the ‘componentDidUpdate’ method is executed. This method receives arguments like prevProps and prevState.

Have a look to understand the updating methods better:

```javascript
class LifeCycle extends React.Component {
      constructor(props)
      {
        super(props);
         this.state = {
           date : new Date(),
           clickedStatus: false,
           list:[]
         };
      }
      componentWillMount() {
          console.log('Component will mount!')
       }
      componentDidMount() {
          console.log('Component did mount!')
          this.getList();
       }
      getList=()=>{
       /*** method to make api call***
       fetch('https://api.mydomain.com')
          .then(response => response.json())
          .then(data => this.setState({ list:data }));
      }
       shouldComponentUpdate(nextProps, nextState){
         return this.state.list!==nextState.list
        }
       componentWillUpdate(nextProps, nextState) {
          console.log('Component will update!');
       }
       componentDidUpdate(prevProps, prevState) {
          console.log('Component did update!')
       }
      render() {
          return (
             <div>
                <h3>Hello Mounting Lifecycle Methods!</h3>
             </div>
          );
       }
}
```
***Unmounting***

This is the last phase in the component’s lifecycle. As the name clearly suggests, the component gets unmounted from the DOM in this phase. The method that is available in this phase is:

***componentWillUnmount()***


This method is called before the unmounting of the component takes place. Before the removal of the component from the DOM, ‘componentWillUnMount’ executes. This method denotes the end of the component’s lifecycle.
Here isa flowchart representation of lifecycle methods:

 ![life_cycle](https://user-images.githubusercontent.com/52384251/224494076-7baba995-9b58-40d9-b67b-eb1d705473cf.png)





5. ### What is the Virtual DOM?

The virtual DOM (VDOM) is a programming concept where an ideal, or “virtual”, representation of a UI is kept in memory and synced with the “real” DOM by a library such as ReactDOM. This process is called reconciliation.

React creates a virtual DOM. When state changes in a component it firstly runs a "diffing" algorithm, which identifies what has changed in the virtual DOM. The second step is reconciliation, where it updates the DOM with the results of the difference.

This Virtual DOM works in three simple steps.

- Whenever any underlying data changes, the entire UI is re-rendered in Virtual DOM representation.

![vd-1](https://user-images.githubusercontent.com/52384251/224521191-834fff3b-31d3-43d9-897c-e26d397f1631.png)

- Then the difference between the previous DOM representation and the new one is calculated.

![vd-2](https://user-images.githubusercontent.com/52384251/224521210-bed561d4-a94d-402e-82cb-9f949bb3f83f.png)

- Once the calculations are done, the real DOM will be updated with only the things that have actually changed. 


![vd-3](https://user-images.githubusercontent.com/52384251/224521219-79476509-042f-4616-ad25-ab62daf298b4.png)





6. ### What is state in React?

State of a component is an object that holds some information that may change over the lifetime of the component. The important point is whenever the state object changes, the component re-renders. It is always recommended to make our state as simple as possible and minimize the number of stateful components.

Let's take an example of User component with message state. Here, useState hook has been used to add state to the User component and it returns an array with current state and function to update it.

```javascript

import React, { useState } from "react";

function User() {
  const [message, setMessage] = useState("Welcome to React world");

  return (
    <div>
      <h1>{message}</h1>
    </div>
  );
}

//class components

class User extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      message: "Welcome to React world",
    };
  }

  render() {
    return (
      <div>
        <h1>{this.state.message}</h1>
      </div>
    );
  }
}
```



7. ### What are props in React?

Props are inputs to a React component. They are single values or objects containing a set of values that are passed to React Components on creation. They are data passed down from a parent component to a child component.

Props are inputs to components. They are single values or objects containing a set of values that are passed to components on creation similar to HTML-tag attributes. They are data passed down from a parent component to a child component.

The primary purpose of props in React is to provide following component functionality:

1. Pass custom data to your component.
2. Trigger state changes.
3. Use via this.props.reactProp inside component's render() method.

props in class based component
```javascript
import React from "react";
import ReactDOM from "react-dom";

class ChildComponent extends React.Component {
  render() {
    return (
      <div>
        <p>{this.props.name}</p>
        <p>{this.props.age}</p>
      </div>
    );
  }
}

class ParentComponent extends React.Component {
  render() {
    return (
      <div>
        <ChildComponent name="John" age="30" />
        <ChildComponent name="Mary" age="25" />
      </div>
    );
  }
}
```

props in functional based component

```javascript
import React from "react";
import ReactDOM from "react-dom";

const ChildComponent = (props) => {
  return (
    <div>
      <p>{props.name}</p>
      <p>{props.age}</p>
    </div>
  );
};

const ParentComponent = () => {
  return (
    <div>
      <ChildComponent name="John" age="30" />
      <ChildComponent name="Mary" age="25" />
    </div>
  );
};
```


8. ### Why should we not update the state directly?

  If you try to update the state directly then it won't re-render the component.

```javascript

  //Wrong
  this.state.message = "Hello world";
  ```
  Instead use setState() method. It schedules an update to a component's state object. When state changes, the component responds by re-rendering.
  
```javascript
  //Correct
  this.setState({ message: "Hello World" });
  
```
Note: You can directly assign to the state object either in constructor or using latest javascript's class field declaration syntax.



9. ### What is the difference between state and props?

  In React, both state and props are are plain JavaScript objects and used to manage the data of a component, but they are used in different 
  ways and have different characteristics. state is managed by the component itself and can be updated using the setState() function. Unlike props, 
  state can be modified by the component and is used to manage the internal state of the component. Changes in the state trigger a re-render of the         component and its children. props (short for "properties") are passed to a component by its parent component and are read-only, meaning that they 
  cannot be modified by the component itself. props can be used to configure the behavior of a component and to pass data between components.

10. ### How to bind methods or event handlers in JSX callbacks?

    There are 3 possible ways to achieve this in class components:

    1. **Binding in Constructor:** In JavaScript classes, the methods are not bound by default. The same rule applies for React event handlers 
      defined as class methods. Normally we bind them in constructor.

       ```javascript
       
       class User extends Component {
         constructor(props) {
           super(props);
           this.handleClick = this.handleClick.bind(this);
         }
         handleClick() {
           console.log("SingOut triggered");
         }
         render() {
           return <button onClick={this.handleClick}>SingOut</button>;
         }
       }
       
       ```

    2. **Public class fields syntax:** If you don't like to use bind approach then _public class fields syntax_ can be used to correctly bind callbacks.        The Create React App eanables this syntax by default.

       ```jsx harmony
       handleClick = () => {
         console.log("SingOut triggered", this);
       };
       ```

       ```jsx harmony
       <button onClick={this.handleClick}>SingOut</button>
       ```

    3. **Arrow functions in callbacks:** It is possible to use _arrow functions_ directly in the callbacks.

       ```jsx harmony
       handleClick() {
           console.log('SingOut triggered');
       }
       render() {
           return <button onClick={() => this.handleClick()}>SignOut</button>;
       }
       ```

    **Note:** If the callback is passed as prop to child components, those components might do an extra re-rendering. In those cases, it is preferred to      go with `.bind()` or _public class fields syntax_ approach considering performance.

  

11.  ### What are inline conditional expressions?

      You can use either _if statements_ or _ternary expressions_ which are available from JS to conditionally render expressions. Apart from these             approaches, you can also embed any expressions in JSX by wrapping them in curly braces and then followed by JS logical operator `&&`.

      ```jsx harmony

          <h1>Hello!</h1>;
          {
            messages.length > 0 && !isLogin ? (
              <h2>You have {messages.length} unread messages.</h2>
            ) : (
              <h2>You don't have unread messages.</h2>
            );
          }

      ```



12. ### What are controlled components?

    A component that controls the input elements within the forms on subsequent user input is called **Controlled Component**, i.e, every state mutation     will have an associated handler function.

    For example, to write all the names in uppercase letters, we use handleChange as below,

    ```javascript
    handleChange(event) {
      this.setState({value: event.target.value.toUpperCase()})
    }
    ```
    
    
    



13. ### What are uncontrolled components?

    The **Uncontrolled Components** are the ones that store their own state internally, and you query the DOM using a ref to find its current value when     you need it. This is a bit more like traditional HTML.

    In the below UserProfile component, the `name` input is accessed using ref.

    ```jsx harmony
    class UserProfile extends React.Component {
      constructor(props) {
        super(props);
        this.handleSubmit = this.handleSubmit.bind(this);
        this.input = React.createRef();
      }

      handleSubmit(event) {
        alert("A name was submitted: " + this.input.current.value);
        event.preventDefault();
      }

      render() {
        return (
          <form onSubmit={this.handleSubmit}>
            <label>
              {"Name:"}
              <input type="text" ref={this.input} />
            </label>
            <input type="submit" value="Submit" />
          </form>
        );
      }
    }
    ```

    In most cases, it's recommend to use controlled components to implement forms. In a controlled component, form data is handled by a React component.     The alternative is uncontrolled components, where form data is handled by the DOM itself.
    
    
    
 14. ### What is context?

     _Context_ provides a way to pass data through the component tree without having to pass props down manually at every level.

     For example, authenticated users, locale preferences, UI themes need to be accessed in the application by many components.

      ```javascript
      
     const { Provider, Consumer } = React.createContext(defaultValue);
     
      ```

    
    
 15. ### What is the purpose of using super constructor with props argument?

      A child class constructor cannot make use of `this` reference until the `super()` method has been called. The same applies to ES6 sub-classes as         well. The main reason for passing props parameter to `super()` call is to access `this.props` in your child constructors.

        **Passing props:**

        ```javascript
        class MyComponent extends React.Component {
          constructor(props) {
            super(props);

            console.log(this.props); // prints { name: 'John', age: 42 }
          }
        }
        ```

      **Not passing props:**

      ```javascript
      class MyComponent extends React.Component {
        constructor(props) {
          super();

          console.log(this.props); // prints undefined

          // but props parameter is still available
          console.log(props); // prints { name: 'John', age: 42 }
        }

        render() {
          // no difference outside constructor
          console.log(this.props); // prints { name: 'John', age: 42 }
        }
      }
      ```

      The above code snippets reveals that `this.props` is different only within the constructor. It would be the same outside the constructor.




16. ### What is reconciliation?
    When a component's props or state change, React decides whether an actual DOM update is necessary by comparing the newly returned element with the       previously rendered one. When they are not equal, React will update the DOM. This process is called reconciliation.



17. ### How to set state with a dynamic key name?

    If you are using ES6 or the Babel transpiler to transform your JSX code then you can accomplish this with _computed property names_.

    ```javascript
    handleInputChange(event) {
      this.setState({ [event.target.id]: event.target.value })
    }
    ```
    
    
18. ### Why React uses `className` over `class` attribute?

    `class` is a keyword in JavaScript, and JSX is an extension of JavaScript. That's the principal reason why React 
    uses `className` instead of `class`. Pass a string as the `className` prop.

    ```jsx harmony
    render() {
      return <span className={'menu navigation-menu'}>{'Menu'}</span>
    }
    ```
    
19. ### What are fragments?

      It's a common pattern in React which is used for a component to return multiple elements. _Fragments_ let you group a list o
      f children without adding extra nodes to the DOM.

      ```javascript
      render() {
        return (
          <React.Fragment>
            <ChildA />
            <ChildB />
            <ChildC />
          </React.Fragment>
        )
      }
      ```

     There is also a _shorter syntax_, but it's not supported in many tools:

      ```javascript
      render() {
        return (
          <>
            <ChildA />
            <ChildB />
            <ChildC />
          </>
        )
      }
      ```
    
    
    
    
    


    
    
    







20. ### What is Context API in ReactJS?

    Context provides a way to pass data through the component tree without having to pass props down manually at every level.

    Context is designed to share data that can be considered “global” for a tree of React components, such as the current authenticated user, theme, or       preferred language. Using context, we can avoid passing props through intermediate elements.
  

21. ###  What are React Hooks?

      Hooks are a new addition to React 16.8. They let you use state and other React features without writing a class.

      With Hooks, you can extract stateful logic from a component so it can be tested independently and reused. Hooks allow you to reuse stateful logic         without changing your component hierarchy


22. ### What is JSX?
    JSX is a shorthand for JavaScript XML. This is a type of file used by React which utilizes the expressiveness of JavaScript along with HTML like         template syntax. This makes the HTML file really easy to understand.


23. ### Why can’t browsers read JSX?

      Browsers can only read JavaScript objects but JSX in not a regular JavaScript object. Thus to enable a browser to read JSX, first, we need to             transform JSX file into a JavaScript object using JSX transformers like Babel and then pass it to the browser.


24. ### What is an event in React?

    In React, events are the triggered reactions to specific actions like mouse hover, mouse click, key press, etc. Handling these events are similar to    handling events in DOM elements. 

25. ### How do you create an event in React?

    ```javascript
    class Display extends React.Component({    
        show(evt) {
            // code   
        },   
        render() {      
            // Render the div with an onClick prop (value is a function)        
            return (            

    <div onClick={this.show}>Click Me!</div>

            );    
        }
    });
    ```
26. ### How to bind methods or event handlers in JSX callbacks?
 
     There are 3 possible ways to achieve this in class components:

     Binding in Constructor: In JavaScript classes, the methods are not bound by default. The same rule applies for React event handlers defined as class      methods. Normally we bind them in constructor.

      ```javascript
      class User extends Component {
        constructor(props) {
          super(props);
          this.handleClick = this.handleClick.bind(this);
        }
        handleClick() {
          console.log("SingOut triggered");
        }
        render() {
          return <button onClick={this.handleClick}>SingOut</button>;
        }
      }
      ```
    Public class fields syntax: If you don't like to use bind approach then public class fields syntax can be used to correctly bind callbacks. The           Create React App eanables this syntax by default.

    ```javascript

    handleClick = () => {
      console.log("SingOut triggered", this);
    };
    <button onClick={this.handleClick}>SingOut</button>

    ```
    Arrow functions in callbacks: It is possible to use arrow functions directly in the callbacks.

    ```javascript
    handleClick() {
        console.log('SingOut triggered');
    }
    render() {
        return <button onClick={() => this.handleClick()}>SignOut</button>;
    }

    ```
  27. ### What is the significance of keys in React?
          Keys are used for identifying unique Virtual DOM Elements with their corresponding data driving the UI. 
          These keys must be a unique number or string, using which React just reorders the elements instead of re-rendering them. 
          This leads to increase in application’s performance.

28. ### What is Redux?
          Redux is one of the most trending libraries for front-end development in today’s marketplace. It is a predictable state container for                     JavaScript applications and is used for the entire applications state management.


29. ### What are the three principles that Redux follows?

        - ***Single source of truth***:  The state of the entire application is stored in an object/ state tree within a single store. The single state                 tree makes it easier to keep track of changes over time and debug or inspect the application.


        - ***State is read-only***: The only way to change the state is to trigger an action. An action is a plain JS object describing the change. Just           like state is the minimal representation of data, the action is the minimal representation of the change to that data.


         - ***Changes are made with pure functions***: In order to specify how the state tree is transformed by actions, you need pure functions. Pure            functions are those whose return value depends solely on the values of their arguments.



30. ### List down the components of Redux.
     Redux is composed of the following components:

    - ***Action*** –  It’s an object that describes what happened.
    - ***Reducer*** –  It is a place to determine how the state will change.
    - ***Store*** – State/ Object tree of the entire application is saved in the Store.
    - ***View*** – Simply displays the data provided by the Store.


31. ### Show how the data flows through Redux?


![redux](https://user-images.githubusercontent.com/52384251/224524649-5c11c7ab-f4d2-4086-9926-4ae6ad6e4d79.png)


32. ### How are Actions defined in Redux?

      Actions in React must have a type property that indicates the type of ACTION being performed.
      In Redux, actions are created using the functions called Action Creators. 

33. ### Explain the role of Reducer.
      Reducers are pure functions which specify how the application’s state changes in response to an ACTION. Reducers work by taking in the previous           state and action, and then it returns a new state.
      It returns the previous state as it is, if no work needs to be done.

34. ###  What is the significance of Store in Redux?
      A store is a JavaScript object which can hold the application’s state and provide a few helper methods to access the state, dispatch actions and          register listeners. The entire state/ object tree of an application is saved in a single store. As a result of this, Redux is very simple and            predictable.


35. ### What is React Router?
     React Router is a powerful routing library built on top of React, which helps in adding new screens and flows to the application. This keeps the URL      in sync with data that’s being displayed on the web page.


36. ### Why do we need a Router in React?
      A Router is used to define multiple routes and when a user types a specific URL, if this URL matches the path of any ‘route’ defined inside the          router, then the user is redirected to that particular route.

37. ### Controlled and uncontrolled components

      In React, a controlled component is a component that has its state controlled by the parent component. The parent component passes the state as           props to the controlled component and also handles any changes to the state via callback functions. The controlled component only renders the             received props and does not have its own state.

      An uncontrolled component, on the other hand, maintains its own internal state and updates it using DOM events. The component directly updates the       DOM and does not rely on the parent component to pass and update the state.


38. ### How do you handle routing in a React application?

    In a React application, routing is typically handled using a library such as React Router. React Router allows you to define specific routes for         different parts of your application and map them to specific components. When the user navigates to a specific route, the corresponding component is     displayed on the page.

39. ### Explain the difference between server-side rendering and client-side rendering in React.

      In a React application, there are two main ways to render the components: server-side rendering (SSR) and client-side rendering (CSR).

      Server-side rendering (SSR) is when the initial render of a React application is done on the server. The server generates the HTML for the initial       state of the application and sends it to the browser. When the JavaScript bundle loads, React takes over and the application continues to function       as a SPA (Single-Page Application) on the client side.

      Client-side rendering (CSR) is when the React application is rendered entirely in the browser, using JavaScript. The browser requests the                  JavaScript bundle from the server and then renders the components on the client side.

40. ### What is the difference between a stateless component and a stateful component in React?

   In React, a component can be either stateless or stateful. The main difference between the two is how they manage and update their data.

   A stateless component, also known as a “dumb” or “presentational” component, is a component that does not maintain its own internal state. It receives    data and callbacks through props (short for properties) and only renders the UI based on those props.

   A stateful component, also known as a “smart” or “container” component, is a component that maintains its own internal state. It can handle internal      state updates and side effects, and may also manage the state of other child components.





41. ### What is the purpose of the combineReducers function in Redux?

T   he combineReducers function in Redux is used to combine multiple individual reducers into a single root reducer. In a Redux application, the state is     managed by a single store and each piece of the state is managed by a specific reducer. The combineReducers function takes an object whose keys           correspond to the keys in the state, and whose values are the individual reducers that will manage those parts of the state.

42. ### What is the difference between a functional component and a class component in React?

    In React, a functional component is a plain JavaScript function that takes in props and returns a React element. A class component is a JavaScript       class that extends React.Component and has a render method that returns a React element.

    One key difference between the two is that a class component can have local state and lifecycle methods, while a functional component cannot.             However, starting with React 16.8, functional components can also have a state using hooks.

    Functional components are considered simpler, easier to understand and test, and have better performance than class components. Class components are     useful when you need to use lifecycle methods or the local state.

43. ### What is the difference between state and props in React?
    State and Props are both concepts in React that are used to store and manipulate data within a React component. The main difference between the two       is that State is used to store and manage the data that is local and specific to a component, while Props are used to pass data from a parent             component to its child components.

44. ### Explain the concept of a Hook in React.

     During an interview, you can explain Hooks in the following way:

      “Hooks are a new feature in React that allows us to add state and other React features to functional components. They were introduced in React 16.8        and have since become a popular way to manage state and side effects in functional components. Hooks are named functions that start with the word        use and allow us to reuse stateful logic across components without having to write a class component. For example, the useState Hook allows us to        add state to a functional component and the useEffect Hook lets us perform side effects like data fetching or updating the document title. Hooks           make our code more reusable, easier to understand, and easier to test.”

45. ### What is the difference between a reducer and an action in Redux?

   In Redux, a reducer and an action are two different but related concepts.

    An action is a plain JavaScript object that describes the change that should be made to the state of the application. It has a type property that         defines the type of action being performed, and a payload property that provides any additional data needed to perform the action. Actions are           dispatched from the application to the Redux store, which then passes the action to the reducers.

     A reducer is a pure function that takes the current state of the application and an action, and returns the next state of the application. The            reducer is responsible for handling the actions and updating the state accordingly. It should not perform any side-effects, such as making API           calls, but should instead only return the next state.

    In summary, actions describe what should change, while reducers define how the state should change in response to the actions.

46. ### What is the difference between a React component and a React element?

   A React component is a JavaScript class or function that returns a React element. It is a reusable piece of UI that describes a part of the user         interface.

   A React element, on the other hand, is a plain JavaScript object that represents a DOM node. It is an immutable representation of a DOM node, which      can be created using React.createElement or JSX.


47. ### How events are different in React?

    Handling events in React elements has some syntactic differences:

    1. React event handlers are named using camelCase, rather than lowercase.
    2. With JSX you pass a function as the event handler, rather than a string.

   

48. ### What will happen if you use `setState()` in constructor?

    When you use `setState()`, then apart from assigning to the object state React also re-renders the component and all its children. You would get         error like this: _Can only update a mounted or mounting component._ So we need to use `this.state` to initialize variables inside constructor.

    

49. ### What is the impact of indexes as keys?

    Keys should be stable, predictable, and unique so that React can keep track of elements.

    In the below code snippet each element's key will be based on ordering, rather than tied to the data that is being represented. This limits the           optimizations that React can do.

    ```jsx harmony
    {
      todos.map((todo, index) => <Todo {...todo} key={index} />);
    }
    ```

    If you use element data for unique key, assuming todo.id is unique to this list and stable, React would be able to reorder elements without needing       to reevaluate them as much.

    ```jsx harmony
    {
      todos.map((todo) => <Todo {...todo} key={todo.id} />);
    }
    ```

50. ### What is the impact of indexes as keys?

    Keys should be stable, predictable, and unique so that React can keep track of elements.

    In the below code snippet each element's key will be based on ordering, rather than tied to the data that is being represented. This limits the optimizations that React can do.

    ```jsx harmony
    {
      todos.map((todo, index) => <Todo {...todo} key={index} />);
    }
    ```

    If you use element data for unique key, assuming todo.id is unique to this list and stable, React would be able to reorder elements without needing to reevaluate them as much.

    ```jsx harmony
    {
      todos.map((todo) => <Todo {...todo} key={todo.id} />);
    }
    ```

51. ### How do you conditionally render components?

    In some cases you want to render different components depending on some state. JSX does not render `false` or `undefined`, so you can use conditional _short-circuiting_ to render a given part of your component only if a certain condition is true.

    ```jsx harmony
    const MyComponent = ({ name, address }) => (
      <div>
        <h2>{name}</h2>
        {address && <p>{address}</p>}
      </div>
    );
    ```

    If you need an `if-else` condition then use _ternary operator_.

    ```jsx harmony
    const MyComponent = ({ name, address }) => (
      <div>
        <h2>{name}</h2>
        {address ? <p>{address}</p> : <p>{"Address is not available"}</p>}
      </div>
    );
    ```
52. ### What is CRA and its benefits?

    The `create-react-app` CLI tool allows you to quickly create & run React applications with no configuration step.

    Let's create Todo App using _CRA_:

    ```console
    # Installation
    $ npm install -g create-react-app

    # Create new project
    $ create-react-app todo-app
    $ cd todo-app

    # Build, test and run
    $ npm run build
    $ npm run test
    $ npm start
    ```
    
53. ### What is the lifecycle methods order in mounting?

    The lifecycle methods are called in the following order when an instance of a component is being created and inserted into the DOM.

    1. `constructor()`
    2. `static getDerivedStateFromProps()`
    3. `render()`
    4. `componentDidMount()`
    
    
    
 54.  ### Why should component names start with capital letter?

       If you are rendering your component using JSX, the name of that component has to begin with a capital letter otherwise React will throw an error           as an unrecognized tag. This convention is because only HTML elements and SVG tags can begin with a lowercase letter.

        ```jsx harmony
        class SomeComponent extends Component {
          // Code goes here
        }
        ```

        You can define component class which name starts with lowercase letter, but when it's imported it should have capital letter. Here lowercase is           fine:

        ```jsx harmony
        class myComponent extends Component {
          render() {
            return <div />;
          }
        }

             export default myComponent;
       ```

         While when imported in another file it should start with capital letter:

        ```jsx harmony
        import MyComponent from "./myComponent";
        ```

  55. ### What is the difference between constructor and getInitialState?

       You should initialize state in the constructor when using ES6 classes, and `getInitialState()` method when using `React.createClass()`.

        **Using ES6 classes:**

        ```javascript
        class MyComponent extends React.Component {
          constructor(props) {
            super(props);
            this.state = {
              /* initial state */
            };
          }
        }
        ```

        **Using `React.createClass()`:**

        ```javascript
        const MyComponent = React.createClass({
          getInitialState() {
            return {
              /* initial state */
            };
          },
        });
        ```
        
  56. ### What is the difference between `super()` and `super(props)` in React using ES6 classes?

        When you want to access `this.props` in `constructor()` then you should pass props to `super()` method.

        **Using `super(props)`:**

        ```javascript
        class MyComponent extends React.Component {
          constructor(props) {
            super(props);
            console.log(this.props); // { name: 'John', ... }
          }
        }
        ```

        **Using `super()`:**

        ```javascript
        class MyComponent extends React.Component {
          constructor(props) {
            super();
            console.log(this.props); // undefined
          }
        }
        ```
  
  57.  ### How to loop inside JSX?

        You can simply use `Array.prototype.map` with ES6 _arrow function_ syntax.

        For example, the `items` array of objects is mapped into an array of components:

        ```jsx harmony
        <tbody>
          {items.map((item) => (
            <SomeComponent key={item.id} name={item.name} />
          ))}
        </tbody>
        ```

        But you can't iterate using `for` loop:

        ```jsx harmony
        <tbody>
          for (let i = 0; i < items.length; i++) {
            <SomeComponent key={items[i].id} name={items[i].name} />
          }
        </tbody>
58. ### What is the difference between React and ReactDOM?

     The `react` package contains `React.createElement()`, `React.Component`, `React.Children`, and other helpers related to elements and component           classes. You can think of these as the isomorphic or universal helpers that you need to build components. The `react-dom` package contains              `ReactDOM.render()`, and in `react-dom/server` we have _server-side rendering_ support with `ReactDOMServer.renderToString()` and                         `ReactDOMServer.renderToStaticMarkup()`.
   
59. ### How to pretty print JSON with React?

     We can use `<pre>` tag so that the formatting of the `JSON.stringify()` is retained:

     ```jsx harmony
     const data = { name: "John", age: 42 };

     class User extends React.Component {
       render() {
         return <pre>{JSON.stringify(data, null, 2)}</pre>;
       }
     }

     React.render(<User />, document.getElementById("container"));
     ```
   
 60. ### Why you can't update props in React?

     The React philosophy is that props should be _immutable_ and _top-down_. This means that a parent can send any prop values to a child, but the           child can't modify received props.

  61.  ### How to update a component every second?

         You need to use `setInterval()` to trigger the change, but you also need to clear the timer when the component unmounts to prevent errors and            memory leaks.

         ```javascript
         componentDidMount() {
           this.interval = setInterval(() => this.setState({ time: Date.now() }), 1000)
         }

         componentWillUnmount() {
           clearInterval(this.interval)
         }
         ```

  62. ### How to import and export components using React and ES6?

       You should use default for exporting the components

       ```jsx harmony
       import React from "react";
       import User from "user";

       export default class MyProfile extends React.Component {
         render() {
           return <User type="customer">//...</User>;
         }
       }
       ```

 63. ### How to define constants in React?

     You can use ES7 `static` field to define constant.

     ```javascript
     class MyComponent extends React.Component {
       static DEFAULT_PAGINATION = 10;
     }
     ```
63. ### Is it possible to use async/await in plain React?

     If you want to use `async`/`await` in React, you will need _Babel_ and [transform-async-to-generator](https://babeljs.io/docs/en/babel-plugin-           transform-async-to-generator) plugin. React Native ships with Babel and a set of transforms.
     
     
64. ### How to implement _default_ or _NotFound_ page?

     A `<Switch>` renders the first child `<Route>` that matches. A `<Route>` with no path always matches. So you just need to simply drop path              attribute as below

     ```jsx harmony
     <Switch>
       <Route exact path="/" component={Home} />
       <Route path="/user" component={User} />
       <Route component={NotFound} />
     </Switch>
     ```
65. ### What are the core principles of Redux?

     Redux follows three fundamental principles:

     1. **Single source of truth:** The state of your whole application is stored in an object tree within a single store. The single state tree makes        it easier to keep track of changes over time and debug or inspect the application.
     2. **State is read-only:** The only way to change the state is to emit an action, an object describing what happened. This ensures that neither the        views nor the network callbacks will ever write directly to the state.
     3. **Changes are made with pure functions:** To specify how the state tree is transformed by actions, you write reducers. Reducers are just pure         functions that take the previous state and an action as parameters, and return the next state.
66. ### Can I dispatch an action in reducer?

     Dispatching an action within a reducer is an **anti-pattern**. Your reducer should be _without side effects_, simply digesting the action payload        and returning a new state object. Adding listeners and dispatching actions within the reducer can lead to chained actions and other side effects.


67. ### What is the difference between React context and React Redux?

     You can use **Context** in your application directly and is going to be great for passing down data to deeply nested components which what it was        designed for.

     Whereas **Redux** is much more powerful and provides a large number of features that the Context API doesn't provide. Also, React Redux uses            context internally but it doesn't expose this fact in the public API.
 
 68. ### What is Redux Thunk?

     _Redux Thunk_ middleware allows you to write action creators that return a function instead of an action. The thunk can be used to delay the             dispatch of an action, or to dispatch only if a certain condition is met. The inner function receives the store methods `dispatch()` and                `getState()` as parameters.
     
     
 69. ### What are the features of Redux DevTools?

     Some of the main features of Redux DevTools are below,

     1. Lets you inspect every state and action payload.
     2. Lets you go back in time by _cancelling_ actions.
     3. If you change the reducer code, each _staged_ action will be re-evaluated.
     4. If the reducers throw, you will see during which action this happened, and what the error was.
     5. With `persistState()` store enhancer, you can persist debug sessions across page reloads.

   
 70. ### How to add multiple middlewares to Redux?

     You can use `applyMiddleware()`.

     For example, you can add `redux-thunk` and `logger` passing them as arguments to `applyMiddleware()`:

     ```javascript
     import { createStore, applyMiddleware } from "redux";
     const createStoreWithMiddleware = applyMiddleware(
       ReduxThunk,
       logger
     )(createStore);
     ```

   
  71. ### What is an action in Redux?

     _Actions_ are plain JavaScript objects or payloads of information that send data from your application to your store. They are the only source of          information for the store. Actions must have a type property that indicates the type of action being performed.

     For example, let's take an action which represents adding a new todo item:

     ```
     {
       type: ADD_TODO,
       text: 'Add todo item'
     }
 
    ```

  72. ### What is the difference between React and Angular?

     Let's see the difference between React and Angular in a table format.

     | React                                                                    | Angular                                                       |
     | ------------------------------------------------------------------------ | --------------------------------------------------------------|
     | React is a library and has only the View layer                           | Angular is a framework and has complete MVC functionality     |
     | React handles rendering on the server side                               | AngularJS renders only on the client side but Angular 2 and   |            | React uses JSX that looks like HTML in JS which can be confusing         | Angular follows the template approach for HTML, which makes   |
     | React Native, which is a React type to build mobile applications are fast| Ionic, Angular's mobile native app is                         |
     | In React, data flows only in one way and hence debugging is easy         | In Angular, data flows both way i.e it has two-way data bindi |                                                                                      

    **Note:** The above list of differences are purely opinionated and it vary based on the professional experience. But they are helpful as base           parameters.

