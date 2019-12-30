#### Redux in TS

src/types/index.tsx

```ts
export interface StoreState {
  languageName: string;
  enthusiasmLevel: number;
}
```

src/constants/index.tsx

```ts
export const INCREMENT_ENTHUSIASM = 'INCREMENT_ENTHUSIASM';
export type INCREMENT_ENTHUSIASM = typeof INCREMENT_ENTHUSIASM;
```

src/actions/index.tsx

```ts
import * as constants from '../constants';
export interface IncrementEnthusiasm {
  type: constants.INCREMENT_ENTHUSIASM;
} //..Decrement..
export type EnthusiasmAction = IncrementEnthusiasm | DecrementEnthusiasm;
export function incrementEnthusiasm(): IncrementEnthusiasm {
  return {
    type: constants.INCREMENT_ENTHUSIASM
  }
}
```

src/reducers/index.tsx

```ts
import { EnthusiasmAction } from '../actions';
import { StoreState } from '../types/index';
import { INCREMENT_ENTHUSIASM, DECREMENT_ENTHUSIASM } from '../contants/index';

export function enthusiasm(state: StoreState, action: EnthusiasmAction): StoreState {
  switch(action.type) {
    case INCREMENT_ENTHUSIASM:
      return {...state, enthusiasmLevel: state.enthusiasmLevel + 1};
    case DECREMENT_ENTHUSIASM:
      return {...state, enthusiasmLevel: Math.max(1, state.enthusiasmLevel - 1)};
  }
}
```

**component**

src/components/Hello.tsx

```tsx
export interface Props {
  name: string;
  enthusiasmLevel?: number;
  onIncrement?: () => void;
  onDecrement?: () => void;
}

function Hello({ name, enthusiasmLevel = 1, onIncrement, onDecrement }: Props){
  if(enthusiasmLevel <=0){
    throw new Error('You could be a little more enthusiastic. :D');
  }
  
  return (
  	<div className="hello">
    	<div className="greeting">
        Hello {name + getExclamationMarks(enthusiasmLevel)}
      </div>
      <div>
      	<button onClick={onDecrement}>-</button>
      	<button onClick={onIncrement}>+</button>
      </div>
    </div>
  );
}
```

**wrap it into a container**

src/containers/Holle.tsx

```tsx
import Hello from '../components/Hello';
import * as actions from '../actions/';
import { StoreState } from '../types/index';
import { connect, Dispatch } from 'react-redux';

export function mapStateToProps({enthusiasmLevel, languageName}: StoreState){
  return {
    enthusiasmLevel,
    name: languageName
  }
}
```

---

Note that **mapStateToProps** only creates 2 out of 4 of the properties a Hello component expects.

---

```tsx
export export function mapDispatchToProps(dispatch: Dispatch<actions.EnthusiasmAction>){
  return {
    onIncrement: () => dispatch(actions.incrementEnthusiasm()),
    onDecrement: () => dispatch(actions.decrementEnthusiasm()),
  }
}
```

finally

```tsx
export default connect(mapStateToProps, mapDispatchToProps)(Hello);
```

**creating a store**

src/index.tsx

```tsx
import { createStore } from 'redux';
import { enthusiasm } from './reducers/index';
import { StoreState } from './types/index';

const store = createStore<StoreState>(enthusiasm, {
  enthusiasmLevel: 1,
  languageName: 'TypeScript',
})
```

Provider

```tsx
import Hello from './containers/Hello';
import { Provider } from 'react-redux';

ReactDOM.render(
	<Provider store={store}>
  	<Hello />
  </Provider>,
  document.getElementById('root') as HTMLElement
);
```

---

Notice that Hello no longer needs props, since we used our **connect** function to adapt our application's state for our wrapped Hello component's props.

---



https://github.com/Microsoft/TypeScript-React-Starter#typescript-react-starter

