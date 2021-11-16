# Cause: error=2, No such file or directory

Created: 2021년 11월 9일 오후 4:05
Tags: Android, Build

## 현상

```java
Cause: error=2, No such file or directory
```

## 원인

- 그래들 싱크 실패
- ndk 파일 위치를 못잡는 듯

## 대처

1. invalidate caches / Restart
    
    > 안됨
    
2. File > Project Structure 에서 NDK 위치설정
    
    But Project Structure가 안들어가짐
    
    > 안드로이드 스튜디오 재 설치.
    
    Project Structure > SDK Location 에서 NDK 다운로드.
    
    이후 invalidate caches / Restart 실행