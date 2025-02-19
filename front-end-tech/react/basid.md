# Offcial Docs

* <https://react.dev/learn/start-a-new-react-project>

## Pre-requisite Concept

### DOM

* <https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction>

## Describing UI

### Component

* In a React app, every piece of UI is a component.
* a React component is a JavaScript function that you can sprinkle with markup
* react returns jsx markup: <https://react.dev/learn/writing-markup-with-jsx>
* react will classify tag that started with lowercase character as HTML tag, otherwise use our own tag
* define every component at the top level

### Import/Export

* Named

```ts
export function Button() {}
// import must use exact name matching. That's why they are called named imports!
import { Button } from './Button.js';
```

* Default

```ts
export default function Button() {}
// import can put any name you want after import
import Fulton from './Button.js';
```

* `./Gallery.js` or `./Gallery` will work with React, though the former is closer to how native ES Modules work.
* A file can have no more than one default export, but it can have as many named exports as you like.
* To reduce the potential confusion between default and named exports, some teams choose to only stick to one style (default or named), or avoid mixing them in a single file. Do what works best for you!

### Markup with JSX

* Previously HTML and JS are separate files, but now logic increasingly determined content
* in React, rendering logic and markup live together in the same place—components
* JSX and React are two separate things. They're often used together, but you can use them independently of each other. JSX is a syntax extension, while React is a JavaScript library.
  * <https://legacy.reactjs.org/blog/2020/09/22/introducing-the-new-jsx-transform.html#whats-a-jsx-transform>
* JSX is stricter and has a few more rules than HTML
  * JSX looks like HTML, but under the hood it is transformed into plain JavaScript objects. You can't return two objects from a function without wrapping them into an array. This explains why you also can't return two JSX tags without wrapping them into another tag or a Fragment.
* JSX rules
  * return a single root: To return multiple elements from a component, wrap them with a single parent tag.
  * Close all the tags
    * `<img />` or `<img></img>`
  * use camelCase like JS variable naming style

### Curly Brace {} in JSX: A window into the JavaScript world

* Can only use curly braces in two ways inside JSX
  * As text directly inside a JSX tag:
    * `<h1>{name}'s To Do List</h1>` works
    * `<{tag}>Gregorio Y. Zara's To Do List</{tag}>` doesn't work
  * As attributes immediately following the `=` sign
    * `src={avatar}` reads avatar variable
    * `src="{avatar}"` will just pass the string `"{avatar}"`
* Use "double curlies" for passing object

```jsx
<ul style={{
    backgroundColor: 'black',
    color: 'pink'
}}>
```

### Passing Props

* Props are the information that you pass to a JSX tag. For example, className, src, alt, width, and height are some of the props you can pass to an `<img>`
  * same for your own react component
* Prop values specifying
  * default value: destructuring
    * `function Avatar({ foo, bar = 100 })`
  * spread syntax (if all variables are used)
    * child:

    ```jsx
    function Profile({ person, size, isSepia, thickBorder }) {
        return (
            <div className="card">
            <Avatar
                person={person}
                size={size}
                isSepia={isSepia}
                thickBorder={thickBorder}
            />
            </div>
        );
    }
    ```

    * parent:

    ```jsx
    function Profile(props) {
        return (
            <div className="card">
            <Avatar {...props} />
            </div>
        );
    }
    ```

* Passing JSX as children
  * React automatically passes the content between `<Card> ... </Card>` as props.children to the Card component.

```jsx
<Card>
  <Avatar />
</Card>
```

* Props changing
  * a component may receive different props over time, props are not always static
  * however, props are immutable—a term from computer science meaning “unchangeable”.
  * Props are read-only snapshots in time: every render receives a new version of props.
  * component needs to ask it's parent when changing props is needed
* Follow up: JSX rule of passing children
  * When passing children to a component, they must be wrapped inside the opening and closing tags of that component.
  * When using a self-closing tag `<Foo />`, it only supports props, not children.
  * cannot put raw JSX elements inside the tag itself.

### Conditional Rendering

* A component must return something, so return `null` if you don't want to render anything
  * In practice, returning null from a component isn't common because it might surprise a developer trying to render it

* Ways of conditional rendering
  * Ternary

    ```jsx
    return (
    <li className="item">
        {isPacked ? name + ' ✅' : name}
    </li>
    );
    ```

  * Logical And `&&`

    ```jsx
    return (
      <li className="item">
        {name} {isPacked && '✅'}
    </li>
    );
    ```

    * Javascript returns the value of its right side if the left side is true
    * But if the condition is false, the whole expression becomes false
    * React considers false as a “hole” in the JSX tree, just like null or undefined, and doesn't render anything in its place.
    * NOTE: Don't put numbers on the left side of &&. Since 0 will be used instead of the right side value!
      * actually it applies any falsy values: `The logical AND (&&) operator returns the first falsy value it encounters. If all values are truthy, it returns the last operand.`
* Plain `if else`

### Rendering Lists

* Basic: `.filter()` and `.map()`

```jsx
  const listItems = people.map(person =>
    <li>{person}</li>
  );
```

* JS pitfalls around returning values
  * arrow function implicitly return the expression right after `=>`
  * however if `=>` is followed by `{`, then must write `return` explicitly
* JSX elements directly inside a map() call always need keys!
* if each item needs to render more then one DOM nodes - group in `<div>` or using `<Fragment>`

```JSX
const listItems = people.map(person =>
  <Fragment key={person.id}>
    <h1>{person.name}</h1>
    <p>{person.bio}</p>
  </Fragment>
);
```

* rules for keys
  * Keys must be unique among siblings
  * Don't generate key on the fly
    * This will cause keys to never match up between renders, leading to all your components and DOM being recreated every time. Not only is this slow, but it will also lose any user input inside the list items. Instead, use a stable ID based on the data.
  * `key` is not used by component by React

### Keeping Components Pure

* Pure function
  * It minds its own business. It does not change any objects or variables that existed before it was called.
  * Same inputs, same output. Given the same inputs, a pure function should always return the same result.
* React assumes that every component you write is a pure function!
  * React components you write must always return the same JSX given the same inputs
* In general, you should not expect your components to be rendered in any particular order
* Strict mode: finds the component that breaks the rule by calling each component twice
* Local Mutation
  * Pure functions don't mutate variables outside of the function's scope or objects that were created before the call—that makes them impure!
  * However, it's completely fine to change variables and objects that you've just created while rendering.
* Allowed side effects cases:
  * Not during rendering: updating the screen, starting an animation, changing the data—are called side effects
  * Normally belong inside event handler - So event handlers don't need to be pure
  * Approach be your last resort: attach side effect logic to JSX with `useEffect`
* Importance of purity
  * Safely run components in different environments
  * Safe to skip rendering for components that input is not changed
  * Safe to restart another rendering when data is changed in the middle of a rendering

* In general, you should not expect your components to be rendered in any particular order
* These changes—updating the screen, starting an animation, changing the data—are called side effects.

### UI as a tree

* Trees are a common way to represent the relationship between entities. They are often used to model UI.
* Render trees
  * represent the nested relationship between React components across a single render.
* Dependency trees
  * represent the module dependencies in a React app (might be different from render tree if let's say a child component is provided as prop)
  * used by build tools to bundle the necessary code to ship an app.
  * useful for debugging large bundle sizes that slow time to paint and expose opportunities for optimizing what code is bundled.

## Adding Interactivity

### Responding to Event

* event handler functions
  * basic structure: `<button onClick={parent.onClick} />`
  * name style: internal handler function use `handle**` | when passing as props name use `on**`
  * NOTE: need to pass the function not call the function! (we don't want to call them on rendering)
    * `onClick={onClick}` NOT `onClick={onClick()}`
    * `onClick={() => alert(...)}` NOT `onClick={alert(...)}`
  * can read other input props
* passing handler as props
  * parent normally specifies the handler for child
  * create action name on top of browser default event names for flexibility
* event propagation (except for `onScroll`)
  * child event will also bubble up in the UI tree to parent handler function
  * Event handlers receive an event object as their only argument, called `e` by convention
  * `e.stopPropagation` to prevent an event from reaching parent components
    * do it in child function to better trace the logic `{e.stopPropagation(); onClick();}`
  * capture phase events: when you want to catch all events on child elements. For below example
    * It travels down, calling all onClickCapture handlers.
    * It runs the clicked element's onClick handler.
    * It travels upwards, calling all onClick handlers.

  ```jsx
  <div onClickCapture={() => { /* this runs first */ }}>
    <button onClick={e => e.stopPropagation()} />
    <button onClick={e => e.stopPropagation()} />
  </div>
  ```

  * Events may have unwanted default browser behavior. Call `e.preventDefault()` to prevent that.
* event handlers don't need to be pure

### State

* linter: eslint-plugin-react-hooks
* state is fully private to the component declaring it.
* This lets you add state to any component or remove it without impacting the rest of the components
* Hooks must be called unconditionally and always in the same order!
* Don’t introduce state variables when a regular variable works well.

### State as a snapshots

* Setting state only changes it for the next render.

### Update object

* `...` only does shallow copy
* object is not really nested!
* <https://github.com/immerjs/use-immer> for changing nested value?

## General

* React components are regular JavaScript functions, but their names must start with a capital letter or they won't work!

## TODOs

* read useState implementations: <https://medium.com/@ryardley/react-hooks-not-magic-just-arrays-cd4f1857236e>
* <https://legacy.reactjs.org/docs/optimizing-performance.html>
* object spread
