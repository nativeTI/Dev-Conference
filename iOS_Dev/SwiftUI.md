# SwiftUI

## Async Images - concurrently load images
- 
-> async 문법, actor 타입 제공


## Async/await
### 등장배경
- 기존 SwiftUI에서 이미지 로드하는 방식 (~Swift 5.4)은 

### 사용방법
1. 
3. ex
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
