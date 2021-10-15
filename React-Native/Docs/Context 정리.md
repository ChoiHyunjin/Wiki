# Context 정리

Created: 2021년 10월 15일 오전 10:11
Tags: React, React-Native
링크: https://ko.reactjs.org/docs/context.html
생성일: 2021년 10월 15일 오전 10:11

React 애플리케이션에서 데이터는 부모에서 자식으로 props를 통해 전달되지만, 여러 컴포넌트에게 전달하는 경우 번거롭다. **Context**는 트리 단계마다 명시적으로 props를 넘겨주지 않아도 많은 컴포넌트가 이러한 값을 공유하도록 할 수 있다.

## 언제 써야할까

context는 react 컴포넌트 트리 안에서 전역(global)적으로 데이터를 공유할 수 있는 방법이다. 그러한 데이터로는 로그인한 유저, 테마, 선호 언어 등이 있을 수 있다.

```jsx
// non context
class App extends React.Component {
  render() {
    return <Toolbar theme="dark" />;
  }
}

function Toolbar(props) {
  // Toolbar 컴포넌트는 불필요한 테마 prop를 받아서
  // ThemeButton에 전달해야 합니다.
  // 앱 안의 모든 버튼이 테마를 알아야 한다면
  // 이 정보를 일일이 넘기는 과정은 매우 곤혹스러울 수 있습니다.
  return (
    <div>
      <ThemedButton theme={props.theme} />
    </div>
  );
}

class ThemedButton extends React.Component {
  render() {
    return <Button theme={this.props.theme} />;
  }
}

// use context
// context를 사용하면 모든 컴포넌트를 일일이 통하지 않고도
// 원하는 값을 컴포넌트 트리 깊숙한 곳까지 보낼 수 있습니다.
// light를 기본값으로 하는 테마 context를 만들어 봅시다.
const ThemeContext = React.createContext('light');

class App extends React.Component {
  render() {
    // Provider를 이용해 하위 트리에 테마 값을 보내줍니다.
    // 아무리 깊숙히 있어도, 모든 컴포넌트가 이 값을 읽을 수 있습니다.
    // 아래 예시에서는 dark를 현재 선택된 테마 값으로 보내고 있습니다.
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

// 이젠 중간에 있는 컴포넌트가 일일이 테마를 넘겨줄 필요가 없습니다.
function Toolbar() {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class ThemedButton extends React.Component {
  // 현재 선택된 테마 값을 읽기 위해 contextType을 지정합니다.
  // React는 가장 가까이 있는 테마 Provider를 찾아 그 값을 사용할 것입니다.
  // 이 예시에서 현재 선택된 테마는 dark입니다.
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}
```

## 사용 전 고려사항

### context보다 컴포넌트 합성이 더 간단한 해결책일 수 있다.

```jsx
//befor
<Page user={user} avatarSize={avatarSize} />
// ... 그 아래에 ...
<PageLayout user={user} avatarSize={avatarSize} />
// ... 그 아래에 ...
<NavigationBar user={user} avatarSize={avatarSize} />
// ... 그 아래에 ...
<Link href={user.permalink}>
  <Avatar user={user} size={avatarSize} />
</Link>

//after
function Page(props) {
  const user = props.user;
  const userLink = (
    <Link href={user.permalink}>
      <Avatar user={user} size={props.avatarSize} />
    </Link>
  );
  return <PageLayout userLink={userLink} />;
}

// 이제 이렇게 쓸 수 있습니다.
<Page user={user} avatarSize={avatarSize} />
// ... 그 아래에 ...
<PageLayout userLink={...} />
// ... 그 아래에 ...
<NavigationBar userLink={...} />
// ... 그 아래에 ...
{props.userLink}
```

- before : 실제 사용되는 곳은 Avatar 컴포넌트 뿐인데, 여러 단계에 걸쳐 props를 보내줘야한다.
- after :  Avatar 컴포넌트 자체를 넘겨, **context를 사용하지 않고** 해결했다. Avatar 컴포넌트의 props 를 아는 건 최상단의 Page 뿐입니다.

## API

- createContext
    
    ```jsx
    const MyContext = React.createContext(defaultValue);
    ```
    
    Context 객체를 만듭니다. 트리 상위에서 가장 가까이에 있는 짝이 맞는 Provider로부터 값을 읽습니다.
    
    defaultValue 매개변수는 트리 안에서 적절한 Provider를 **찾지 못했을 때만** 쓰이는 값입니다. Provider로 *undefined*를 값으로 보낸다고 해도 defaultValue를 읽지는 않는다는 점 유의해야 합니다.
    
- Context.Provider
    
    ```jsx
    <MyContext.Provider value={/* 어떤 값 */}>
    ```
    
    Context에 포함된 React 컴포넌트인 Provider는 context를 구독하는 컴포넌트들에게 context의 변화를 알리는 역할을 합니다.
    
    Provider는 value prop을 받아 하위 컴포넌트들에게 전달합니다.
    
    Provider 하위의 context를 구독하는 모든 컴포넌트는 value가 바뀔 때마다 다시 렌더링됩니다. 이때 shouldComponentUpdate 메서드가 적용되지 않습니다. context 값 변화 여부는 Object.is와 동일한 알고리즘을 사용합니다.
    
- Class.contextType
    
    ```jsx
    class MyClass extends React.Component {
      componentDidMount() {
        let value = this.context;
        /* MyContext의 값을 이용한 코드 */
      }
      componentDidUpdate() {
        let value = this.context;
        /* ... */
      }
      componentWillUnmount() {
        let value = this.context;
        /* ... */
      }
      render() {
        let value = this.context;
        /* ... */
      }
    }
    MyClass.contextType = MyContext;
    ```
    
    Context 객체를 원하는 클래스의 contextType 프로퍼티로 지정할 수 있습니다. 이 후 this.context를 이용해 가장 가까운 Provider의 Context 값을 읽을 수 있게 됩니다.
    
- Context.Consumer
    
    ```jsx
    <MyContext.Consumer>
      {value => /* context 값을 이용한 렌더링 */}
    </MyContext.Consumer>
    ```
    
    context 변화를 구독하는 컴포넌트입니다. 이를 사용하면 함수 컴포넌트안에서 context를 구독할 수 있습니다.
    
    Consumer의 자식은 함수여야합니다. context의 현재값을 받고, React 노드를 반환합니다.
    
    이 Consumer를 이용하면 여러 Context를 구독할 수 있습니다.
    
- Context.displayName
    
    React DevTools 에서 context를 어떻게 보여줄 지 설정합니다.
    
    ```jsx
    const MyContext = React.createContext(/* some value */);
    MyContext.displayName = 'MyDisplayName';
    
    <MyContext.Provider> // "MyDisplayName.Provider" in DevTools
    <MyContext.Consumer> // "MyDisplayName.Consumer" in DevTools
    ```