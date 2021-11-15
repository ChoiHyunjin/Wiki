# App Link ( 안드로이드 Universal Link)

Created: 2020년 8월 7일 오후 3:24
Tags: Android

> *주의 : 안드로이드 6.0 이상에서 가능*
> 

Android App Link는 Android 앱의 특정 콘텐츠로 사용자를 바로 안내하는 HTTP URL.

# 설정 과정

## 인텐트 필터 추가하기

Android 스튜디오의 App Links Assistant를 사용한다.

1. Toos > App Links Assistant 선택
2. **Open URL Mapping Editor**를 클릭한 후 **URL Mapping** 목록 하단에 있는 **Add**를 클릭하여 새 URL 매핑을 추가
3. 새 URL 매핑에 관한 세부정보를 추가
    
    ![Untitled](images/App%20Link%20(%20%E1%84%8B%E1%85%A1%E1%86%AB%E1%84%83%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8B%E1%85%B5%E1%84%83%E1%85%B3%20Universal%20Link)/Untitled.png)
    
    1. Host : 웹사이트 URL 입력
    2. Path : 매핑할 URL의 path, pathPrefix, pathPattern 을 추가
    3. Activity: URL을 통해 사용자를 안내할 Activity를 선택
4. AndroidManifest.xml에 반영되어 Preview에서 확인할 수 있다.
5. Check URL Mapping를 통해 URL 매핑이 제대로 되는 지 확인할 수 있다.

## 들어오는 링크 처리하기

이 후 인텐트 처리 로직을 추가한다.

1. App Links Assistantd에서 Select Activity를 클릭한다.
2. 목록에서 활동을 선택 하고 Insert Code를 클릭한다.

```java
// ATTENTION: This was auto-generated to handle app links.
Intent appLinkIntent = getIntent();
String appLinkAction = appLinkIntent.getAction();
Uri appLinkData = appLinkIntent.getData();
```

1. 추가된 코드를 다듬어 인텐트를 처리한다.

```java
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  ...
  handleIntent(getIntent());
}

protected void onNewIntent(Intent intent) {
  super.onNewIntent(intent);
  handleIntent(intent);
}

private void handleIntent(Intent intent) {
    String appLinkAction = intent.getAction();
    Uri appLinkData = intent.getData();
    if (Intent.ACTION_VIEW.equals(appLinkAction) && appLinkData != null){
        String recipeId = appLinkData.getLastPathSegment();
        Uri appData = Uri.parse("content://com.recipe_app/recipe/").buildUpon()
            .appendPath(recipeId).build();
        showRecipe(appData);
    }
}
```

## 앱과 웹사이트 연결하기

App Links Assistant가 [디지털 애셋 링크](https://developer.android.com/training/app-links/verify-site-associations#web-assoc) 파일을 생성한다. 혹은 Search Console에서 사이트와 앱을 연결할 수도 있다.

Assistant에서 Open Digital Asset Links File Generator를 클릭한다.

![Untitled](images/App%20Link%20(%20%E1%84%8B%E1%85%A1%E1%86%AB%E1%84%83%E1%85%B3%E1%84%85%E1%85%A9%E1%84%8B%E1%85%B5%E1%84%83%E1%85%B3%20Universal%20Link)/Untitled%201.png)

1. **Site domain** 및 **[Application ID](https://developer.android.com/studio/build/configure-app-module#set_the_application_id)**를 입력
2. [비밀번호 대용 Smart Lock](https://developers.google.com/identity/smartlock-passwords/android/associate-apps-and-sites) 지원 기능을 포함하려면 **Support sharing credentials between the app and the website**를 선택하고 사이트의 로그인 URL을 입력. 그러면 앱과 웹사이트가 로그인 사용자 인증 정보를 공유한다고 선언하는 문자열이 디지털 애셋 링크에 추가된다.
3. signing config 혹은 keystore file을 지정한다. 앱 릴리즈/디버그 빌드에 맞는 구성이나 keystore 파일을 선택해야한다.
프로덕션 : release / 개발 : debug
4. **Generate Digital Asset Links file**을 클릭하면
5. 파일이 생성되고, Save file을 클릭하여 다운로드 한다.
6. 읽기 액세스 권한을 가지도록 assetlinks.json 파일을 사이트(`https://<yoursite>/.well-known/assetlinks.json`)에 업로드한다.

## 앱 링크 테스트하기

다음 단계를 통해 앱 링크를 테스트 할 수 있다.

1. App Links Assistantdㅔ서 Test App Links를 클릭한다.
2. URL 을 입력한다.
3. Run Test를 클릭한다.
    1. URL 매핑이 설정되지 않았거나 존재하지 않는 경우 URL 아래에 오류 메시지가 표시된다.
    2. URL 매핑 존재하는 경우 device나 에뮬레이터에서 지정된 앱 활동을 시작한다.

링크 : [https://developer.android.com/studio/write/app-link-indexing.html](https://developer.android.com/studio/write/app-link-indexing.html)
