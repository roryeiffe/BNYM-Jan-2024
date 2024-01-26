## Actions

### 1. Simple Actions
Simple actions are the most basic form of actions. They often indicate a single event has occurred.

- **Example**: Triggering a UI event, like opening a modal.
  ```typescript
  import { createAction } from '@ngrx/store';

  export const openModal = createAction('[UI Component] Open Modal');
  ```

### 2. Actions with Payload
These actions carry additional information (payload) required to perform an action.

- **Example**: Adding a product to a cart, where the product details are the payload.
  ```typescript
  import { createAction, props } from '@ngrx/store';
  import { Product } from './models';

  export const addProduct = createAction(
    '[Product List] Add Product',
    props<{ product: Product }>()
  );
  ```

### 3. Lifecycle Actions
Lifecycle actions are used to represent different stages of an asynchronous process, like an API call.

- **Example**: Loading data from an API, with actions for start, success, and failure.
  ```typescript
  import { createAction, props } from '@ngrx/store';
  import { User } from './models';

  export const loadUsers = createAction('[User API] Load Users');
  export const loadUsersSuccess = createAction(
    '[User API] Load Users Success',
    props<{ users: User[] }>()
  );
  export const loadUsersFailure = createAction(
    '[User API] Load Users Failure',
    props<{ error: any }>()
  );
  ```

## Props

In NgRx, `props` is a function used to define additional data or payload that an action carries. When you define an action in NgRx, it often represents an event or a command, and sometimes this event needs to carry with it some context or data. This is where `props` comes into play. It allows you to attach this additional information to your actions in a type-safe manner.

### Understanding `props`

- **Type Safety**: With `props`, you can specify the type of the data that the action is expected to carry. This enhances type safety, making your application more robust and less prone to runtime errors.
  
- **Payload**: The term 'payload' in the context of NgRx actions typically refers to the data carried by an action. `props` is used to define the shape and type of this payload.

### How `props` is Used

1. **Importing `props`**: First, you need to import `props` from '@ngrx/store'.

   ```typescript
   import { createAction, props } from '@ngrx/store';
   ```

2. **Defining an Action with Payload**: When creating an action, you use `props` to define the payload. This is done by passing a function to `props` that returns the payload structure.

   ```typescript
   export const addBook = createAction(
     '[Book List] Add Book',
     props<{ bookId: string; title: string }>()
   );
   ```

   In this example, `addBook` is an action that carries a payload with two properties: `bookId` and `title`, both of which are strings.

### Why Use `props`?

- **Clarity and Maintenance**: Using `props` makes it clear what data an action is expected to carry. This is especially useful in large applications where actions can be dispatched and handled in many different places.
  
- **Consistency**: NgRx's use of `props` provides a consistent way of handling action payloads, making your code more uniform and easier to understand.

- **Decoupling**: `props` allows you to define the payload separately from the action, which can be beneficial for decoupling your action definitions from their usage.

### Example in Reducer

When you handle this action in a reducer, you can access this payload directly:

```typescript
const bookReducer = createReducer(
  initialState,
  on(addBook, (state, { bookId, title }) => {
    // You can directly use bookId and title here
    return { ...state, /* your updated state */ };
  })
);
```
