# Fastlane을 활용해 아카이브 자동화하기

## Fastlane
> ruby 기반 클라이언트 자동 빌드 오픈소스 라이브러리  

### iOS 배포 과정
1. info.plist에서 버전과 빌드버전을 높이는 작업
2. 타겟이 여러 개일 때, 모든 타겟이 버전과 빌드가 동일한지 확인
3. 1년에 한 번 씩 certificate과 provisioning profile 발급 및 공유
4. Xcode에서 archive 버튼을 누른 후 아카이브가 끝날 때까지 대기
5. archive가 완료되면 export 버튼 누르고, 배포가 끝날 때까지 대기

-> 위의 iOS 앱 배포 과정을 **Fastlane**을 활용해 자동화할 수 있다  

## 관련 문서  
🔗[Fastlane 저장소](https://github.com/fastlane/fastlane)  

📃[Fastlane 문서](https://docs.fastlane.tools/)

## 예시 코드  
```ruby
desc "archive ipa"
  lane :ipa do
	clear_derived_data
	xcode_select(<응용프로그램 내 Xcode 경로>)
	gym(
		clean: true,
		include_bitcode: false,
		output_directory: <ipa를 추출할 디렉토리 경로>,
		output_name: <ipa 이름>,
		export_method: <build configurations>,
		export_options: {
    			provisioningProfiles: { 
      				<App bundle Identifier> => <provisioning파일명>
    			}
		}
	
	)
  end
  ```
