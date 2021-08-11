# Redux 정리

Created: 2021년 8월 10일 오후 5:01
Tags: React, React-Native

## 0. context API와의 차이점

### 1. 미들웨어

리덕스의 미들웨어를 사용하면 액션 객체가 리듀서에서 처리되기 전에 다음과 같은 작업들을 수행 할 수 있다.

- 특정 조건에 따라 액션이 무시
- 액션을 콘솔에 출력하거나, 서버에 로깅
- 액션이 디스패치 됐을 때 이를 수정해서 리듀서에 전달
- 특정 액션 발생 시 다른 액션 발생
- 특정 액션 발생 시 특정 자바스크립트 함수 실행

### 2. 유용한 함수와 Hooks

Context API는 Context 생성, Provider 설정, 전용 Hook들을 따로 만들어 사용

반면에 리덕스는 비슷한 작업을 편리하게 해줌

- connect  함수를 사용해 리덕스의 상태, 액션 생성 함수를 컴포넌트의 props로 받을 수 있으며, useSelector, useDispatch, useStore와 같은 Hooks로 손쉽게 상태 조회, 액션 디스패치 가능
- connect와 useSelector는 내부적으로 최적화가 잘 이루어져 실제 상태가 바뀔때만 컴포넌트 리렌더링

## 6.2 리덕스의 주요 개념

![Redux%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5/Untitled.png](Redux%20%E1%84%8C%E1%85%A5%E1%86%BC%E1%84%85%E1%85%B5/Untitled.png)

그림 6-1 리덕스에서 상탯값이 변경되는 과정

> View == Component

### 6.2.1 액션

액션은 **type 속성값**을 지닌 자바스크립트 객체다.

```jsx
// bad
store.dispatch({type: 'ADD', title: '영화 보기', priority: 'high'}) // 1

// good
store.dispatch({type: 'todo/ADD', title: '영화 보기', priority: 'high'}) // 2
store.dispatch({type: 'todo/REMOVE', id: 123})
store.dispatch({type: 'todo/REMOVE_ALL'})
```

type 속성값은 고유해야 하기 때문에 2와 같이 접두사를 붙이는 방법이 많이 사용된다.

dispatch 메서드를 호출할 때 직접 액션 객체를 입력하는 방법은 지양하는게 좋다. 예제에서 'todo/ADD' 액션의 경우 title, priority 속성값이 항상 존재하도록 강제할 필요가 있다.

```jsx
export const ADD = 'todo/ADD'                   // 1
export const REMOVE = 'todo/REMOVE'
export const REMOVE_ALL = 'todo/REMOVE_ALL'

export function addTodo({title, priority}){
	return { type: ADD, title, priority})  // 2
}
export function removeTodo({id}){
	return { type: REMOVE, id})  // 2
}
export function removeAllTodo(){
	return { type: REMOVE_ALL})  // 2
}

store.dispatch(addTodo({title: '영화 보기', priority: 'high'}))  // 3
store.dispatch(removeTodo({id: 123}))                         // 3
store.dispatch(removeAllTodo())                               // 3
```

type 이름을 상수 변수로 만들고, 리듀서에서도 사용하기 위해 export한다(1). 세 개의 액션 생성자 함수를 정의하고 마찬가지로 리듀서에서 사용하기 위해 export(2). 그 후 dispatch 메서드에 액션 생성자 함수를 이용한다(3).

### 6.2.2 미들웨어

미들웨어는 리듀서가 액션을 처리하기 전에 실행되는 함수. 로그 출력, 예외 처리 등으로 활용 가능.

```jsx
const middleware = store => next => action => next(action)

///////

const middleware = function(store){
	return function(next){
		return funcion(action){
			return next(action)
		}
	}
}
```

미들웨어는 함수 세개가 중첩된 구조이다. next 함수를 호출하면 다른 미들웨어 함수가 호출되면서 최종적으로 리듀서 함수가 호출된다. 코드 6-1은 아무런 작업이 next를 호출하기에 무의미한 미들웨어 함수다.

```jsx
import { createStore, applyMiddleware } from 'redux'
const middleware1 = store => next => action => { // 1
	console.log('m1 start')
	const result = next(action)
	console.log('m1 end')
	return result
}
const middleware2 = store => next => action => {
	console.log('m2 start')
	const result = next(action)
	console.log('m2 end')
	return result
}
const reducer = (state, action) => { // 2
	console.log('reducer')
	return state
}
const store = createStore(myReducer, applyMiddleware(middleware1, middleware2)) // 3
store.dispatch({type: 'someAction'})  // 4

// middleware1 start
// middleware2 start
// reducer
// middleware2 end
// middleware1 end
```

(1)간단한 두개의 미들웨어를 정의했다. (2아무 일도 하지 않는 리듀서. (3)applyMiddleware 함수를 이용해서 미들웨어가 입력된 스토어 생성. (4)액션을 dispatch하면 로그가 출력된다.

1. middleware1의 next 함수는 middleware2 함수 실행. 
2. middleware2의 next 함수는 스토어가 갖고 있던 dispatch 메서드 호출
3. 최종적으로 스토어의 dispatch 메서드는 리듀서 호출

---

- 리덕스의 applyMiddleware 함수

    ```jsx
    const applyMiddleware = (...middlewares) => createStore => (...args) => {
    	const store = createStore(...args)   // 1
    	const funcsWithStore = middlewares.map(middleware => middleware(store))  // 2
    	const chainedFunc = funcsWithStore.reduce((a,b) => next => a(b(next))) // 3
    	return {...store, 
    		dispatch: chainedFunc(store.dispatch) // 4
    	}
    }
    ```

    1. createStore함수를 호출해서 스토어 생성
    2. 생성된 스토어와 함께 모든 미들웨어의 첫 번째 함수 호출
    3. 모든 미들웨어의 두 번째 함수를 체인으로 연결. 만약 미들웨어가 세 개였다면 next ⇒ a(b(c(next)))
    4. 외부에 노출되는 스토어의 dispatch 메서드는 미들웨어가 적용된 버전으로 변경된다.(즉 a(b(next)) )

    ```jsx
    function dispatch(action) {
    	currentState = currentReducer(currentState, action)   // 1
    	for(let i = 0; i< listeners.length; i++){  
    		listeners[i]()   // 2
    	}
    	return action
    }
    ```

    1. 리듀서 함수를 호출해서 상탯값 변경
    2. dispatch 메서드가 호출될 때 마다 등록된 모든 이벤트 처리 함수 호출. 상태값이 변하지 않아도 이벤트 처리 함수를 호출한다. 

---

**미들웨어의 활용**

- setTimeout 사용으로 실행 딜레이 추가

### 6.2.3 리듀서

리듀서는 액션이 발생했을 때 새로운 상탯값을 만드는 함수.

```jsx
(state, action) => nextState
```

```jsx
function reducer(state = INITIAL_STATE, action) {    // 1
	switch(action.type){
	case REMOVE_ALL:    // 2
	return {            // 3
		...state, 
		todos: []
	}
	case REMOVE_ALL:
	return {
		...state, 
		todos: state.todos.filter(todo => todo.id !== action.id)
	}
	default:
		return state // 4
}

const INITIAL_STATE = { todos: [] }
```

1. 초기 상탯값을 정의
2. 액션 타입별 case
3. 상탯값은 불변 객체로 관리해야하므로 수정 할 때 마다 새로운 객체 생성
4. 처리할 액션 없으면 상태값 유지
