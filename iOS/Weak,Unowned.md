## 1. Weak
`Weak`를 사용하여 **강한 참조가 아닌 약한 참조**를 만들어 메모리 누수를 방지할 수 있다! 
```swift
class TestClass{
    weak var testClass: TestClass? = nil  // 약한 참조
    init(){
        print("init")
    }
    deinit(){
        print("deinit")
    }
}

var testClass1: TestClass? = TestClass()
var testClass2: TestClass? = TestClass()

testClass1?.testClass = testClass2
testClass2?.testClass = testClass1

testClass1 = nil
testClass2 = nil
```
![](https://velog.velcdn.com/images/minaje/post/6dba1e4b-c926-47a4-8fa9-737ffd149097/image.png)
약한 참조만 남아있다면 객체들의 메모리는 **해제**된다.    
즉 약한 참조는 참조는 할 수 있지만, Reference Count는 증가하지 않는다.   

weak는 객체의 메모리가 해제된 후 그에 대응하는 변수는 **자동으로 nil**이 된다.    
그러므로 모든 weak 참조 변수는 반드시 **optional** 타입이어야한다.   

## 2. Unowned
Unowned는 weak랑 매우 유사하지만 nil이 될 수 없습니다.    
따라서 Unowned 변수는 Optional로 선언되어서는 안됩니다.   

weak 사용하는 것이 보다 안전하긴 하다.    
**하지만 변수가 weak가 되기를 원하지않고**     
**해당 변수가 가리키는 객체의 메모리가 해제된 이후에는**    
**해당 영역을 가리키지 않는다는 확신이 있다면** Unowned를 사용하자!

참고 : https://www.thomashanning.com/retain-cycles-weak-unowned-swift/
