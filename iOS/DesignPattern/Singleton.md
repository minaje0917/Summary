# Singleton

## What??
특정용도로 객체를 하나만 생성하여, 공용으로 사용하고 싶을 때 사용하는 디자인 유형       

## How??
```swift 
class Example {
    //공용으로 사용되어야하니 전역으로 static을 사용해 Instance를 저장할 프로퍼티 하나를 생성
    static let shared = Example()

    var h1: String?
    var h2: String?
    var h3: String?

    //혹시라도 Instance를 호출해 Instance를 또 생성하는 것을 막기 위해 private으로 init() 함수접근제어자를 지정
    private init() {}
}
```      
### 외부에서 Example class 접근 방법!
```swift 
// A VC
let Example = Example.shared
Example.h1 = "h1"


// B VC
let Example = Example.shared
Example.h2 = "h2"


// C VC
let Example = Example.shared
Example.h3 = "h3"
```
어느 클래스에서든 shared라는 static 프로퍼티에 접근하면 하나의 인스턴스를 공유하는 것이다.