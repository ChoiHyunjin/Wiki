# Shortcut(홈 아이콘) badge 설정 안됨

Created: 2021년 7월 15일 오후 3:03
Tags: Android

## 현상

PushNotification.setApplicationIconBadgeNumber으로 뱃지 on/off가 안됨 (0개 → n개 or n개 → 0개)

뱃지가 켜져 있으면 갯수 변경은 가능

## 원인

- react-native-push-notification 라이브러리에서 미지원, ShortcutBadger 사용 권장

## 대처 ( 미해결 )

- ~~안드로이드 세팅 추가~~

    [react-native-push-notification](https://nicedoc.io/zo0r/react-native-push-notification#user-content-get-the-initial-notification)