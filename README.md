## Store


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