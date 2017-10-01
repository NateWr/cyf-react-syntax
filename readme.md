# React Syntax

This document will help you understand the **syntax** and **terminology** of React. It introduces the words you will hear us use when describing different parts of the React system.

Read the code and the descriptions carefully to make sure you understand. You will need to know the following terms to pass your test next week:

- Component
- Smart Component
- Dumb Component
- Props
- Attributes
- State
- How the state is changed
- Functions
- JSX

## Components

In React, your entire app is broken down into **Components**. Some of these components may be very simple. Read the following `Button` component and try to understand how it works.

```js
import React from 'react';

function Button(props) {
	return <button>{props.text}</button>;
}

export default Button;
```

Some components may be more complex and control other components. Read the following `ContactForm` component and try to understand how it works.

```js
import React, { Component } from 'react';
import Button from './Button.js';

class ContactForm extends Component {

	constructor() {
		super();
		this.state = {
			fullName: ''
		};
	};

	changeFullName = (event) => {
		this.setState({
			fullName: event.target.value
		});
	};

	render () {
		return (
			<form>
				<input type="text" name="fullName" value={this.state.fullName} onChange={this.changeFullName}/>
				<Button text="Submit"/>
			</form>
		);
	}
}

export default ContactForm;
```

## Smart Components

We call the more complex components **Smart Components** because they manage state. That means that they can change the data in the application.

In the example above, `ContactForm` is a smart component because it is responsible for changing the value of `fullName`.

## Dumb Components

We call the simpler components **Dumb Components** because they do *not* manage state. That means that they can receive data, but they can't change it.

In the example above, `Button` is a dumb component because it is not responsible for changing any information.

## HTML Output

A React component always renders and displays HTML code. The `ContactForm` component above will render the following HTML:

```HTML
<form>
	<input type="text" name="fullName" value="" />
	<button>Submit</button>
</form>
```

### What should you know?

1. In the Components section above, we've written code for two components. Can you name the two components discussed?

2. Can you name the "smart" component?

3. Can you name the "dumb" component?

4. Do you understand how the `<button>Submit</button>` code was generated?

# Props

When a component receives data from its parent, we call that data **props**, which is short for "properties". Can you identify the **props** received by the `Button` component below?

```js
import React from 'react';

function MyButton(props) {
	let className = 'btn-default';
	if (props.isDisabled === true) {
		className = 'btn-disabled';
	}
	return <button className={className} disabled={isDisabled}>{props.text}</button>;
}

export default MyButton;
```

The `MyButton` component receives `isDisabled` and `text` props. These are passed to the button as `props`, and then used to render the HTML output.

But where do the `props` come from? They come from a *parent component*. Can you identify the values of the `props` being passed to the `MyButton` component in the parent component below?

```js
import React, { Component } from 'react';
import MyButton from './MyButton.js';

class ContactForm extends Component {

	render () {
		return (
			<form method="POST">
				<MyButton isDisabled={true} text="Complete"/>
			</form>
		);
	}
}

export default ContactForm;
```

When the `<MyButton>` component is created, we added two **props**. The `isDisabled` prop is set to `true` and the `text` prop is set to `Complete`.

The `<MyButton>` component receives these props as an object with the following values:

```js
{
	isDisabled: true,
	text: "Complete"
}
```

So we say the `<MyButton>` component has two props: `isDisabled` and `text`. The props with the values above will render the following HTML:

```HTML
<form>
	<button class="btn-disabled" disabled="disabled">Complete</button>
</form>
```

## What should you know?

1. In the `ContactForm` component, is `method` a **property** passed to the `MyButton` component?

2. What are the names of the **props** that the `MyButton` component receives from the `ContactForm`?

3. When the HTML is rendered, what will the value of `class` be in the `<button>` element if the `isDisabled` prop is set to false?

# State

**State** is another name for data in the application which might change. If **props** represent data that will never change in a component, **state** represents data in your component which may change.

Read the following `ContactForm` component and try to determine what data in the component is **state**:

```js
import React, { Component } from 'react';
import Button from './Button.js';

class ContactForm extends Component {

	constructor() {
		super();
		this.state = {
			fullName: '',
			emailAddress: '',
		};
	};

	changeFullName = (event) => {
		this.setState({
			fullName: event.target.value
		});
	};

	changeEmailAddress = (event) => {
		this.setState({
			emailAddress: event.target.value
		});
	};

	render () {
		return (
			<form>
				<input type="text" name="fullName" value={this.state.fullName} onChange={this.changeFullName}/>
				<input type="email" name="emailAddress" value={this.state.emailAddress} onChange={this.changeEmailAddress}/>
				<Button text="Submit">
			</form>
		);
	}
}

export default ContactForm;
```

There are two pieces of data in this component which are **state**. These are `fullName` and `emailAddress`.

We make this data **state** because it might change. When a visitor to the website enters their name or email address into the form, the data will change, and we will need to render the HTML code again.

Let's assume the value of our **state** is the following:

```
{
	fullName: 'Nate Wright',
	email: 'nate@example.org'
}
```

The component above will render the following HTML:

```HTML
<form>
	<input type="text" name="fullName" value="Nate Wright"/>
	<input type="email" name="emailAddress" value="nate@example.org"/>
	<button>Submit</button>
</form>
```

## Updating State

When **state** is updated, React needs to know so that it can render the new HTML code. Let's look at the following line from the above component:

```
<input type="text" name="fullName" value={this.state.fullName} onChange={this.changeFullName}/>
```

This is the form field where a visitor will type their full name. There are four **attributes** on this field: `type`, `name`, `value` and `onChange`. Let's look at the `onChange` attribute:

```
onChange={this.changeFullName}
```

`this.changeFullName` is a **function**. Take a moment to find the `changeFullName` function in the component code above. Whenever a visitor types their name into the field, the `changeFullName` function will be fired:

```
changeFullName = (event) => {
	this.setState({
		fullName: event.target.value
	});
};
```

The function uses `event.target.value` to get the new value of the field, and calls `this.setState` to update the `fullName` state.

In this way, we can convert actions that the user takes in the browser into changes in our application **state**.

## What should you know?

1. What pieces of data are part of the **state** in the above example?

2. In the following code, what are the variable types of the **attributes** `value` and `onChange`: `<input type="text" name="fullName" value={this.state.fullName} onChange={this.changeFullName}/>`? (If you don't remember what a "variable type" is, Google it.)

3. What code would we use to change the state of `emailAddress` to `any@example.org`?

# JSX and HTML

**JSX** is a special way of writing HTML. React uses this to mix JavaScript (like expressions: `someCondition ? 'true' : 'false'`) with HTML (like tags: `<p>`).

In a React component, you *write* JSX code in the `render` function. When you want to write JavaScript inside your JSX, you wrap it in curly brackets: `{}`.

```js
render () {
	return (
		<button>{this.props.text}</button>
	);
}
```

React takes the JavaScript expressions you put into those curly braces (`{` and `}`) and returns what they equal. If you have a variable, `let number = 5`, and you use `{number}` in your JSX code, it will replace `{number}` with `5`.

In additional variables, you can do other things inside the curly braces.  You can call functions (`{this.buildGame()}`), use ternary expressions (`isSelected ? 'true' : 'false'`) and more.

React will convert the JSX code into HTML elements. If you create a new React component, it will convert that component's JSX code into HTML elements and print that.

```
// props
let props = {
	text: 'Submit',
	id: 'submit-button'
};

// JSX code:
<button id={props.id.length ? props.id : ''}>{this.props.text}</button>

// Converted to HTML
<button id="submit-button">Submit</button>
```

It's important to understand the difference: JSX is a way of converting your application data (**state** and **props**) into HTML code, which the browser understands. When **state** changes, React will render the HTML code again.

## Attributes and Child Components

The terminology of JSX can be confusing, because **props** passed to components look just like **attributes** on HTML elements. Consider the following component:

```js
import React, { Component } from 'react';
import Button from './Button.js';

class TwoButtons extends Component {
	render () {
		return (
			<button class="btn-default">Submit</button>
			<MyButton className="btn-default" text="Submit"/>
		);
	}
}
```

This will render **two buttons** that are the same. Here is what the HTML looks like after it has been converted:

```HTML
<button class="btn-default">Submit</button>
<button class="btn-default">Submit</button>
```

So what's the difference?

In the render function, the first button is created with just basic HTML code. The second button loads a *new React component* named `MyButton`.

Let's break this down.

Here's the first line:

```HTML
<button class="btn-default">Submit</button>
```

This is plain HTML code. The HTML **tag** is `button` and it has one **attribute** named `class`.

Here's the second line:

```HTML
<MyButton className="btn-default" text="Submit"/>
```

This component is passed two **props** named `className` and `text`. These props are written to look like HTML attributes, but they **are not HTML attributes**. Instead, they are converted into **props** and passed to the `MyButton` component in an object like this:

```js
{
	className: 'btn-default',
	text: 'Submit',
}
```

The code for the `MyButton` component will receive the props like this:

```js
import React from 'react';

function MyButton(props) {
	return <button className={props.className}>{props.text}</button>;
}

export default MyButton;
```

How can you tell if you're rendering a HTML element or another React component? HTML elements **always** start with a lowercase letter (`<button>`), whereas React components start with a capital (`MyButton`). When we write **attributes** on a React component (`<MyButton text="Submit"/>`), we call them **props**.

## What should you know?

1. Does the browser understand JSX?

2. Do you write JSX or HTML in React components?

3. In the following code, which pieces of data are HTML **attributes** and which are **props** that will be passed to a child component?

```js
render () {
	return (
		<img src="/my-image.jpg">
		<input type="text" name="emailAddress">
		<Avatar userId={this.state.userId} size="sml"/>
	);
}
```

# Conclusion

The answers to all the questions can be found in the examples and descriptions above. If you're not able to answer any of the questions, read the section again carefully. Pay special attention to terms highlighted in **bold**. Search on Google if you are confused about anything.

For the test next week you should be able to read the following code and identify the following elements:

- Component
- Smart Component
- Dumb Component
- Props
- Attributes
- State
- How the State is changed
- Functions
- JSX

```js
import React, { Component } from 'react';
import UserCard from './UserCard.js';

class UserList extends Component {

	constructor() {
		super();
		this.state = {
			users: [],
		};
	};

	buildUserList() {
		let users = [];
		for (var i = 0; i < this.state.users; i++) {
			let userName = this.state.users[i].userName;
			let userImage = this.state.users[i].userImage;
			users.push(<User userName={userName} userImage={userImage}/>);
		}
		return users;
	};

	addUser() {
		let newUsers = this.state.users;
		newUsers.push({
			userName: 'Another User',
			userImage: '/user/image.jpg',
		});
		this.setState({
			users: newUsers
		});
	};

	render () {
		return (
			<div class="users">
				<h1>Users</h1>
				<div class="users-list">
					{this.buildUserList()}
				</div>
				<button onClick={this.addUser}>Add User</button>
			</div>
		);
	}
}

export default ContactForm;
```

```js
import React from 'react';

function User(props) {
	let className = 'btn-default';
	if (props.isDisabled === true) {
		className = 'btn-disabled';
	}
	return (
		<div>
			<img src={props.userImage}/>
			{props.userName}
		</div>
	);
}

export default Button;
```
