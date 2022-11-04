# React Notes

## Table of Contents
- [Create React project](#link-part-1)
- [Start React project](#link-part-2)
- [React Syntax](#link-part-3)
- [React Class Component](#link-part-4)
- [React Function Component](#link-part-5)
- [Hook](#link-part-6)

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

#### useState

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

#### useEffect

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
