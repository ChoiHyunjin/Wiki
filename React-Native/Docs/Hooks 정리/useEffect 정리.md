# useEffect

부수 효과(side effect) 처리에 사용되는 훅.

```jsx
function Component() {
  const [count, setCount] = useState(0)
  useEffect(() => {               // 1
    document.title = `업데이트 횟수: ${count}`
  })
  return <button onClick={() => setCount(count + 1)}>increase</button>   // 2
}
```

1. 부수 효과 함수. 렌더링 결과가 실제 돔에 반영된 후 호출되고, 컴포넌트가 사라지기 직전에 마지막으로 호출된다.
2. 버튼을 클릭할 때 마다 다시 렌더링 되고, 렌더링이 끝나면 부수 효과 함수가 호출된다.

### 의존성 배열

```jsx
function Profile({userId}) {
  const [user, setUser] = useState(null)
  useEffect(() => {             
    getUserApi(userId).then(data => setUser(data))    // 1
  }, [userId])                                 // 2
  return <div>
    {user ? (<>
      <p>{user.name}</p>
      <p>{user.age}</p>
    </>) : <p>사용자 정보 가져오는 중...</p>}
  </div>
}
```

1. 부수 효과 함수
2. useEffect의 두 번째 인자 : 의존성 배열. 배열안의 값이 변경되는 경우에만 함수가 호출된다.

### 컴포넌트가 사라지기 전 액션 설정

```jsx
function WidthPrinter() {
  const [width, setWidth] = useState(0)
  useEffect(() => {
    const onResize = () => {...
    }
    window.addEventListener('resize', onResize)
    return () => window.removeEventListener('resize', onResize)  // 1
  }, [])                                 // 2
  return <div><p>width</p></div>
}
```

1. 함수 반환. 이 반환 된 함수는 컴포넌트가 사라지기 전에 한번 호출 된다.
2. 의존성 배열로 빈 배열을 입력하면 컴포넌트가 생성될 때만 부수 효과 함수가 호출되고, 컴포넌트가 사라질 때만 반환된 함수가 호출된다.