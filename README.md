# React Notes

## Table of Contents
- [Create React project](#link-part-1)
- [Start React project](#link-part-2)
- [React Syntax](#link-part-3)
- [React Class Component](#link-part-4)
- [React Function Component](#link-part-5)

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

### <a name="link-part-5">React Function Component - rfc</a>

