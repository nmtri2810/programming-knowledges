# Reactjs

- Passing function through `props`
- **Immutability** in react -> to undo changes, ...

## Hooks

### useState

...

### useEffect

- Deals with side effects

#### No dependencies

#### Blank dependencies

#### Have dependencies

### useRef

### useCallback

# HOC (High order component)

- A function that have parameters as another component and return new component

# Redux

Main concept:

- `Dispatch`: send the action
- `Action`: has the { type: action-type, payload: data-to-transfer } and goes to store. `Actions` are plain JS objects
- `Store`: Contains `state` and `reducer` to handle state changes. `Reducers` specify how the state should be updated based on those `actions`

![redux data flow](../public/img/redux-data-flow.png)
