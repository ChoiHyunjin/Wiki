# Flatlist - item 사라짐 현상

Created: 2021년 10월 13일 오후 3:08
Tags: IOS, ReactNative

## 현상

![스크린샷 2021-10-13 오후 3.10.56.png](images/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2021-10-13_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_3.10.56.png)

화면 전환시 FlatList 아이템 사라짐 현상

스크롤 시 다시 그려짐

## 원인

- Flatlist - removeClippedSubviews 옵션

## 대처

- Flatlist - removeClippedSubviews 옵션 off 혹은 조건부 설정
