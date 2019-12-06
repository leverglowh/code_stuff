# React and JSX

### JSX in development mode:

Import React and ReactDOM and babel to the webpage:

```react
<script src="https://unpkg.com/react@16/umd/react.development.js" crossorigin></script>
<script src="https://unpkg.com/react-dom@16/umd/react-dom.development.js" crossorigin></script>
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<script type="text/babel">
```

And this is a JSX expression:

```react
const element = <h1>Hello, world!</h1>
```

##### Embedded expressions:

Putting valid JavaScript expression inside *curly braces* in JSX:

```react
const name = 'Karen';
const element = <h1>Hello, {name}</h1>; //variable

function changeName(name){
    return 'new'+name;
}
const element2 = <h1>Hello, {changeName(name)}</h1> //function
```

##### JSX in JS expressions:

`if` statements, `for` loops, assigned to *variables*, passed as *argument*, *returned* from functions...

```react
function sayHello(user) {
    if (user) {
        return <h1>Hello, {user}!</h1>; //as returned value
    }
    return <h1>Hello, Stranger!</h1>;
}
```

##### Specifying attributes in JSX

```react
const element = <div tabIndex="0"></div>; //"quotes"!
const element = <img src={user.avatarUrl}></img> //{JS}!!!

const element = (
    <div>
    	<h1>I'm a child!!!</h1>
    </div>
);
```

##### React.createElement() in JSX

All these expressions are equivalent:

```react
const element = ( 						//JSX
	<h1 className="greeting"> 
    	Hello, world!
    </h1>
);

const element = React.createElement(	//React
	'h1',
    {className: 'greeting'},
    'Hello, world!'
);

const props = {
    className: 'container',
    children: 'Hello World'
}
const element = (						//JSX with props obj
	<div {...props} /> //Exploding props
)
```
##### Default and overriding props:

The *last* attribute always wins:

```react
const props = {
    className: 'container',
    children: 'Hello World'
}

const element = ( 
	<div 
        className="default className" 
        {...props} 
        children='must be this children'/>
) 
//className = 'container', children = 'must be this children'
```

##### Creating my own component

```react
const Message = (props) => <div>{props.msg}</div> //Component: CamelCase
const element2 = (
	<div className="second-class">
    	<Message msg='Hello World'/>
        <Message msg='Hello Second World'/>
    </div>
)

class element extends React.Component {
    render() {
        //do something...
    }
}
ReactDOM.render(element, rootElement);
ReactDOM.render(element2, rootElement);
```

##### Validating prop types

```react
<script src="https://unpkg.com/prop-types@15.6/prop-types.js"></script>
```

```react
//When it's a function
function sayHello(props) {
    return (
    	<div>
        	Hello {props.firstName} {props.lastName || 'Unknown'}!
        </div>
    )
}
sayHello.propTypes = {
    firstName: PropTypes.string.isRequired,
    lastName: PropTypes.string,
}
//When it's a class
class SayHello extends React.Component {
    static propTypes = {
        firstName: PropTypes.string.isRequired,
    	lastName: PropTypes.string,
    }
	render() {
        //do something...
    }
}
```

##### Lifecycle

```react
shouldComponentUpdate(nextProps, nextState){
    return nextProps.value == 7; //re-render only when the value prop equals 7
}
```

##### Conditionally render a component

 ```react
//Using if
function Message(props) {
    if (!props.message) {
        return <div>No Message</div>
    }
    return <div>{props.message}</div>
}
//Using proper JSX
function Message(props) {
    return (
    	<div>
        	{props.message ? 
            (<div>{props.message}</div>) :
            (<div>No Message</div>)}
        </div>
    )
}
//or using logical &&
function Message(props) {
    return (
    	{isOkToPrint &&
         <h1>Print!!!</h1>
        }
    );
}

reactDOM.render(
	<Message />, 							//No message
    document.getElementById('root'),
)
reactDOM.render(
	<Message message="new message!"/>, 		//New message!
    document.getElementById('root'),
)
 ```

##### To prevent some component from rendering:

```react
function saySomething(props) {
    if (!props.ok){
        return null; //return null from function to not render anything
    }
    
    return (
    <div className="something">Something</div>
    );
}
```

##### Inline styling

```react
function Box({style, className='', ...rest}) {
    return (
    	<div 
            className={`box ${className}`}
            style={{"paddingLeft": 20, "fontSize":50, ...style}}
            {...rest>
            box
        </div>
    )
}
const element = (
	<div>
    	<Box className="box--small" style={{backgroundColor: 'lightblue'}}>
        	small box
        </Box>
    </div>
)
```

##### Event handling:

To prevent default behavior:

```react
function handleClick(e){
    e.preventDefault();
}
```

```react
//The caller provides the onClick function
function butt(props){
    return (
    	<button onClick={props.onClick}>butt!!</button>
    )
}
function base(){
    function handleClick(){
        return <div>Clicked!!</div>
    }
    return (
    	<butt onClick={()=>this.handleClick()}/>
    )
}

//The callee has the click handle function
function butt(props){
    return (
    	<button onClick={()=>{return <div>Clicked!!</div>}}>butt!!</button>
    )
}
```

##### Add new event listener

```react
componentDidMount(){
    document.addEventListener('keydown', this.handleKeyPressed);
}

componentWillUnmount(){
    document.removeEventListener('keydown', this.handleKeyPressed);
}
```

Events keywords are between quotes, a list of them can be found [Here](https://www.w3schools.com/jsref/dom_obj_event.asp).

##### Functions in a JSX object

```react
class Counter extends React.Component {
    count(){ 		//needs binding in constructor: this.count = this.count.bind(this);
        //do something...
    }
    counting = () => { //Doesn't need binding!
        //do something
    }
}
```

For unbound functions, `this` won't work properly.

##### Public class fields

```react
class Counter extends React.Component {
    state = {counted:0}
	this.count = this.count.bind(this);

	count(){}			//yes bind 
	yell = () => {}		//no Need to bind!!
}
```

##### Lists and Keys

Rendering multiple components from ***array***:

```react
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number)=>
	<li>{number}</li>                           
);
ReactDOM.render(
	<ul>{listItems}</ul>,
    document.getElementById('root')                      
);                         
```

A key is a special string attribute for lists of elements, when I use `.map()`:

```react
const listItems = numbers.map((number)=> //string attribute!!
	<li key={number.toString()}> 
    	{number}
    </li>                           
);
```

To identify which items have changed and thus to decide which ones to re-mount, React uses `keys`. If I *really* don't know what to set as key, use the indexes (*very* poor choice).

Keys are not accessible from the elements, if I need to use them, I'll have to pass them as properties.

```react
const content = posts.map((post) =>
  <Post
    key={post.id} 			//not accessible
    id={post.id}			//Accessible!!
    title={post.title} />
);
```

### Forms

```react
class NameForm extends React.Component {
    constructor(props){
        super(props);
        this.state={value: ''};
    }
    
    handleChange = (event) => {
        this.setState({value: event.target.value});
    }
    
    handleChangeToUpperCase = (event) => {
        this.setState({
            value: event.target.value.toUpperCase() //when using nothing
        });
    }
    
    handleSubmit = (event) => {
        alert(event.target.elements.username.value); //when using username prop
        console.log(this.inputNode.value); //when using ref
        event.preventDefault(); //Otherwise it refreshes the whole page
    }
    
    render(){
        return(
        	<form onSubmit={this.handleSubmit}>
            	<label>
                	Name: 
                    <input 
                        type="text" 
                        name="username" 
                        ref={node => (this.inputNode = node)} 
                        value={this.state.value} 
                        onChange={this.handleChange}/>
                </label>
                <input disabled="disabled" type="submit" value="Submit" />
                <p></p> //New line space
        		<div>
                    <label>
                        {`You are saying: ${this.state.value}`}
                    </label>
                </div>
            </form>
        )
    }
   
}
```
If I don't put `onChange` prop but I have a `value` prop, it becomes ***readOnly***.

```react
//Text input box (single line)
<input type="text" value={this.state.value} onChange={this.handleChange} />
//Text input box (big square) 
<textarea value={this.state.value} onChange={this.handleChange} />
//Drop-down list!
<select value={this.state.value} onChange={this.handleChange}>
	<option value="value1">Value 1</option>
    <option value="value2">Value 2</option>
    <option value="value3">Value 3</option>
    <option value="value4">Value 4</option>
</select>
```

### HTTP Requests

wip

### React Hook

##### Add state to functions

```react
function counter(){
    const [countOne, setCountOne] = useState(0); //first state var, initial value = 0
    const [countTwo, setCountTwo] = useState(0); //second var
    //I can call the set method in "return"
    return (
    	<div>
        	The total count is {countOne+countTwo}
            <button onClick={()=> setCountOne(count+1)}> Add one </button>
        </div>
    )
}
```

!!! If the argument of the function `setCount` isn't a *new* reference (slice array, new variable etc.), the DOM won't be re-rendered at once.

##### Do action after render

```react
function counter(){
	const [count, setCount] = useState(0);
    useEffect(()=>{ 
        //after every rendering
        document.title = `${count}`;
        //on unmount
        return()=>{ 
        	document.title = 'The end';
        };
    });
    return(...);
}
```

### Render to server

```react
ReactDOMServer.renderToString(<App />);
```

Server pre-rendered content has many vantages:

- It *can* be crawled by the search engine -> **more visibility**
- It can speed up the loading process -> **speed**

React.PureComponent 