# 5. reselect 패키지로 선택자 함수 만들기 (실습)

리덕스에 저장된 데이터를 화면에 보여줄 때는 다양한 형식으로 가공한다. 이때 reselect 패키지는 원본 데이터를 다양한 형태로 가공하도록 도와준다. 특히 리덕스의 데이터를 컴포넌트에서 필요한 데이터로 가공하는 용도로 많이 사용된다.

## 5.1 reselect 패키지 없이 직접 구현하기

```jsx
import React from 'react'

export default function NumberSelector({value, options, postfix, onChange}) {
  return (
    <div>
      <select onChange={e => {
        const value = Number(e.currentTarget.value)
        onChange(value)   // 1
      }}>
        {options.map(option => (                // 2
          <option key={option} value={option}>
            {option}
          </option>
        ))}
      </select>
      {postfix}
    </div>
  )
}
```

1. 사용자가 옵션을 선택하면 부모 컴포넌트에게 알려준다.
2. 부모 컴포넌트가 알려준 옵션 목록을 화면에 출력한다.

### 친구 목록의 리덕스 코드 리팩터링

```jsx
import createReducer from "../common/createReducer";
import creatItemsLogic from "../common/createItemsLogic";
import {MAX_AGE_LIMIT, MAX_SHOW_LIMIT} from "./common";
import mergeReducers from "../common/mergeReducers";

const {add, remove, edit, reducer: friendsReducer} = creatItemsLogic('friends')

const SET_AGE_LIMIT = "friend/SET_AGE_LIMIT"        // 1
const SET_SHOW_LIMIT = "friend/SET_SHOW_LIMIT"

export const addFriend = add
export const removeFriend = remove
export const editFriend = edit
export const setAgeLimit = ageLimit => ({type: SET_AGE_LIMIT, ageLimit})      // 2 
export const setShowLimit = showLimit => ({type: SET_SHOW_LIMIT, showLimit})

const INITIAL_STATE = {ageLimit: MAX_AGE_LIMIT, showLimit: MAX_SHOW_LIMIT}    // 4
const reducer = createReducer(INITIAL_STATE, {                    // 3
  [SET_AGE_LIMIT] : (state, action) => (state.ageLimit = action.ageLimit),
  [SET_SHOW_LIMIT] : (state, action) => (state.showLimit = action.showLimit),
})
export default mergeReducers([reducer, friendsReducer])             // 5
```

1. 2. 3. 연령 제한과 개수 제한 정보를 처리하는 액션 타입, 액션 생성자 함수, 리듀서 함수 작성

 4. 초기 상탯값 설정

 5. 친구 목록 처리 리듀서를 하나로 통합

### FriendMain 컴포넌트 리팩토링

```jsx
import {useEffect, useReducer} from "react";
import store from "../../common/store";
import {addFriend} from "../state";
import FriendList from "../component/FriendList";
import {getNextFriend} from "../../common/mockData";
import {shallowEqual, useDispatch, useSelector} from "react-redux";
import NumberSelector from "../component/NumberSelector";
import {MAX_AGE_LIMIT, MAX_SHOW_LIMIT} from "../common";
import * as actions from "../state";

export default function FriendMain() {
  const [ageLimit, showLimit, friendsWithAgeLimit, friendsWithShowLimit] = useSelector(state => {
    const {friends, ageLimit, showLimit} = state.friend
    const friendsWithAgeLimit = friends.filter(friend => friend.age <= ageLimit)    // 1
    return [ageLimit, showLimit, friendsWithAgeLimit, friendsWithAgeLimit.slice(0, showLimit)]    // 2
  }, shallowEqual)
  const dispatch = useDispatch()

  function onAdd() {
    const friend = getNextFriend()
    dispatch(actions.addFriend(friend))
  }

  return (
    <div>
      <button onClick={onAdd}>친구 추가</button>
      <NumberSelector                                         // 3
        onChange={v => dispatch(actions.setAgeLimit(v))}
        value={ageLimit}
        options={AGE_LIMIT_OPTIONS}
        postfix={'세 이하만 보기'}
      />
      <FriendList friends={friendsWithAgeLimit}/>      // 4
      <NumberSelector                                         // 5
        onChange={v => dispatch(actions.setShowLimit(v))}
        value={showLimit}
        options={SHOW_LIMIT_OPTIONS}
        postfix={'명 이하만 보기 (연령 제한 적용)'}/>
      <FriendList friends={friendsWithShowLimit}/>      // 6
    </div>
  )
}

const AGE_LIMIT_OPTIONS = [15, 20, 25, MAX_AGE_LIMIT]     // 7
const SHOW_LIMIT_OPTIONS = [2, 4, 6, MAX_SHOW_LIMIT]
```

1. 친구 목록에 연령 제한을 적용해서 새로운 목록을 만든다.
2. 연령 제한 된 리스트에 갯수 제한을 적용한다.
3. 연령 제한 옵션 출력. 연령 제한 옵션을 선택하면 setAgeLimit 액션이 생성되고, 리덕스의 상탯값이 바뀐다.
4. 연령 제한으로 필터링된 친구 목록
5. 개수 제한 옵션. 옵션 선택 시 setShowLimit 액션이 생성되고, 리덕스의 상탯값이 변경된다.

코드 3에서 주목할 부분은 선택자 함수의 내부 코드이다. 리덕스에 저장된 데이터를 가공하고 있다. 여기서 문제는 액션이 처리될 때마다 새로운 목록을 만들게 된다.

## 5.2 reselect 패키시 사용

### 친구 목록 데이터 선택자 함수 만들기

```jsx
import {createSelector} from "reselect";    // 1

const getFriends = state => state.friend.friends    // 2
const getAgeLimit = state => state.friend.ageLimit
const getShowLimit = state => state.friend.showLimit

export const getFriendsWithAgeLimit = createSelector(    // 3
  [getFriends, getAgeLimit],                   // 4 
  (friends, ageLimit) => friends.filter(friend => friend.age <= ageLimit))     
export const getFriendsWithAgeShowLimit = createSelector(
  [getFriendsWithAgeLimit, getShowLimit], 
  (friendsWithAgeLimit, showLimit) => friendsWithAgeLimit.slice(0, showLimit))
```

1. createSelector 함수를 이용해 선택자 함수를 만든다.
2. 코드 중복을 없애기 위해 만든 선택자 함수.
3. 연령 제한이 적용된 친구 목록을 반환해 주는 선택자 함수 정의
4. createSelector의 첫번째 인자. 배열의 각 함수가 반환하는 값이 순서대로 두번째 인자의 함수로 전달된다.

reselect 패키지는 메모이제이션 기능이 있다. 따라서 getFriendsWithAgeLimit 함수는 friends, ageLimit가 변경될 때만 연산하고, getFriendsWithAgeShowLimit friends, ageLimit, showLimit가 변경될 때만 연산한다.

### 선택자 함수 사용하기

코드 4에서 만든 선택자 함수를 사용하는 코드로 수정한다.

```jsx
export default function FriendMain() {
  const [ageLimit, showLimit, friendsWithAgeLimit, friendsWithShowLimit] = useSelector(state => [
    getAgeLimit(state),
    getShowLimit(state),
    getFriendsWithAgeLimit(state),
    getFriendsWithAgeShowLimit(state)
  ], shallowEqual)
  const dispatch = useDispatch()

//...
}
```

### 5.3 reselect에서 컴포넌트의 속성값 이용하기

선택자 함수는 속성값도 입력으로 받을 수 있다.

```jsx
export default function FriendMain({ageLimit}) {   // 1
  const [ageLimit, showLimit, friendsWithAgeLimit, friendsWithShowLimit] = useSelector(state => [
    getAgeLimit(state),
    getShowLimit(state),
    getFriendsWithAgeLimit(state, ageLimit),       //2 
    getFriendsWithAgeShowLimit(state)
  ], shallowEqual)
// ...
```

1. ageLimit를 props(속성값)으로 받는다.
2. ageLimit를 getFriendsWithAgeLimit의 두번째 인자로 전달한다.

```jsx
// ...
export const getAgeLimit = (_, ageLimit) => ageLimit
// ...
```

단순히 두 번째 인자를 반환한다.

현재까지의 코드로는 reselect에서 제공하는 메모이제이션 기능이 제대로 동작하지 않는다. FriendMain 컴포넌트 인스턴스가 서로 다른 연령 제한 속성값을 가지고 있기 때문이다.

5.4 컴포넌트 인스턴스별 독립된 메모이제이션 적용하기

```jsx
// ...
export const makeGetFriendsWithAgeLimit = () => {
  return createSelector([getFriends, getAgeLimit],
    (friends, ageLimit)=> friends.filter(friend => friend.age <= ageLimit))
}
```

선택자 함수를 생성하는 함수. 이 함수를 사용하여 각 컴포넌트 인스턴스가 자신만의 선택자 함수를 가질 수 있다.

```jsx
export default function FriendMain({ageLimit}) {   // 1
  // ...
  const getFriendsWithAgeLimit = useMemo(makeGetFriendsWithAgeLimit, [])
  const friendsWithAgeLimit = useSelector(state => getFriendsWithAgeLimit(state, ageLimit))
	// ...
}
```

makeGetFriendsWithAgeLimit 함수를 이용해서 getFriendsWithAgeLimit 함수를 생성한다. useMemo를 사용하여 getFriendsWithAgeLimit함수의 참조값이 변경되지 않도록 고정한다.