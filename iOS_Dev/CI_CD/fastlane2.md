
# Fastlane을 활용해 아카이브 자동화하기

## Fastlane
> ruby 기반 클라이언트 자동 빌드 오픈소스 라이브러리  

### iOS 배포 과정
1. App Store Connect > 사용자 및 액세스 > Keys 이동
2. API키 생성
3. 해당 키 .p8 파일은 무조건 저장해둬야함 (생성 이후 최초 1번만 다운로드 가능)
4. 아래 예시코드를 fastFile에 추가
5. 커맨드 열고 fastlane 명령어 입력
-> 위의 iOS 앱 TestFlight 배포 과정을 **Fastlane**을 활용해 자동화할 수 있다  

## 관련 문서  
🔗[Fastlane 저장소](https://github.com/fastlane/fastlane)  

📃[Fastlane 문서](https://docs.fastlane.tools/)

## 예시 코드  
```ruby
desc "Push a new beta build to TestFlight"
  lane :beta do
    xcode_select("/Applications/Xcode_13.1.app")  
    gym(
      configuration: "AppStore",
      clean: true,
      scheme: "OneQPlus",
      include_bitcode: false,
      export_method: "app-store"
    )
    upload_TestFlight
  end
  
  lane :upload_TestFlight do
    version = get_version_number(xcodeproj: "OneQPlus.xcodeproj", target: "OneQPlus", configuration: "AppStore")

    upload_to_testflight(
      app_identifier: "com.kebhana.hanapush",
      api_key_path: "fastlane/____.json",     # ____.json파일의 이름은 키 ID로 셋팅
      apple_id: "1362508015",
      app_version: version
    )
  end
  
  
  ```
