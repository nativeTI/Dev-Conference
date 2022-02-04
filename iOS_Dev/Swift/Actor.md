# Actor

## 등장배경
concurrent program의 등장으로, async/await을 통해 여러 스레드에서 작업을 수행할 수 있게 되었는데, 여러 문제가 발생  
1. 데이터 공유 시, 데이터가 일치하지 않을 경우  
2. 데이터 공유 시, 데이터가 손상될 경우. 
3. 두 개의 다른 스레드가 동시에 같은 데이터에 접근할 때, 그 중 하나가 write인 경우. 
=> Actor가 등장

## 특징
```swift
class Counter { 
  var count: Int = 0
  func increment() { 
    self.count += 1 
  }
}
```
1) 위의 코드는 multi-thread에서 동작하지 않음
2) 2개 이상의 스레드가 increment() 호출 시, corruption이 발생함
-> actor 사용 시, corruption을 방지할 수 있음  

```swift
struct Counter { 
  var count: Int = 0
  func increment() -> Int { 
    value += 1
    return value
  }
}

let counter = Counter()
asyncDetached {
  print(counter.increment())
}

asyncDetached {
  print(counter.increment())
}
```
counter의 값을 1, 2 또는 2,1을 보장받을 수 있음

1) actor는 shared mutable state를 위한 동기화를 제공하고, 다른 프로그램으로부터 독립된 자신만의 state를 가짐
2) actor는 특정한 변경의 수행이 안전할 때까지 corruption을 일으킬 수 있는 작업을 일시중단함
3) state에 접근하기 위해선 actor를 통해서만 사용 가능
4) struct, enum, class와 비슷한 capabilities를 가짐
5) reference type

## Main Actor
### 개념
main thread에서 동작을 보장하는 actor
### 특징
```swift
@MainActor func checkedOut(_ booksOnLoan: [Book]) {
  booksView.checkedOutBooks = booksOnLoan
}

await checkedOut(booksOnLoan)
```
1)  main actor는 DispatchQueue.main에서 동기화를 수행함
2)  @MainActor 밖에서 실행될 경우 await이 필요

