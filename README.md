# React-redux cheat sheet
This is my personal cheat sheet, i didn't commented this very well because my intention is just to make my life easier when i forget something about Redux. This probably will not make much sense to you even if you know Redux. But give it a try if you want, feedbacks are welcome.

*By the way, these codes below are very 'handly' and wasn't tested, so don't try it at home.*

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

const store = createStore(reducer, middleware);

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

import usersReducer from './usersReducer';
import moviesReducer from './moviesReducer';

export default combineReducers({ user: usersReducer, movie: moviesReducer, });
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


export function fetchUsers() {
  
  return function(dispatch) {
    axios(URL)
      .then(response => {
        dispatch({ type: 'FETCH_USER_FULFILLED', payload: response.data });
      })
      .catch(error => {
        dispatch({
          type: 'FETCH_USER_REJECTED',
          payload: error});
      })
  }

}
```

## Middleware

```js
import { createStore, applyMiddleware } from 'redux';

import logger from 'redux-logger';
import thunk from 'redux-thunk';
import promise from 'redux-promise-middleware';

import reducer from './reducer';

const middleware = applyMiddleware(promise(), logger, thunk);

export default createStore(reducer, middleware);
```

#### Logger implementation
```js
const customLogger = (store) => (next) => action => {
  console.log('That is the action', action);
  next(action);
}
```

## Connecting React and Redux
```js
import ReactDOM from 'react-dom';
import React from 'react';

import { Provider } from 'react-redux';
import App from './components/App';
import store from './store';

const app = document.getElementById('root');

ReactDOM.render(
  <Provider store={store}>
    <App/>
  </Provider>,
  app
);
```

#### App.js file
```js
import React from 'react';
import { connect } from 'react-redux';

// code of fetchUsers is on the Actions section
import { fetchUsers } from '../actions/UserActions';
import { fetchMovies } from '../actions/MovieActions';

import User from './User';
import Movie from './Movie';


class App extends React.Component {

  render() {
    return (
      <div>
        <Button changeUsername={() => this.props.fetchUser()}>
        <User username={this.props.user.name}/>
        <Movie name={this.props.movie.name} rating={this.props.movie.rating}/>
      </div>
    );
  }

}


const mapStateToProps = (state) => {
  return {
    user: state.user,
    movie: state.movie,
  }
}


const mapDispatchToProps = (dispatch) => {
  return { fetchUsers: () => dispatch(fetchUsers()) }
}


export default connect(mapStateToProps, mapDispatchToProps)(App);
```

<br/>
<br/>

# Project Structure
![Project Structure](https://github.com/lucasfrosty/redux-cheatsheet/blob/master/structure.png 'Project Structure')