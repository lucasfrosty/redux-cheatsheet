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
      };
    }

    case 'SUB': {
      state = {
        ...state,
        result: state.result - action.payload,
      };
    }

  }
  

  return state;
}
```