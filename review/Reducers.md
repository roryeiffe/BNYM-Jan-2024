### Understanding Reducers

1. **Pure Functions**: Reducers are pure functions. They take the current state and an action as arguments and return a new state. They don't modify the current state directly (immutability) and have no side effects (pure).

2. **Handling Actions**: Reducers determine how the state should change in response to an action. They use the action's type to decide how to transform the state.

3. **Immutability**: It's crucial that reducers don't modify the original state object. They should create and return a new state object with the necessary changes.

### Basic Structure of a Reducer

Here’s how a basic reducer function looks:

```typescript
function myReducer(state: StateType = initialState, action: Action): StateType {
  switch (action.type) {
    case 'ACTION_TYPE':
      // handle action and return new state
      return newState;
    default:
      return state;
  }
}
```

### Using `createReducer` Function

In NgRx, `createReducer` is a function that simplifies the creation of reducers:

1. **Defining Reducers**: It allows you to define a reducer in a more readable and declarative way.
2. **Handling Actions**: Uses the `on` function to handle specific actions.

Here’s an example:

```typescript
import { createReducer, on } from '@ngrx/store';
import { increment, decrement, reset } from '../actions/counter.actions';

export const initialState = 0;

export const counterReducer = createReducer(
  initialState,
  on(increment, state => state + 1),
  on(decrement, state => state - 1),
  on(reset, state => 0)
);

```

In this example, `counterReducer` handles three actions: `increment`, `decrement`, and `reset`. The state is updated accordingly in response to these actions.

### Best Practices

1. **Immutability**: Always return a new state object instead of modifying the existing one.
2. **Simplicity**: Keep reducers simple and focused on a single responsibility.
3. **No Side Effects**: Reducers must not produce side effects like API calls or routing transitions.
