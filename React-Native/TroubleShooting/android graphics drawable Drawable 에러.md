# android.graphics.drawable.Drawable 에러

Created: 2021년 8월 23일 오후 2:41
Tags: Android

## 현상

갑자기 android.graphics.drawable.Drawable 에러 발생

- 전문

    ```java
    Attempt to invoke virtual method
    'android.graphics.drawable.Drawable
    android.graphics.drawable.Drawable$ConstantState.newDrawable(android.content.res.Resources)' on a null object reference
    ```

## 대처

- npm 캐시 삭제

    *npm start -- --reset-cache*