# Coding Guidelines - ReactJS



## Table of Contents

  1. [Basic Rules](#basic-rules)
  1. [Naming](#naming)
  1. [Declaration](#declaration)
  1. [Alignment](#alignment)
  1. [Quotes](#quotes)
  1. [Spacing](#spacing)
  1. [Props](#props)
  1. [Parentheses](#parentheses)
  1. [Tags](#tags)
  1. [Methods](#methods)
  1. [Ordering](#ordering)
  
  
  

## Basic Rules

- Only include one React component per file.
- Always use JSX syntax.
- Do not use `React.createElement` unless you're initializing the app from a file that is not JSX.



## Naming
- File- and component name need to be identical.
- Use PascalCase naming convention for filename as well as component name, e.g. GlobalHeader.js

```javascript
// Bad
// Filename: foo.js

class Foo extends React.Component {}

export default Foo;


// Good
// Filename: Foo.js

class Foo extends React.Component {}

export default Foo;
```



## Ordering

- Ordering for class extends React.Component:

1. constructor
1. optional static methods
1. getChildContext
1. componentWillMount
1. componentDidMount
1. componentWillReceiveProps
1. shouldComponentUpdate
1. componentWillUpdate
1. componentDidUpdate
1. componentWillUnmount
1. *clickHandlers or eventHandlers* like onClickSubmit() or onChangeDescription()
1. *getter methods for render* like getSelectReason() or getFooterContent()
1. *Optional render methods* like renderNavigation() or renderProfilePicture()
1. render

- How to define propTypes, defaultProps, contextTypes, etc...  

```javascript
import React, { Component, PropTypes } from 'react';

const propTypes = {
    id: PropTypes.number.isRequired,
    url: PropTypes.string.isRequired,
    text: PropTypes.string,
};

const defaultProps = {
    text: 'Hello World',
};

export default class Link extends Component {
    static methodsAreOk() {
        return true;
    }

    render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
    }
}

Link.propTypes = propTypes;
Link.defaultProps = defaultProps;
```


## Alignment
- Follow these alignment styles for JSX syntax

```javascript
// bad
<Foo superLongParam="bar"
     anotherSuperLongParam="baz" />

// good
<Foo
  superLongParam="bar"
  anotherSuperLongParam="baz"
/>

// if props fit in one line then keep it on the same line
<Foo bar="bar" />

// children get indented normally
<Foo
  superLongParam="bar"
  anotherSuperLongParam="baz"
>
    <Spazz />
</Foo>
```


## Quotes

- Always use double quotes (`"`) for JSX attributes, but single quotes for all other JS.

```javascript
// bad
<Foo bar='bar' />

// good
<Foo bar="bar" />

// bad
<Foo style={{ left: "20px" }} />

// good
<Foo style={{ left: '20px' }} />




## Props
- Always use camelCase for prop names.

```javascript
// bad
<Foo
    UserName="hello"
    phone_number={12345678}
/>

// good
<Foo
    userName="hello"
    phoneNumber={12345678}
/>
```



## Tags
- Always self-close tags that have no children.

```javascript
// bad
<Foo className="stuff"></Foo>

// good
<Foo className="stuff" />
```

- If your component has multi-line properties, close its tag on a new line.
```javascript
// bad
<Foo
    bar="bar"
    baz="baz" />

// good
<Foo
    bar="bar"
    baz="baz"
/>
```


## Stateless function components
For stateless components use the function syntax, introduced in React 0.14.

```javascript
// Using an ES2015 (ES6) arrow function:
var Aquarium = (props) => {
    var fish = getFish(props.species);
    return <Tank>{fish}</Tank>;
};

// Or with destructuring and an implicit return, simply:
var Aquarium = ({species}) => (
    <Tank>
        {getFish(species)}
    </Tank>
);

// Then use: <Aquarium species="rainbowfish" />
```

## PropTypes declarations

- Setting propTypes declarations is mandatory
- Group them into required/none-required
- Alphabetically sort each group
- Separate them by a new line

```javascript
static propTypes = {
    blank: React.PropTypes.bool.isRequired,
    block: React.PropTypes.bool.isRequired,
    size: React.PropTypes.string.isRequired,
    to: React.PropTypes.string.isRequired,
    disabled: React.PropTypes.bool,
};
```


## Prefixing none React methods
Prefix all none React methods within a component with an underscore.

```javascript
class Foo extends React.Component {

    componentDidMount() {
        this._update();
    }
    
    _update() {
        // e.g. update position
    }
    
    render() {
        return (
            <div>foo</div>
        );
    }
}   
```

## Prefixing component wide variables
In the exception that you do not want to place a component wide variables on the state, you have to prefix it with an underscore.

```javascript
class Foo extends React.Component {

    componentDidMount() {
        this._el = React.FindDOMNode(this.refs.foo);
    }
    
    render() {
        return (
            <div>foo</div>
        );
    }
}   
```


## Using handler methods

- Name methods using `'_handle' + triggering event`, e.g. `_handleClick`
- Bind handler using the ES6 arrow syntax, so inside the callback it has always the right context

```javascript
class Foo extends React.Component {

    _handleClick = (e) => {
        this.setState(
            {
                clicked: true
            }
        );
    }
    
    render() {
        return (
            <button onClick={this._handleClick}>Submit</button>
        );
    }
}
```



## Using “container” components for loading data from Stores


```javascript
// CommentListContainer.js

class CommentListContainer extends React.Component {
    constructor() {
        super();
        this.state = { comments: [] }
    }
    componentDidMount() {
        $.ajax({
            url: "/my-comments.json",
            dataType: 'json',
            success: function(comments) {
            this.setState({comments: comments});
            }.bind(this)
        });
    }
    render() {
        return <CommentList comments={this.state.comments} />;
    }
}



// CommentList.js

class CommentList extends React.Component {
    constructor(props) {
        super(props);
    }
    _renderComment({body, author}) {
        return <li>{body}—{author}</li>;
    }
    render() { 
        return <ul> {this.props.comments.map(_renderComment)} </ul>;
    }
}
```

Source: https://medium.com/@learnreact/container-components-c0e67432e005



## Closing Components without children

```javascript
render() {
    return (
        <Foo>
            <Bar />
        </Foo>
    );
}
```


## List iterations

When rendering a list of components from an array, do it inline if it makes sense. If the map function is too long or complicated, consider extracting it out into its own method on the component class.

```javascript
render() {
    return (
        <ul>
            {this.state.fooList.map(fooItem => <FooItem>{fooItem}</FooItem>)}
        </ul>
    );
}
```


## Formatting Attributes

```javascript
<input
    type="text"
    value={this.state.foo}
    onChange={this._handleInputChange.bind(this, 'foo')}
/>
```



## Inline CSS styles
Static properties should be set in the SCSS, dynamic ones in JS.

```css
.Foo {
    background-color: #ff0;
}
```

```javascript
class Foo extends React.Component {

    render() {
        
        const styles = {
            'transform': 'translateX(' + this.state.position + ' + px)'
        };
    
        return (
            <div className="Foo" styles={classes}>Foo Header</div>
        )
    };

}
```




## Use "classnames" to set CSS classes

Use the [classnames](https://www.npmjs.com/package/classnames) node module for setting CSS classes on an element.

```javascript
import React from 'react';
import classnames from 'classnames';

class Foo extends React.Component {

    render() {
        
        const classes = classnames('FooHeader', {
            'is-fixed': this.state.fixed,
            'is-visible': this.state.visible
        });
    
        return (
            <div className={classes}>Foo Header</div>
        )
    };

}
```

1. Variable
const activeAdmin = user.active && (user.role === "admin");

if (activeAdmin){
// ...
}

2. Helper Method

// somewhere in the helper methods file or at the top if only used in one file
const canAttemptLogin = ({active, noOfAttempts, lastLoginAttempt}) => {
  return active && noOfAttempts > 3 && (Date.now() - lastLoginAttempt >= 60000;
}


// inside your component
{
  canAttemptLogin(user) ? <div>You can log in</div> : <div>You are not allowed to login. Please try again after some time.</div>
}

3.CONST Variables for Repeated String Literals
const SCHOOL = "school";
if(user.instituteType === SCHOOL) {
}
// ...
const schoolGoing = users.map(u=>u.instituteType === SCHOOL);
// ...
{
    user.instituteType === SCHOOL? <div>School</div>: null
}

4.Destructuring
import {forEach, filter} from "lodash";

// ....

forEach(
  //...
)
filter(
  //...
)

5.Props value passing
const UserInfo = ({
  firstName,
  lastName
}) =>
(<div>
  <div>
    First Name: {firstName}
  </div>
  <div>
    Second Name: {secondName}
  </div>
</div>)

6.Naming Conventions - Component’s names should be written using pascal case:
Header.js
HeroBanner.js
CookieBanner.js
BlogListing.js

7.Naming Conventions - Non-components should be written using camel case:
myUtilityFile.js
cookieHelper.js
fetchApi.js

8.Naming Conventions - Unit test files should use the same name as its corresponding file:
CookieBanner.js
CookieBanner.test.js

fetchData.js
fetchData.test.js

9.Naming Conventions - Attribute name should be camel case:
className
onClick

10.Naming Conventions - Inline styles should be camel case:
<div style={{font-size:'1rem'}}></div>

11.Naming Conventions - Variable names should be camel case. Variable names can contain number and special characters:
const variable = 'test';
let variableBoolean = true;

12.Naming Conventions - CSS files should be named the same as the component:
CookieBanner.css
Header.css

13.Bug Avoidance
Use optional chaining if things can be null
Use the guard pattern/prop types/typescript to ensure your passed in parameters are valid
Create PURE functions and avoid side-effects
Avoid mutating state when working with arrays
Remove all console.log()
Treat props as read-only. Do not try to modify them

14.Putting imports in an order
React import
Library imports (Alphabetical order)
Absolute imports from the project (Alphabetical order)
Relative imports (Alphabetical order)
Import * as
Import ‘./<some file>.<some extension>
Each kind should be separated by an empty line. This makes your imports clean and easy to understand for all the components, 3rd-party libraries, and etc.

15.Code
Play the game of HOOKS by following all its rules. You can read more about the rules here. 
Either manually follow these rules or use an ESLint plugin called eslint-plugin-react-hooks which enforces these rules. So, add these rules while working on any project
Remove console. logs — unless you have strong motivation why you would like it.
Avoid multiple if-else blocks. Instead, use ternary — best for clean code practice
Remove all commented-out codes. The biggest motivation for writing comments is the bad code that you write. It would be good to spend more time writing descriptive functions, methods, and filenames that are self-explanatory.
Write Tests for each component. It's a good practice to write test cases for each component developed as it reduces the chances of getting errors when deployed. You can check all the possible scenarios through unit testing — and for that, some of the most commonly used React test frameworks you can use are JEST and ENZYMES.


16.Use JSX ShortHand
Try to use JSX shorthand for passing boolean variables. Let’s say you want to control the title visibility of a Navbar component.
return(
  <Navbar showTitle />  
)


17.Use Ternary Operators
Let’s say you want to show a user’s details based on role.
const { role } = user;
return role === ADMIN ? <AdminUser /> : <NormalUser />






























