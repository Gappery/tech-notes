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
* general rule for event propagation (except for `onScroll`)
  1. Capture Phase:
      * Events move from the root to the target element (top-down).
  2. Target Phase:
      * The event reaches the target element.
  3. Bubbling Phase:
      * Events propagate back up from the target to the root (bottom-up).
      * child event will also bubble up in the UI tree to parent handler function
  * Example: when you want to catch all events on child elements. For below example
    * It travels down, calling all onClickCapture handlers.
    * It runs the clicked element's onClick handler.
    * It travels upwards, calling all onClick handlers.

  ```jsx
  <div onClickCapture={() => { /* this runs first */ }}>
    <button onClick={e => e.stopPropagation()} />
    <button onClick={e => e.stopPropagation()} />
  </div>
  ```

* stop propagation
  * Event handlers receive an event object as their only argument, called `e` by convention
  * `e.stopPropagation` to prevent an event from reaching parent components
    * do it in child function to better trace the logic `{e.stopPropagation(); onClick();}`
  * Events may have unwanted default browser behavior. Call `e.preventDefault()` to prevent that.
* event handlers don't need to be pure

### State as Component Memory

* local variables
  * Local variables don’t persist between renders
  * Changes to local variables won’t trigger renders.
* `useState` hook
  * state variable: retain data between renders
  * setter function: trigger react component re-render when updating value
* TODO: (move to another place?) Hooks—functions starting with use—can only be called at the top level of your components or your own Hooks.
  * You use React features at the top of your component similar to how you import modules at the top of your file.
  * Hooks must be called unconditionally and always in the same order!
* Each component has it's own state variable, it's not shared across components

### Render & Commit

* Trigger render
  * initial render
  * re-render - state update
* Actual render - react calls the component - it updates the state variable if needed
  * initial render - call the root - create DOM nodes
  * re-render - call corresponding component - calculate but doesn't update state yet
* Commit - commit change to DOM
  * initial render - use `appendChild` to put all DOM nodes in the screen
  * re-render - apply the minimal necessary operations (calculated while rendering!, only change the difference) to make the DOM match the latest rendering output.
* Epilogue - browser painting (rendering)

### State as snapshot

* setting state triggers re-render
* The JSX returned is like a snapshot of the UI, meaning its props, event handlers, and local variables were all calculated using its state at the time of the render.
* State lives in react, outside of the component function
* A state variable’s value never changes within a render
  * even the async code in the event handler function: React keeps the state values “fixed” within one render’s event handlers.
* example
  * Setting state only changes it for the next render, and the snapshot value for `number` is 0, so alerting shows `0`, and `number` value for next render is `1`

```jsx
export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 1);
        setNumber(number + 1);
        setNumber(number + 1);
        alert(number)
      }}>+3</button>
    </>
  )
}
```

### Queueing State Updates

* React waits until all code in the event handlers has run before processing your state updates.
  * help avoid triggering multiple re-renders
  * also means UI won't be updated until every code in event handler completes
* React does not batch across multiple intentional events like clicks—each click is handled separately.
  * This ensures that, for example, if the first button click disables a form, the second click would not submit it again.
* Solution: passing updater function, for code below
  * react adds those 3 updates to the queue, and goes over the queue when processing `useState` in next render

  ```jsx
  setNumber(n => n + 1);
  setNumber(n => n + 1);
  setNumber(n => n + 1);
  ```

* <https://react.dev/learn/queueing-a-series-of-state-updates> -> fix a request counter is a good question

* It’s common to name the updater function argument by the first letters of the corresponding state variable (more verbose is also find):

  ```jsx
  setLastName(ln => ln.reverse());
  setEnabled(enabled => !enabled)
  ```

* You may have noticed that `setState(5)` actually works like `setState(n => 5)`, but n is unused!

* linter: eslint-plugin-react-hooks
* state is fully private to the component declaring it.
* This lets you add state to any component or remove it without impacting the rest of the components

* Don’t introduce state variables when a regular variable works well.

### Update object & array

* treat both as IMMUTABLE/read-only although they're technically mutable
* without using the state setting function, React has no idea that object/array has changed
* need to create a new object/array and pass it to the state setting function

#### update object

* create a new one and call setter
* copying object with spread syntax to avoid copying every property separately
  * `...` only does shallow copy - it only copies things one level deep

  ```jsx
  setFoo({
    ...oldFoo,
    newFoo: false
  })
  ```

* updating nested object
  * Generally, you shouldn’t need to update state more than a couple of levels deep. So think of restructuring first

  ```jsx
  setFoo({
    ...oldFoo,
    oldField1: {
      ...oldFields,
      field1: false,
    }
  })
  ```

* object is not really nested! Especially objects can point to each other
  * editing `obj3.artwork.city` in below example affects obj2 and obj1
  * they are separate objects “pointing” at each other with properties.

  ```ts
  let obj1 = {
    title: 'Blue Nana',
    city: 'Hamburg',
    image: 'https://i.imgur.com/Sd1AgUOm.jpg',
  };

  let obj2 = {
    name: 'Niki de Saint Phalle',
    artwork: obj1
  };

  let obj3 = {
    name: 'Copycat',
    artwork: obj1
  };
  ```

* [immer](https://github.com/immerjs/use-immer) is recommended for nested object
  * The draft provided by Immer is a special type of object, called a [Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy), that “records” what you do with it.
      This is why you can mutate it freely as much as you like! Under the hood, Immer figures out which parts of the draft have been changed, and produces a completely new object that contains your edits.

  ```jsx
  const [person, updatePerson] = useImmer({});
  updatePerson(draft => {
    draft.artwork.city = 'Lagos';
  });
  ```

#### update array

* can't call function that mutates the array, should only call non-mutating method
* add
  * NO: `push(), unshift()`
  * YES: `[...arr, newElement] concat()`

  ```jsx
    const newArr = [
      // Items before the insertion point:
      ...oldArr.slice(0, idx),
      // New item:
      { id: nextId++, name: name },
      // Items after the insertion point:
      ...artists.slice(idx)
  ];
  ```

* remove
  * NO: `pop(), shift(), splice()`
  * YES: `filter(), slice()`
* replace
  * NO: `splice()`
  * YES: `map()`
* sort
  * NO: `reverse(), sort()`
  * YES: `use [...list] to copy first`
* `slice()` vs `splice()`
  * `slice` lets you copy an array or a part of it.
  * `splice` mutates the array (to insert or delete items).
* `[...arr]` does the shallow copy
  * Meaning if you modify an object inside the copied array, you are mutating the existing state
  * should refer to `update object`
  * use `map` for substitution

  ```jsx
  setMyList(myList.map(artwork => {
    if (artwork.id === artworkId) {
      // Create a *new* object with changes
      return { ...artwork, seen: nextSeen };
    } else {
      // No changes
      return artwork;
    }
  }));
  ```

* `lmmer`

```jsx
  const [myList, updateMyList] = useImmer([]);
  updateMyList(draft => {
    const artwork = draft.find(a =>
      a.id === id
    );
    artwork.seen = nextSeen;
  });
```

## State Management

### Imperative VS Declarative

* Imperative: give direct instruction to each UI element on how to respond to a certain UI action
  * works well enough for isolated examples, but gets exponentially more difficult to manage in more complex systems
  * one example is just think of ticket submission logic in launch driver, and how difficult it is to add a new small change!
* Declarative in React: declare what you want to show, and let React update the UI
  * Identify component's different visual states
    * storybooks: document all states and their corresponding UI layouts
  * Determine what triggers those state changes
    * human input: click button/type text field/navigate - always requires event handlers
    * computer input: svc response/image loading/timeout completion
    * draw state chart can help visualize the workflow
  * Represent the state in memory using `useState`
    * Simplicity is key: each piece of state is a “moving piece”, and you want as few “moving pieces” as possible. More complexity leads to more bugs!
    * add all -> refactor is also a good practice
  * Remove any non-essential state variables
    * the goal is to prevent the cases where the state in memory doesn’t represent any valid UI that you’d want a user to see
      * Does this state cause a paradox: e.g. `isTyping` and `isSubmitting` can't both be true
      * Is the same information available in another state variable already: e.g. two type represent the same info
      * Can you get the same information from the inverse of another state variable
  * Connect the event handlers to set the state
* Expressing all interactions as state changes lets you later introduce new visual states without breaking existing ones.

### Principles for structuring state

Goal: make state easy to update without introducing mistakes.

* Group related state
  * if some two state variables always change together, it might be a good idea to unify them into a single state variable
    * e.g. first/last name to name object
  * if you don't know how many pieces of state you'll need: form with customized fields
* Avoid contradictions in state
  * e.g. `isSent` and `isSending` can't both be true of false => change to one state instead
* Avoid redundant state
  * If information can be calculated from the props or existing state variables, then shouldn't add state for that variable
  * TODO:
* Avoid duplication in state
* Avoid deeply nested state

## More

* You shouldn’t read (or write) the current value during rendering.

## TOADD

* ref: When a piece of information is used for rendering, keep it in state. When a piece of information is only needed by event handlers and changing it doesn’t require a re-render, using a ref may be more efficient.
* ref: You shouldn’t read (or write) the current value during rendering.

## General

* React components are regular JavaScript functions, but their names must start with a capital letter or they won't work!

## TODOs

* read useState implementations: <https://medium.com/@ryardley/react-hooks-not-magic-just-arrays-cd4f1857236e>
* <https://legacy.reactjs.org/docs/optimizing-performance.html>
* Why is mutating state not recommended in React?: <https://react.dev/learn/updating-objects-in-state>
* <https://react.dev/learn/thinking-in-react>
* Lexical Scope:
  * <https://cleverzone.medium.com/lexical-scope-in-javascript-929789101dab#:~:text=In%20JavaScript%2C%20lexical%20scope%20is%20the%20concept%20of%20determining%20the,in%20which%20it%20is%20used>.
  * <https://www.techtarget.com/whatis/definition/lexical-scoping-static-scoping#:~:text=Lexical%20scoping%20in%20JavaScript,of%20where%20it%20was%20invoked>.
* Closure:
  * <https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures> |
