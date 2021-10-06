# CocoaPods could not find compatible versions for pod 에러

Created: 2021년 10월 6일 오후 1:55
Tags: Build, npm

## 현상

```jsx
[!] CocoaPods could not find compatible versions for pod "React-perflogger":
  In Podfile:
    React-cxxreact (from `../node_modules/react-native/ReactCommon/cxxreact`) was resolved to 0.66.0, which depends on
      React-perflogger (= 0.66.0)

    React-perflogger (from `../node_modules/react-native/ReactCommon/reactperflogger`)

Specs satisfying the `React-perflogger (from `../node_modules/react-native/ReactCommon/reactperflogger`), React-perflogger (= 0.66.0)` dependency were found, but they required a higher minimum deployment target.
```

## 원인

- 라이브러리가 요구하는 target의 최소 버전보다 낮아서 생기는 현상

## 대처

- xcode 에서 Deployment Targete을 높여주면 된다.