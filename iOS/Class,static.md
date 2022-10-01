# Class  vs Static 

swift에는 3가지 함수가 있다.
```swift 
class Sample {
// 1. Instance 함수
func myInstanceFunc() {}
// 2. class 함수
class func myClassFunc() {}
// 3. static 함수
static func myStaticFunc() {}
}
```

## Class func와 Static func의 차이점
Class func : 오버라이딩 가능        
Static func : 오버라이딩 불가능

```swift 
class SubSample : Sample {

override class func myClassFunc() {}
// Good

override static func myStaticFunc() {}
//Compile Error
}
```

## Class Property와 Static Property의 차이점
Class Property : 상속 가능, 연산 타입 프로퍼티로 표현되어야한다.
Static Property : 상속 불가능