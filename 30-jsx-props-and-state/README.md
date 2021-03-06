JSX, Props, and State
=====================

## Summary of this morning's Lecture

* **Component**
  * High level understanding => a reusable part of our UI.
  * _Single Responsibility Principle_
  * You can have containers to group components (aka: _Container Components_)
* **Element**
  * JavaScript Object
  * (lightweight) representation of what to render
  * important for the Virtual DOM
* **JSX**
  * transpiled by _Babel_
  * `React.createElement` => Element (JavaScript Object)
* **Thinking in React**
  * Mock
  * Step 1: Break UI into Component Hierarchy
  * Step 2: Build a Static Version (JSX + Mock Data)

```
App
  Forms
    ListForm
    TaskForm
  Lists
    ListContainer
      Task
```


## Outline

* **Environment Setup**
  * _yarn_
  * _create-react-app_
* **JSX**
  * we know what it is, let's play with it
  * attributes => camelCase
* **Use JSX to make Components**
  * a component is like a reusable function
  * all they need are props
* **props**
  * one way data flow!
* **state**
  * _immutability_
  * Step 3: Identify The Minimal (but complete) Representation Of UI State
  * Step 4: Identify Where Your State Should Live
* **manipulating state**
  * Step 5: Add Inverse Data Flow


## Lecture Notes

### Pre-JSX

- We left off with JSX at **Step 2: Build a Static Version (JSX + Mock Data)**.
- Before continuing with JSX, a recap:
  - JSX **describes** what we will see in a _declarative_ manner.
  - JSX is not magic. We saw that it was just an object.
  - This object is what React calls an _element_ and it too describes what React will _render_ in our root node.
- Still before continuing:
  - **Step 2** is the _first big conceptual jump_!!!
  - We created our **component hierarchy**.
  - We _don't_ know what a _component_ is.

**Component** ([as React describes it](https://reactjs.org/docs/components-and-props.html))

1. Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.
2. Conceptually, components are like JavaScript functions. They accept arbitrary inputs (called **“props”**) and **return React elements** _describing_ what should appear on the screen.

- **Point 1:** Great! We did this in **Step 1** and understand it (single responsibility principle).
- **Point 2:** We see them say, _"return React elements"_. We know what an _element_ is! It's _JSX_!
  - Components return (aka: render) JSX (or null if nothing is meant to be rendered).
  - We don't know what **props** are though. That will be clarified once we learn about JSX.

**Summary:** Components are reusable pieces of our UI that return (render) JSX. You can think of them as legos. Some are the same, some are different. They can be a single lego or a block of them. They are the building blocks of your UI.


### JSX

**Recap:** We now know that JSX is what a _component_ returns. We also saw earlier that JSX is just _syntactic sugar_ for a React method that returns an object. This object is an element and is a description that will be used by React to figure out what to render to the DOM.

```javascript
// This:
ReactDOM.render(
  <h1>Hello, world!</h1>,
  document.getElementById('root')
);
// is this:
'use strict';

ReactDOM.render(React.createElement(
  'h1',
  null,
  'Hello, world!'
), document.getElementById('root'));

// And this:
React.createElement(
  'h1',
  null,
  'Hello, world!'
)
// returns an object that looks like this:
{
  $$typeof: Symbol(react.element),
  type: "h1",
  key: null,
  ref: null,
  props: {
    children: "Hello, world!"
  }
}
```

If you give this object over to `ReactDOM.render`, it will be able to understand it and render the same output to the DOM.


#### JSX Deep Dive

This is a rehash of the notes from [Intro to React](https://github.com/learn-co-students/nyc-web-031218/tree/master/29-intro-to-react). Look to those notes for:
- JSX syntax
- Attribute usage
- `{ }` expressions
- Children (nesting JSX)

Here are the key points to JSX:
1. Looks like HTML.
2. What would have been HTML **attributes** would be _camelCased_ in JSX.
3. They can have 0 or more _children_.
4. These _children_ can be:
  - more JSX
  - the evaluated result of expressions inside `{ }`
    - **Note!** These results can be JSX too!
5. Lowercase JSX tags are assumed to be HTML tags.
6. Captialized JSX tags are assumed to be **components** that we define ourselves.
  - We're getting closer to fully understanding components!

```javascript
// This is JSX:
// It has no attributes.
// It has no children.
<div/>

// This too is JSX:
// It has no attributes.
// It has one child: "Hello World!"
<h1>Hello World!</h1>

// This too is JSX:
// It has 1 attribute: href, whose value is: "http://learn.co"
// It has one child: "Learn"
<a href="http://learn.co">Learn</a>

// This too is JSX:
// It has no attributes.
// It has 1 child: <h1>
<div>
  <h1>Hello World!</h1>
</div>

// This too is JSX:
// It has no attributes.
// It has 2 children: <h1> and <p>
// These children are called siblings since they are all of the same depth.
<div>
  <h1>Hello World!</h1>
  <p>I like React</p>
</div>

// This too is JSX:
// It has no attributes.
// It has 2 children: <h1> and <p>
// <span> is the child of the JSX <p>
// <span> is not a sibling of <h1> or <p> because it's deeper in the tree.
<div>
  <h1>Hello World!</h1>
  <p>I like <span>React</span></p>
</div>
```


#### JSX, (Functional) Components, and Props

At this point, you may be thinking that JSX _are_ components. You aren't entirely off there.
- Follow the comments in the code below and keep in mind two things:
  - JSX attributes
  - JSX children
- It will illustrate what a **component** is.

```javascript
// These can all be used here:
ReactDOM.render(
  /* code will go here */,
  document.getElementById('root')
);

// JSX
<a href="http://learn.co">Learn</a>

// JSX assigned to a variable (link):
const link = <a href="http://learn.co">Learn</a>
ReactDOM.render(
  link,
  document.getElementById('root')
);

// JSX returned from a function:
function link() {
  return <a href="http://learn.co">Learn</a>
}
ReactDOM.render(
  link(),
  document.getElementById('root')
);

// Component that returns JSX:
// Since this is a function, it's called a Functional Component.
// It must take one argument: props
function Link(props) {
  return <a href="http://learn.co">Learn</a>
}
ReactDOM.render(
  Link(),
  document.getElementById('root')
);
// This is not interesting though.
// We aren't using the component.
// We are just doing the same thing as returning JSX from a function.
// This is how we use components:
ReactDOM.render(
  <Link />,
  document.getElementById('root')
);

// So far not interesting.
// The power of components comes from it's declarative nature.
// We tell our Link component information through props:
function Link(props) {
  return <a href={props.url}>{props.name}</a>
}
// How do we get information into props?
// Like this:
ReactDOM.render(
  <Link url="http://learn.co" name="Learn" />,
  document.getElementById('root')
);
// Notice how url and name are attributes of our JSX.
// Notice how they can be accessed in our component, Link, via props.url and props.name.
// This makes our components reusable, customizable, and keeps things descriptive:
ReactDOM.render(
  <div>
    <Link url="http://learn.co" name="Learn" />
    <Link url="http://google.com" name="Google" />
  </div>,
  document.getElementById('root')
);

// This is another way we could have written our component:
// Notice how our JSX, Link, has one child: "Learn"
// Notice how we can access the JSX's children through props.children.
function Link(props) {
  return <a href={props.url}>{props.children}</a>
}
ReactDOM.render(
  <Link url="http://learn.co">Learn</Link>,
  document.getElementById('root')
);

// Let's see how this works with multiple children by making an App component:
// We're now one step closer to seeing the true power of components!
function App(props) {
  return <div>{props.children}</div>
}
function Link(props) {
  return <a href={props.url}>{props.children}</a>
}
ReactDOM.render(
  <App>
    <Link url="http://learn.co">Learn</Link>
    <Link url="http://google.com">Google</Link>
  </App>,
  document.getElementById('root')
);

// Lastly, as we know, JavaScript has arrow functions.
// Functional Components can also look like this:
const Link = (props) => {
  return <a href={props.url}>{props.name}</a>
}
ReactDOM.render(
  <Link url="http://learn.co" name="Learn" />,
  document.getElementById('root')
);

// The gotchas with functional components of both kinds is that you may see people using destructuring:
// You don't see props anywhere!!
// But remember, that props are being passed in.
// We're just using destructuring to get access to url and name directly.
const Link = ({url, name}) => {
  return <a href={url}>{name}</a>
}
ReactDOM.render(
  <Link url="http://learn.co" name="Learn" />,
  document.getElementById('root')
);
```

**Recap:** We've now seen that components are:
- custom JSX (capitalized) and used like JSX
- functions that take one argument `props`
- they return (render) JSX
- their `props` come from the _attributes_ and _children_ that are passed into them when used like JSX


#### Class Components and Props

Let's take components one step further and explain why I keep saying `render` next to _return_:
- _Components can also be classes!_
- Components that are classes are called _class components_.
- They just need to extend `React.Component`
- and they need to have a `render()` method that returns JSX (or nothing).

**Keys to note:**
- When using classes, React creates instances of our class.
- As you know from vanilla JS, each instance of a class is different from each other.
- So the `this` context inside one instance is not the same as another.
  - You'll eventually see why _this_ is the reason why classes can have `state` and lifecycle methods whereas functional components cannot.
  - When you use one of the other becomes clear after building out several projects.

```javascript
// React creates instances of our classes.
// As instances, they have a context that can be accessed through this.
// One of the properties of this instance (this) is our props!
// So to get access to props, we use: this.props
class Link extends React.Component {
  render() {
    return <a href={this.props.url}>{this.props.name}</a>
  }
}
ReactDOM.render(
  <Link url="http://learn.co" name="Learn" />,
  document.getElementById('root')
);

// But where does props come from?
// As we know, classes have constructors (initializers in Ruby).
// This is inherited from React.Component.
// This constructor takes one argument: props
// This is where props comes from; just like our Functional Components.
class Link extends React.Component {
  // Writing our own constructor.
  constructor(props) {
    super(props); // calling React.Component's constructor via super.
  }

  render() {
    return <a href={this.props.url}>{this.props.name}</a>
  }
}

// As you know, classes allow us to encapsulate functionality.
// This allows us to keep our JSX descriptive and declarative
// and isolate functionality in our components.
// By doing this, it's easy to:
// - read higher level code that uses our components in a declarative way
// - look at individual components and understand what they do
class Link extends React.Component {
  // Writing our own constructor.
  constructor(props) {
    super(props); // calling React.Component's constructor via super.
  }

  capitalize(name) {
    return name.charAt(0).toUpperCase() + name.slice(1);
  }

  render() {
    const {url, name, capitalize} = this.props; // destructuring    
    return <a href={url}>{capitalize ? this.capitalize(name) : name}</a>
  }
}
ReactDOM.render(
  <div>
    <Link url="http://learn.co" name="learn" capitalize />
    <Link url="http://google.com" name="google" />
  </div>,
  document.getElementById('root')
);
```

#### Composition, Props, and Step 2

So we now understand components (functional and class). However, what is the point?
- We will now touch on **composition** and how it allows our code to remain **declarative** at various levels of depth.
- However, React still won't seem all that amazing yet as we don't get into **state** and **state management** yet. You will have to trust that everything will click once all the parts of React are taught.

React prefers [**composition**](https://reactjs.org/docs/composition-vs-inheritance.html) over inheritance. This allows for **containment** and **specialization**. You can read about it in the docs, but for now, don't worry about it. For us to understand the the benefits of **components**, we just need to touch a bit on composition and see how it let's us keep code readable. And we'll do this by continuing with **Step 2**:

```
App
  Forms
    ListForm
    TaskForm
  Lists
    ListContainer
      Task
```

Remember that? We're going to write semi-pseudocode with components to show how our code will remain **declarative** all the way down.

```javascript
// Most of app will start out looking like this for a reason:
// We understand at the very root level of our code (index.js) that we are rendering an app.
// Readable.
ReactDOM.render(
  <App />,
  document.getElementById('root')
);

// Next level down. What is App?
// It's our Component Hierarchy for Task Lister!
class App extends React.Component {
  render() {
    return (
      <div>
        <Forms />
        <Lists />
      </div>
    )
  }
}

// Let's look at Forms.
// Still readable!
class Forms extends React.Component {
  render() {
    return (
      <div>
        <ListForm />
        <TaskForm />
      </div>
    )
  }
}

// Let's look at Lists.
// Still readable!
class Lists extends React.Component {
  // That map is pseudocode, but gives a clear idea of how this would work.
  render() {
    return (
      <div>
        {this.props.lists.map(list => <ListContainer tasks={list.tasks} />)}
      </div>
    )
  }
}

// Lastly, let's look at ListContainer since this is just getting redundant.
// Again, still readable!
class Lists extends React.Component {
  // Again, pseudocode:
  render() {
    return (
      <div>
        {this.props.tasks.map(task => <Task description={task.description} priority={task.priority} />)}
      </div>
    )
  }
}
```

And that's it!

All that is left is to actually make some mock data and write components that can use that mock data. To start, you'd want to do something like this:

```javascript
const mockData = {
  /* fake data goes here */
}

class App extends React.Component {
  render() {
    // Pass the parts of the mock data that you need down
    // into other components through their attributes
    // so they can access them through their props.
    // Continue doing this at every level and pass the data down.
    return (
      <div>
        <Forms />
        <Lists />
      </div>
    )
  }
}
```

If you construct your _static version_ in this manner, it will become clear that **props go down** (data is always passed down through props via attributes and/or children).
- I'll leave the exercise of actually building this static version to you.


#### Component & Props Summary

And that's all we need to know about **components** and **props** for now. Hopefully I've convinced you that writing code using components keeps things readable.

**Summary:** (you should be able to connect the dots for all of these)
- We understand how JSX is a crucial part of components.
- We understand functional and class components.
- We can now see that JSX is like legos but so are components:
  - Components can be as small as a single lego.
  - Components can be as big as multiple legos.
- We make components to allow our code to remain readable in a declarative manner at every level of our codebase.
- We understand where `props` come from and how they are related to JSX.
- With just this basic understanding of components, we can execute **Step 2** of _Thinking in React_.


### Thinking About State & Step 3

_We're not going to write any code dealing with `state` yet!!_ The next few steps are all a thought exercise to help you get thinking about React and `state`. I think it's important to go through the steps of thinking, but you can skip to the next section if you'd rather just start seeing code first. Different learning styles for everyone (^ ^)b

**Next Up:** Step 3: Identify The Minimal (but complete) Representation Of UI State

In order to tackle this next step, we finally need to learn about **state**.
- _class components_ have `state`
- _functional components_ do not have `state`
- Keep this in mind when deciding whether or not to use function or class components.

Things we will not talk about yet, but are important parts of `state`:
- changing state (`setState`)
- [lifting state up](https://reactjs.org/docs/lifting-state-up.html)
- event handlers
- lifecycle methods

Per the docs, we can figure out what is `state` by asking these [three questions](https://reactjs.org/docs/thinking-in-react.html):
1. Is it passed in from a parent via `props`? If so, it probably isn’t `state`.
2. Does it remain unchanged over time? If so, it probably isn’t `state`.
3. Can you compute it based on any other `state` or `props` in your component? If so, it isn’t `state`.

But before we answer those questions for our _Task Lister_ app, what do those questions tell us about the purpose of `state`?
- (Thinking exercise.)

```
App
  Forms
    ListForm
    TaskForm
  Lists
    ListContainer
      Task
```

- `<Task>` gets its information to display from `<ListContainer>` so all `props`.
- Ditto for `<ListContainer>` and `<Lists>`.
- `<TaskForm>` has fields that users type into. We need to keep track of this data and it doesn't come from a parent or can be computed. There's some `state` here!
- Ditto for `<ListForm>`.
- `<Forms>` however just group and wrap our forms together. So no `state`.
- `<App>` is where the data for the `<Lists>` start getting passed down from.
  - That data can change (lists and tasks can be added and removed).
  - So there should be some `state` here as well.

In the end, we have different bits of `state` in different components that looks like this:

- `<ListForm>` has `state` for their form fields.
- `<TaskForm>` has `state` for their form fields.
- `<App>` has the `state` for the lists and tasks.

This is all thinking though and no code yet. However, it's an exercise that you should do as it helps identify what is `state`, what isn't, and eventually what `state` you'll need to [lift up](https://reactjs.org/docs/lifting-state-up.html). And now onwards to _Step 4: Identify Where Your State Should Live_.

For each piece of state in your application:
- Identify every component that renders something based on that state.
- Find a common owner component (a single component above all the components that need the state in the hierarchy).
- Either the common owner or another component higher up in the hierarchy should own the state.
- If you can’t find a component where it makes sense to own the state, create a new component simply for holding the state and add it somewhere in the hierarchy above the common owner component.

Is our `state` in the correct places?
- (thinking exercise)
- Overall looks good so let's continue.
- At this point, you should be able to place state in the correct components and pass down the correct data to other components so they can use that data via their `props`.
  - Try to refactor your code and get this working with dummy data in `state`.
  - You don't need your `mockData` variable anymore.


### State (for real this time)

Now that we know where `state` will live, let's learn about state using a much more scoped down example of a button that continually adds "!" to its name when you click on it.
- `state` is what will change and that's the button's name
- so we will have `name` be a part of `state`

```javascript
// The two ways you'll see state initialized will be like this:
class Button extends React.Component {
  // Inside the constructor:
  // (I'm more used to this syntax so it's what I'll be using.)
  constructor(props) {
    super(props);

    this.state = {
      name: "button",
    };
  }

  // Without a constructor as a class field declaration:
  // https://github.com/tc39/proposal-class-fields
  /*
  state = {
    name: "button",
  };
  */

  // Now we can access the properties in state similar to props like so:
  render() {
    return (
      <button>{this.state.name}</button>
    )
  }
}
```

We could have also initialized `state` using values that the component received via `props`:

```javascript
// Not necessarily in ReactDOM.render,
// but anywhere where we use our custom Button component.
ReactDOM.render(
  <Button name="button" />,
  document.getElementById('root')
);

class Button extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      name: this.props.button,
    };
  }

  render() {
    return (
      <button>{this.state.name}</button>
    )
  }
}
```

However, that's wholely uninteresting. `state` should be something we can change. Let's change `state`!

```javascript
// Back to our original example to keep things simple:
class Button extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      name: "button",
    };
  }

  // First thought would be to do this:
  addExclamation() {
    this.state.name += "!";
  }

  // Then just let onClick, note the camelCase, run our function.
  render() {
    return (
      <button onClick={this.addExclamation}>{this.state.name}</button>
    )
  }
}
```

But that won't work! Nothing changed! Why?
- `state` is supposed to be _immutable_.
- So how do we change it?
  - Changing `state` must be done through `setState`.
  - `setState` will tell React to check for changes and _schedule_ an update if needed.
  - Keyword: schedule. That should reveal to you that `setState` is asynchronous so don't expect it to happen immediately. Otherwise you'll run into unexpected issues later!

`setState` works by merging the the object it receives with the current `state`.

```javascript
class Button extends React.Component {
  constructor(props) {
    super(props);

    this.state = {
      name: "button",
    };
  }

  // Now using setState:
  addExclamation() {
    const newName = this.state.name + "!";
    this.setState({ name: newName });
  }

  render() {
    return (
      <button onClick={this.addExclamation}>{this.state.name}</button>
    )
  }
}
```

Great! Let's run it now!
- Wait... `undefined`? Why?

```javascript
// We're passing the function block here so it can be run later somewhere else.
// That means it won't be running in the context of our Button anymore.
// It will be in the context of wherever we are running our function.
onClick={this.addExclamation}
```

How can we solve this? Remember back to scope and `this` in mod 3.
- Solution 1: `bind`

  ```javascript
  class Button extends React.Component {
    constructor(props) {
      super(props);

      this.state = {
        name: "button",
      };

      // Solution 1: by binding
      this.addExclamation = this.addExclamation.bind(this);
    }

    addExclamation() {
      const newName = this.state.name + "!";
      this.setState({ name: newName });
    }

    render() {
      return (
        <button onClick={this.addExclamation}>{this.state.name}</button>
      )
    }
  }
  ```

- Solution 2: arrow functions

  ```javascript
  class Button extends React.Component {
    constructor(props) {
      super(props);

      this.state = {
        name: "button",
      };
    }

    addExclamation = () => {
      const newName = this.state.name + "!";
      this.setState({ name: newName });
    }

    render() {
      return (
        <button onClick={this.addExclamation}>{this.state.name}</button>
      )
    }
  }
  ```

And that's it for `state`! These are the _basics_ you'll need to know that will let you complete _Steps 3 & 4_. Once you're comfortable with changing state locally, you can even venture into _Step 5_!
