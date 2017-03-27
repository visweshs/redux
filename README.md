Inspired by [redux-worker](https://github.com/chikeichan/redux-worker).

The root reducer's computation of the next state is an unnecessary tax (albeit small if you follow the redux design paradigm) on the main UI thread. 

This is the proposal:


```
import { createStore } from 'redux'
import { combineReducers } from 'redux-immutable'
import { Record } from 'immutable'

const StateR1 = Record({
  number: 0
})


const r1 = (state = new StateR1(), action) =>  {
  switch (action.type) {
  case 'INCREMENT': {
    const curr = state.get('number')
    return state.set('number', curr + 1)
   }
  case 'DECREMENT':
    const curr = state.get('number')
    return state.set('number', curr + 1)
   }
  default:
    return state
  }
}


const StateR2 = Record({
...
})

const r2 (state, action) => {
...
}

const rootReducer = combineReducers({ ... })

let store = createStore(rootReducer, webworker=true)
```

Work Flow: 
1) Actions are dispatched
2) The store posts the message to the rootReducer Web worker
3) A replacement root reducer that listens to messages from the rootReducer Web Worker and propogates the new state.

