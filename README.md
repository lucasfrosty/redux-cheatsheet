![diagram](https://github.com/lucasfrosty/redux-cheatsheet/blob/master/diagram.png 'Redux Diagram')

# Brainstorm

- Create the store
- Pass the reducer to the store
- The store will dispatch an action passing the **action.type** and **action.payload**
- The reducer will return a new state (not changing the previous one) based on the **action.type**
- The new state will be stored on the store.
- On the store change, the React itself will rerender the components that uses it.

## Store
```js
import { createStore } from 'redux';

const store = createStore(reducer);

/* in React this code will not be necessary because the store will be passed as props,
and you know that once the prop is changed the React will rerender. */
store.subscribe(() => {
  console.log('Store was updated!!', store.getState());
})

store.dispatch({
  type: 'ADD',
  payload: 100,
});

store.dispatch({
  type: 'SUB',
  payload: 40,
});
```

## Reducer

```js
const initialState = {
  result: 0,
  lastValues: [],
}

const reducer = (state = initialState, action) => {
  switch (action.type) {

    case 'ADD': {
      state = {
        ...state,
        result: state.result + action.payload,
        lastValues: [...state.lastValues, action.payload],
      };

      break;
    }

    case 'SUB': {
      state = {
        ...state,
        result: state.result - action.payload,
        lastValues: [...state.lastValues, action.payload],
      };
      
      break;
    }

  }


  return state;
}
```

#### Multiples Reducers
```js
import { combineReducers } from 'redux';

import users from './usersReducer';
import movies from './moviesReducer';

export default combineReducers({ users, movies, });
```

## Actions
```js
import axios from 'axios';
const URL = 'https://jsonplaceholder.typicode.com/users';

export function someSyncAction() {
  return function(dispatch) {
    dispatch({ type: 'SYNC_TYPE', payload: 1, });
  }
}


export function someAsyncAction() {
  
  return function(dispatch) {
    axios(URL)
      .then(response => {
        dispatch({ type: 'FETCH_USER_FULFILLED', payload: response.data });
      })
      .catch(error => {
        dispatch({
          type: 'FETCH_USER_REJECTED',
          payload: response.data});
      })
  }

}
```
