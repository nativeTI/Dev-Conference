# SwiftUI

## Async Images - concurrently load images

## Async/await
### 등장배경
기존 SwiftUI에서 이미지 로드하는 방식 (~Swift 5.4)은,
ImageLoader 옵저버블 객체를 하나 생성하여 url값 주입받으면 response값 data를 image로 변환 -> 이러한 작업을 custom 해야했음

### 사용방법
1. AsyncImage(url: URL) { UIImage } (https://developer.apple.com/documentation/swiftui/asyncimage)
2. ex
```swift
struct PhotoView: View {
  var photo: SpacePhoto
  var body: some View {
    ZStack(alignment: .bottom) {
      AsyncImage(url: photo.url) { image in
        image       // 이미지 수정사항은 AsyncImage()가 아닌 Image 객체에 적용해야함
          .resizable()
          .aspectRatio(contentMode: .fill)
      } placeholder: {
          ProgressView()
      }.frame(minWidth: 0.0, minHeight: 400.0)
      
      HStack {
        Text(photo.title)
        Spacer()
        SavePhotoButton(photo: photo)
      }
      .paddiing()
      .background(.thinMaterial)
    }
    .background(.thickMaterial)
    .mask(RoundedRectangle(cornerRadius: 16))
    .padding(.bottom, 8)
  }

}
```
