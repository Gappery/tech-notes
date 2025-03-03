# ALL

## Observer

* observer is a higher-order component (HOC) from mobx-react-lite.
It wraps a React component and makes it reactive, meaning it will automatically re-render whenever the MobX state it depends on changes.

### Why needed

* By default, React components only re-render when props or state change.
However, MobX manages state outside of React’s internal state system, so React doesn’t know when to re-render.
* observer connects MobX state to React, telling React "Hey, re-render this component when any observable inside it changes."

### How it works

* Tracks all observable state used inside the component.
* Subscribes the component to re-render when those observables change.
* Only re-renders when necessary (very optimized!).

### When use

* use if
  * Components displaying observable state.
  * Components depending on MobX actions or computed values.
* NOT use if
  * Components that don’t use MobX state (e.g., layout wrappers, pure UI components).
  * Components that only receive props from parent components.
