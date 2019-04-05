---
title: "React.js Notes"
date: "2018-09-06T18:59:03-04:00"
draft: "false"
author: Shaquille
comments: true
tags: ["React", "JavaScript"]
categories: ["code", "tutorial"]
description: "Just some rough notes from a react youtube tutorial"
---


<center>
[![Learn React - React Crash Course ](https://img.youtube.com/vi/Ke90Tje7VS0/0.jpg)](https://www.youtube.com/watch?v=Ke90Tje7VS0 "Learn React | Programming with Mosh")
</center>

- A react application consists of components
	- Components are independent, isolated and resuable.
	- Each component is a piece of UI.
- Every react application has at least one component - the root.
- React applications are represented as a tree of components.
- Components are usually implemented as js class that consists of a `state` and a `render()` method.
	- The `state` is the data we want to display when the component is rendered
	- The `render` method describes what the UI should look like.
		- The output of this method is a *React Element* which is a simple javascript object which that maps to a *DOM element*
		- This is a lightweight representation of the DOM in memory - this is referred to as the *Virtual DOM*. This is cheap to create, unlike the real DOM
		- React updates the DOM automatically when a state changes. You no longer have query the DOM and manipulate it yourself.

<br/>

- `create-react-app` installs react and all third part libraries we'll need:
	- Lightweight dev server
	- webpack for bundling our files
	- babel for compiling js code
	- etc.
- To customise this config for prod envs, you can run `npm run eject`
- Babel takes jsx syntax and compiles it to javascript code that browsers can then understand

<br/>

- We can set properties in a class that can then be referenced inline by jsx syntax using curly braces `{this.someProperty}`.
	- Generally speaking, we can exceute any regular javascript expression within curly braces

- To set classes as we do in html, we use the `className` attribute instead of `class`:
`<button className="btn btn-secondary btn-sm">Increment</button>`
- In React, `state` is a special property in a component that includes any data that the specific component needs:

```javascript
class Counter extends Component {
  state = {
    count: 0,
    tags: ["tag1", "tag2", "tag3"]
  };

  render() {
    return (
        <button className="btn btn-secondary btn-sm">Increment</button>
    );
  }
}
```
<br/>

- We have to pass a reference to a function in order to handle events, rather than calling a function itself like we would in html:
	- Like this `onClick={this.handleIncrement}`
	- Not like this `onClick={this.handleIncrement()}`

<br/>

### What happens when we call: `this.setState`
- This tells react that the state of this component is going to change
- React will then schedule an asynchronous call to the `render()` method
- React will then do a `diff` on the old virtual DOM and the new virtual DOM (with state change) and figure out what elements in the virtual DOM are modified
- Once it's figure this out, it will reach out to the actual browser DOM and update the element that has had a state change so that the new virtual DOM and the the actual DOM are matching.

</br>

- Attributes set inside of a component tag like `Count` are passed to our Component using a single js object called `props`
- In order to pass data between closing tags of a component, we use the `children` property

### What is the difference between `props` and `state`?
- `props` includes data that we give to a Component
	- properties of the `props` object are **read-only**
- `state` includes data that is private to that Compoenent
	- other Components do not have access to this state

<br/>

- *The component that owns a piece of the state should be the one modifying it.*
- Thus, the best practice in React is as follows:
	- If a component `a` consists of another component `b`, then if `a` wants to mnodify a state that `b` owns, `a` should *raise* an event for component `b` to *handle*.

<br/>

- Components go through phases during their lifecycles:
    1. **Mounting Phase** - when an instant of a component is created and inserted into the DOM
    	- During this phase, there are a few special methods that we can act for react to automatically call these
    	- They are called lifecycle hooks
    		- They allow us to hook into a certain moment during the lifecylce of the component to do something.
    	- We have: `constructor`, `render`,  `componentDidMount`
    		- `constructor` is called only once when an instance of that component is created.
    		- `componentDidMount` is called after the component is rendered into the DOM
    			- This is a good place to make ajax calls to get data from a server
    		- React will cause these methods in order
    2. **Update Phase** - this happens when the state or the props of a component change
    	- This has two commonly lifecycle hooks: `render`, `componentDidUpdate`
    		- Again, these two methods are called in order by React
    3. **Unmounting phase** - this is when a component is removed from the DOM
    	- Like deleting a counter, for instance
    	- This has a single commonly used hook: `componentWillUpdate`
