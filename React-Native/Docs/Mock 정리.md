# Mock 정리

Created: 2021년 7월 19일 오후 1:59 Tags: Test 생성일: 2021년 7월 19일 오후 1:59

## Mocking?

Mocking은 테스트를 독립시키기 위해 의존성을 개발자가 컨트롤하고 검사할 수 있는 오브젝트로 변환 하는 테크닉이다. 무엇이든 될 수 있지만, 일반적으론 import 하는 모듈이다. 이때 의존성을 Mock
Function으로 대체한다.

## Mock Function

우리는 컨트롤할 수 없는 무엇인가를 대체하기 위해 Mocking을 사용한다. 그렇기에 Mocking은 필요로 하는 기능을 갖고 있어야한다.

Mocking 기능

- 함수 호출 Capture
- Return Value 설정
- 구현 변경

Mock 함수 인스턴스를 만드는 가장 간단한 방법은 jest.fn()이다.

```jsx
test("returns undefined by default", () => {
  const mock = jest.fn();

  let result = mock("foo");

  expect(result).toBeUndefined();
  expect(mock).toHaveBeenCalled();
  expect(mock).toHaveBeenCalledTimes(1);
  expect(mock).toHaveBeenCalledWith("foo");
});
```

다음은 return Value, 구현, Promise를 바꾸는 예제이다.

```jsx
test("mock implementation", () => {
  const mock = jest.fn(() => "bar");

  expect(mock("foo")).toBe("bar");
  expect(mock).toHaveBeenCalledWith("foo");
});

test("also mock implementation", () => {
  const mock = jest.fn().mockImplementation(() => "bar");

  expect(mock("foo")).toBe("bar");
  expect(mock).toHaveBeenCalledWith("foo");
});

test("mock implementation one time", () => {
  const mock = jest.fn().mockImplementationOnce(() => "bar");

  expect(mock("foo")).toBe("bar");
  expect(mock).toHaveBeenCalledWith("foo");

  expect(mock("baz")).toBe(undefined);
  expect(mock).toHaveBeenCalledWith("baz");
});

test("mock return value", () => {
  const mock = jest.fn();
  mock.mockReturnValue("bar");

  expect(mock("foo")).toBe("bar");
  expect(mock).toHaveBeenCalledWith("foo");
});

test("mock promise resolution", () => {
  const mock = jest.fn();
  mock.mockResolvedValue("bar");

  expect(mock("foo")).resolves.toBe("bar");
  expect(mock).toHaveBeenCalledWith("foo");
});
```

## 의존성 주입

Mock 함수를 사용하는 일반적인 방법 중 하나는 Mock 함수를 테스트 하려는 함수의 arguments로 직접 전달하는 방식입니다. 이렇게 테스트를 실행시키면, Mock 함수가 어떤 arguments와 어떻게
실행됐는지 assert 구문으로 확인할 수 있다.

```jsx
const doAdd = (a, b, callback) => {
  callback(a + b);
};

test("calls callback with arguments added", () => {
  const mockCallback = jest.fn();
  doAdd(1, 2, mockCallback);
  expect(mockCallback).toHaveBeenCalledWith(3);
});
```

이 전략은 견고하지만, 코드가 의존성 주입을 지원해야한다. 이를 만족하지 않는 경우 때문에, 실존하는 모듈과 함수를 mock하는 도구가 필요하다.

## Mocking Modules and Functions

Jest에는 다음 세 종류의 모듈과 함수 mocking 타입이 있다.

- jest.fn : 함수 Mock
- jest.mock : 모듈 Mock
- jest.spyOn : 함수 Mock 혹은 Spy

이것들은 각각 Mock Function을 어떤 방법으로 생성한다. 이를 설명하기 위해 다음과 같은 프로젝트 구조가 있다고 생각하자.

```
├ example/
| └── app.js
| └── app.test.js
| └── math.js
```

이 설정에서는 app.js를 테스트하는 일반적인 방법인데, 실제로 math.js 함수를 호출하지 않거나, 실제로 호출하는지 확인하기 위한 spy하기를 원한다. 이 예제는 간단하지만, math.js가 복잡한
계산이거나, 피하고 싶은 IO 처리 요청이라고 생각해보라.

```jsx
// math.js
export const add = (a, b) => a + b;
export const subtract = (a, b) => b - a;
export const multiply = (a, b) => a * b;
export const divide = (a, b) => b / a;

// app.js
import * as math from './math.js';

export const doAdd = (a, b) => math.add(a, b);
export const doSubtract = (a, b) => math.subtract(a, b);
export const doMultiply = (a, b) => math.multiply(a, b);
export const doDivide = (a, b) => math.divide(a, b);
```

## Mock a function with jest.fn

Mocking 가장 기본 전략은 함수를 Mock 함수로 재할당하는 것이다. 그러면, 재할당된 함수가 어디에서 사용되든, mock 함수는 원래 함수 대신해 사용될 것이다.

```jsx
import * as app from "./app";
import * as math from "./math";

math.add = jest.fn();
math.subtract = jest.fn();

test("calls math.add", () => {
  app.doAdd(1, 2);
  expect(math.add).toHaveBeenCalledWith(1, 2);
});

test("calls math.subtract", () => {
  app.doSubtract(1, 2);
  expect(math.subtract).toHaveBeenCalledWith(1, 2);
});
```

이 mocking은 다음 이유들로 덜 사용된다.

- jest.mock : 자동으로 모듈의 모든 함수를 mocking 한다.
- jset.spyOn : 같은 것을 mocking 하면서, 원래 함수를 복원할 수도 있다.

## Mock a function with jest.mock

더 일반적인 접근은 jest.mock을 사용하는 것이다. 이 방식은 모듈의 모든 export 를 자동으로 mock 함수로 세팅해준다. 따라서, jest.mock('./math.js')와 같이 호출하면 다음처럼
세팅된다.

```jsx
export const add = jest.fn();
export const subtract = jest.fn();
export const multiply = jest.fn();
export const divide = jest.fn();
```

여기서 우리는 모듈의 모든 export들의 Mock 함수를 사용할 수 있다

```jsx
import * as app from "./app";
import * as math from "./math";

// Set all module functions to jest.fn
jest.mock("./math.js");

test("calls math.add", () => {
  app.doAdd(1, 2);
  expect(math.add).toHaveBeenCalledWith(1, 2);
});

test("calls math.subtract", () => {
  app.doSubtract(1, 2);
  expect(math.subtract).toHaveBeenCalledWith(1, 2);
});
```

이 방식이 가장 쉽고 보편적인 형태의 Mocking이다. ( Jest의 *automock: true* 설정이기도 하다. )

이 전략의 유일한 단점은 모듈의 원래 구현에 접근하기가 어렵다는 점이다. 그래서 spyOn을 사용한다.

## Spy or mock a function with jest.spyOn

때로 원래 구현을 유지하면서, 메소드가 호출 되었는지를 보고싶을 때가 있다. 때로는 구현을 mock 하고, 다시 복원하고 싶을 때도 있다. 이 때 spyOn을 사용한다.

단순히 'spy'를 math 함수에 호출하지만, 원래 구현은 남겨둔다.

```jsx
import * as app from "./app";
import * as math from "./math";

test("calls math.add", () => {
  const addMock = jest.spyOn(math, "add");

  // calls the original implementation
  expect(app.doAdd(1, 2)).toEqual(3);

  // and the spy stores the calls to add
  expect(addMock).toHaveBeenCalledWith(1, 2);
});
```

이는 실제 함수 대체 없이, 특정 사이드 이펙트 발생을 경고하고 싶을 때 유용하다.

함수를 mock하고, 원래 구현을 복원 할 수도 있습니다.

```jsx
import * as app from "./app";
import * as math from "./math";

test("calls math.add", () => {
  const addMock = jest.spyOn(math, "add");

  // override the implementation
  addMock.mockImplementation(() => "mock");
  expect(app.doAdd(1, 2)).toEqual("mock");

  // restore the original implementation
  addMock.mockRestore();
  expect(app.doAdd(1, 2)).toEqual(3);
});
```

이 방식은 같은 파일에서 테스트할 때 유용하지만, Jest의 각 테스트 파일은 샌드박스화 되어 있기에 afterAll hook에서는 불필요하다.

기억해야할 점은 기본적인 jest.fn() 사용은 sugar(syntactic sugar: 읽고 쓰기 쉽게 디자인된 문법)라는 점이다. 우리는 원래 구현을 저장하고, mock 구현을 원래의 것에 세팅하고, 후에
원래의 것으로 재할당하므로써 동일한 목표를 달성할 수 있다.

```jsx
import * as app from "./app";
import * as math from "./math";

test("calls math.add", () => {
  // store the original implementation
  const originalAdd = math.add;

  // mock add with the original implementation
  math.add = jest.fn(originalAdd);

  // spy the calls to add
  expect(app.doAdd(1, 2)).toEqual(3);
  expect(math.add).toHaveBeenCalledWith(1, 2);

  // override the implementation
  math.add.mockImplementation(() => "mock");
  expect(app.doAdd(1, 2)).toEqual("mock");
  expect(math.add).toHaveBeenCalledWith(1, 2);

  // restore the original implementation
  math.add = originalAdd;
  expect(app.doAdd(1, 2)).toEqual(3);
});
```
이 것이 실제로 jest.spyOn이 구현된 방식이다.

## 결론

Mock 함수에 대한 것, 그리고 재할당 모듈과 함수의 호출 추적, 구현 교체, 값 반환 세팅에 대한 전략들에 대해 알아보았다.
[Understanding Jest Mocks](https://medium.com/@rickhanlonii/understanding-jest-mocks-f0046c68e53c)
