# react-recap

### React life cycle

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


