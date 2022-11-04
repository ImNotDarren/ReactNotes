# React Notes

## Table of Contents
- [Create React project](#link-part-1)
- [Start React project](#link-part-2)
- [React Syntax](#link-part-3)
- [React Class Component](#link-part-4)
- [React Function Component](#link-part-5)
- [Hook](#link-part-6)
- - [useState](#link-part-6-1)
- - [useEffect](#link-part-6-2)
- - [createContext](#link-part-6-3)
- - [useContext](#link-part-6-4)
- [Controlled Components and Uncontrolled Components](#link-part-7)
- [memo, useCallback, useMemo](#link-part-8)
- [React Redux](#link-part-9)
- [React Router](#link-part-10)
- - [BrowserRouter](#link-part-10-1]


### <a name="link-part-1">Create React project</a>
```
$ npx create-react-app ${app_name}
```

### <a name="link-part-2">Start React project</a>
```
$ npm start
```

### <a name="link-part-3">SReact Syntax</a>
- File suffix js or jsx
- Component name: first letter capital
- html inside js: ( )
- js inside html: { }
- "export default" can be before class declaration.

### <a name="link-part-4">React Class Component - rcc</a>

```jsx
export default class App2 extends Component {
    state = {
        num: 1
    }

    render() {
        return (
        <div>
            <h2>Number: {this.state.num}</h2>
            <button onClick={()=>this.setState({num: this.state.num + 1})}>add</button>
        </div>
        )
    }
}
```

Use state -> setState to update page

#### Other format of onClick function (bind `this`)
```jsx
export default class App2 extends Component {

    constructor(props){
        super(props)
        this.state = {
            num: 1
        }
        // bind this in constructor
        this.minusNum = this.minusNum.bind(this)
    }

    render() {
        return (
        <div>
            <h2>Number: {this.state.num}</h2>
            <button onClick={()=>this.setState({num: this.state.num + 1})}>add</button>
            <button onClick={this.minusNum}>minus</button>
            // bind this here
            <button onClick={() => this.setZero()}>zero</button>
        </div>
        )
    }

    minusNum(){
        this.setState({
            num: this.state.num - 1
        })
    }

    setZero(){
        this.setState({
            num: 0
        })
    }
}
```

#### Pass parameters to function

```jsx
<button onClick={this.btnClick.bind(this, 1)}>button1</button>
```

define label class in jsx:
```jsx
// className instead of class
<div className='box'>something</div>
```

### <a name="link-part-5">React Function Component - rfc</a>

```jsx
const FunctionApp = () => {
    return <h2>App</h2>
}

export default FunctionApp
```
There's no lifecycle or state in RFC.

### <a name="link-part-6">Hook</a>

#### <a name="link-part-6-1">1. useState</a>

Use `useState` as the `setState` function in React class component.

`useState` has two outputs: the varible and the function to modify it.

```jsx
function Hook(){
    // Hook can only be at the very beginning of the function
    const [num, setNum] = useState(1)

    const fn = () => {
        setNum(num + 1)
    }

    return (
        <>
            <h2>{num}</h2>
            <button onClick={fn}>add</button>
        </>
        
    )
}

export default Hook
```

#### <a name="link-part-6-2">2. useEffect</a>

`useEffect` can simulate a lifecycle.

**Mounted:**

```jsx
useEffect(()=>{
    console.log('mounted')
})
```

**Inspect varible update:**

- To inspect all varibles, remove the second parameter (the array).
- To inspect none varible, pass in an empty array.

```jsx
useEffect(()=>{
    console.log('num1 updated')
}, [num1])
```

**beforeDestory/Garbage Collection**

```jsx
useEffect(()=>{
    return () => {
        console.log('destory')
    }
})
```

#### <a name="link-part-6-3">3. createContext</a>

Initialize a context first:

```jsx
const NumContext = createContext()
```

Top component:

```jsx
export default function AppContext() {
    const [num, setNum] = useState(123)
    return (
        <NumContext.Provider value={{num, setNum}}>
            <Father/>
        </NumContext.Provider>
    )
}
```

Father component:

```jsx
const Father = () => <Child/>
```

Child component:

```jsx
function Child(){
    return (
        <NumContext.Consumer>
           {
                ({num, setNum}) => (
                    <>
                        <h2>{num}</h2>
                        <button onClick={()=>setNum(456)}>change</button>
                    </>
                )
           }
        </NumContext.Consumer>
    )
}
```


#### <a name="link-part-6-4">4. useContext</a>

Get `num` and `setNum` from top component using `useContext`.

The only difference is in the child component:

```jsx
function Child(){
    // get num and setNum from top component using useContext
    const {num, setNum} = useContext(NumContext)
    return (
        <>
            <h2>{num}</h2>
            <button onClick={()=>setNum(456)}>change</button>
        </>
    )

}
```

### <a name="link-part-7">Controlled Components and Uncrontrolled Components</a>

```jsx
export default function App5() {
        const [value, setValue] = useState("asd")
        const inputChange = (e) => setValue(e.target.value)
        const element = useRef(null)

    return (
        <div>
            <h3>Controlled Components：</h3>
            <input type="text" value={value} onChange={inputChange} />
            <button onClick={()=>console.log(value)}>get input</button>
            <hr/>
            <h3>Uncontrolled Components：</h3>
            <input type="text" ref={element}/>
            <button onClick={()=>console.log(element.current.value)}>get input</button>
        </div>
    )
}
```

### <a name="link-part-8">memo, useCallback, useMemo</a>

When using `memo`, child component won't reload when reloading father component.

Use `useCallback`:

```jsx
const Child = memo((props) => {
    console.log(123)
    return <button onClick={()=>props.doSth()}>add</button>
})

export default function App6() {
    const [num, setNum] = useState(1)

    const doSth = useCallback(() => {
        setNum((num) => num + 1)
    }, [])

    return (
        <div>
            <h3>Number: {num}</h3>
            <Child doSth={doSth}/>
        </div>
        
    )
}
```

Use `UseMemo`:

```jsx
const Child = memo((props) => {
    console.log(123)
    return <button onClick={()=>props.doSth()}>add</button>
})

export default function App6() {
    const [num, setNum] = useState(1)

    const doSth = useMemo(() => {
        return ()=>setNum(num=>num+1)
    })

    return (
        <div>
            <h3>Number: {num}</h3>
            <Child doSth={doSth}/>
        </div>
        
    )
}
```
### <a name="link-part-9">React Redux</a>

Official website: https://react-redux.js.org

Install:
```
$ npm i redux react-redux
```

Create  a `store` folder under `/src`.

Create two files - index.js and reducer.js under `/src/store`.

Create a initial state in `reducer.js`, and export a function:

```js
const defaultState = {
    num: 1
}

// export a function
export default (state=defaultState, action) => {
    let newState = JSON.parse(JSON.stringify(state)) // deepcopy

    switch(action.type){
        case "addNum":
            newState.num += action.value
            break

        default:
            break
    }

    return newState
}
```

Create a store and export it in `index.js`

```js
// import reducer
import reducer from "./reducer";

// create store
import { legacy_createStore } from "redux";

const store = legacy_createStore(reducer)

// export store
export default store
```

Add a provider to top component:

```js
root.render(
    <Provider store={store}>
        <App8/>
    </Provider>
)
```

Add a connector to the component that you wanna use the varible:

```js
function App8(props) {
  return (
      <>
          <div>Number: {props.num}</div>
          <button onClick={()=>props.add()}>add</button>
      </>
  )
}

const mapStateToProps = (state) => {
    return {
        num: state.num
    }
}
// dispatch an action to reducer
const mapDispatchToProps = (dispatch) => {
    return {
        add(){
            const action = {
                type: "addNum",
                value: 2
            }
            dispatch(action)
        }
    }
}

export default connect(mapStateToProps, mapDispatchToProps)(App8)
```

### <a name="link-part-10">React Router</a>

Official website: https://reactrouter.com

Install:
```
$ npm install react-router-dom@6
```

#### <a name="link-part-10-1">1. BrowserRouter</a>

Store all the pages in `/src/pages`. (All pages must export at the end)

For example:
```jsx
function Home() {
  return (
    <h2>Home</h2>
  )
}
export default Home
```

Create a new folder `/src/router`, then create a router file `index.jsx`.

```jsx
// react-router-dom有两种模式：BrowserRouter(History模式)，HashRouter(Hash模式)
// 定义一个路由
const BaseRouter = () => (
    <BrowserRouter>
        <Routes>
            <Route path="/" element={<App9 />}>
                <Route path="/home" element={<Home />}></Route>
                <Route path="/list" element={<List />}></Route>
                <Route path="/detail" element={<Detail />}></Route>
            </Route>
        </Routes>
        
    </BrowserRouter>
)

export default BaseRouter
```

Then, add the router to `/src/index.js`:

```jsx
const container = document.getElementById('root')
const root = createRoot(container)

root.render(
    <BaseRouter />
)
```

Lastly, add `<outlet />` label to root page (/src/App9.jsx):

```jsx
export default function App9() {
  return (
    <div>
        <h3>App9</h3>
        <Outlet />
    </div>
  )
}
```


