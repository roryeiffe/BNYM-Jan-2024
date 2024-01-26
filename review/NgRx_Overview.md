### NgRx Overview
NgRx is a framework for building reactive applications in Angular. It's heavily inspired by Redux, a popular state management pattern in the React community. NgRx provides a way to manage global and local states in an Angular application using a single source of truth, which makes the state predictable.

### Core Concepts and Features
1. **Store**: The single source of truth in an application. It's an immutable data structure.
   - **Example**: Creating a basic store.
     ```typescript
     import { StoreModule } from '@ngrx/store';
     import { reducer } from './reducers';

     @NgModule({
       imports: [StoreModule.forRoot({ myState: reducer })],
     })
     export class MyModule {}
     ```

2. **Actions**: Plain JavaScript objects that describe an action and its payload.
   - **Example**: Defining an action.
     ```typescript
     import { createAction } from '@ngrx/store';

     export const myAction = createAction('[My Component] My Action');
     ```

3. **Reducers**: Functions that take the current state and an action, and return a new state.
   - **Example**: Implementing a reducer.
     ```typescript
     import { createReducer, on } from '@ngrx/store';
     import { myAction } from './actions';

     const initialState = { /* initial state */ };

     const myReducer = createReducer(
       initialState,
       on(myAction, state => ({ ...state, /* new state */ })),
     );
     ```

4. **Effects**: Model side effects in your application. They listen for actions from the store, perform operations, and then dispatch new actions.
   - **Example**: Defining an effect.
     ```typescript
     import { Actions, ofType, createEffect } from '@ngrx/effects';
     import { myAction } from './actions';
     import { map, mergeMap } from 'rxjs/operators';

     export class MyEffects {
       loadItems$ = createEffect(() => this.actions$.pipe(
         ofType(myAction),
         mergeMap(() => this.myService.getItems().pipe(
           map(items => ({ type: '[Items API] Items Loaded Success', payload: items }))
         ))
       ));

       constructor(private actions$: Actions, private myService: MyService) {}
     }
     ```

5. **Selectors**: Pure functions used for obtaining slices of store state.
   - **Example**: Creating a selector.
     ```typescript
     import { createSelector } from '@ngrx/store';

     export const selectFeature = (state: AppState) => state.feature;
     export const selectFeatureItems = createSelector(
       selectFeature,
       (state: FeatureState) => state.items
     );
     ```

### Real-World Example: Shopping Cart
Consider a shopping cart application where users can add items to their cart, remove them, and check out.

1. **Store**: Contains the state of the shopping cart, including the items and the total cost.
2. **Actions**: Actions like `AddItem`, `RemoveItem`, and `Checkout`.
3. **Reducers**: Handle actions, updating the state of the shopping cart.
4. **Effects**: Deal with asynchronous operations like communicating with a backend service to retrieve item details or submit the order.
5. **Selectors**: Used to select items from the cart and calculate the total cost.

```typescript
// Action examples
export const addItem = createAction('[Cart] Add Item', props<{ item: Item }>());
export const removeItem = createAction('[Cart] Remove Item', props<{ itemId: string }>());

// Reducer example
const cartReducer = createReducer(
  initialState,
  on(addItem, (state, { item }) => ({ ...state, items: [...state.items, item] })),
  on(removeItem, (state, { itemId }) => ({ ...state, items: state.items.filter(item => item.id !== itemId) }))
);

// Selector example
export const selectCartItems = createSelector(selectCart, (state: CartState) => state.items);
```

### Review Questions
1. Basic Understanding
    - What is NgRx and how does it relate to Redux?
    - Describe the role of a Store in NgRx.
    - What are Actions in NgRx, and how are they used?
2. Reducers and State Management
    - Explain the purpose of a Reducer in NgRx.
    - How does NgRx ensure immutability in state management?
3. Effects and Side-Effects
    - Define Effects in the context of NgRx. Why are they important?
    - Give an example of a side-effect that you might handle with NgRx Effects.
4. Selectors and State Selection
    - What are Selectors in NgRx, and how do they differ from other methods of state retrieval?
    - How can Selectors improve performance in an Angular application?