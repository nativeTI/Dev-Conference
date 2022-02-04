# Concurrency (동시성)

## 동시성의 목표
- 쉽고 편리한 비동기 프로그래밍
- 컴파일 시점의 비동기 코드의 성능 향상
- 불안정한 메모리 관리 방식 개선 (data race, deadlock 제거)
-> async 문법, actor 타입 제공


## Async/await
### 등장배경
- Closure 및 completion Handler를 사용하는 asynchronous(비동기) 처리가 많이 사용 됨
- 비동기 작업, 오류처리, 비동기 호출 간의 흐름이 복잡해질 때 문제가 발생
- 비동기 처리 시, deeply-nested closure 발생
- 콜백에 대한 오류처리가 복잡 + 길어짐
- 비동기 호출 간의 제어 흐름이 복잡함
- 실수하기 쉬움 (ex. completion호출해야하는데 그냥 return 해버릴 경우)

### 해결방안
- async-await proposal에 coroutine model 도입  
-> 비동기 함수의 semantics 정의, 하지만 동시성을 제공하진 않음  
-> 비동기 코드를 동기 코드인것 처럼 작성 가능  

### 사용방법
1. completion -> return
2. async 키워드 추가, 사용하는 쪽에서 await와 함께 사용
3. ex
```swift
let photoNames = await listPhotos(inGallery: "Summer Vacation")
let sortedNames = photoNames.sorted()
let name = sortedNames[0]
let photo = await downloadPhoto(named: name)
show(photo)
```
### 특징
1) async 선언 시, 함수를 비동기 함수로 만듦
2) 프로토콜에서도 사용 가능
3) async와 throw를 같이 쓸 수 있다 (단, async 키워드가 throws 키워드보다 먼저 선언되어야 함)
4)  비동기 함수가 에러를 낼 수 있다면 await에 try를 함께 써줘야한다
5) await 키워드는 비동기 메소드가 일시 중단(suspend) 될 수 있는 지점을 나타냄

-> 성공, 실패 여부 상관 없이 최종 결과가 준비되었을 때, 그 후에야 함수가 재개(resume)됨  
-> 비동기 함수는 일시 중지된 동안, 리소스를 사용하지 않음 (스레드 차단 x)  
-> Swift runtime은 함수가 다른 작업을 위해 실행중인 스레드를 재사용할 수 있음  
-> 많은 비동기 프로세스 간에 매우 적은 스레드를 공유할 수 있음  


