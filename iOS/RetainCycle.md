# Retain Cycle

## 1. Swift의 메모리 관리!
**Retain Cycle**을 살펴보기 전에 Swift의 **메모리 관리**에 대해 알아야한다!    
다행히도 **ARC** (Auto Reference Counting)는      
대부분의 메모리 관리를 자동으로 해줍니다!       
자! 그러면 **ARC**가 어떤 원리인지 알아봅시다.     

### 1-1. ARC 원리!
기본적으로 클래스의 객체를 가르키는 각각의 reference는 **강한 참조**다.     
최소 하나의 강한 참조가 있는 이상 객체는 메모리에서 해제되지않는다.    
하지만 객체에 대한 강한 참조가 없으면 메모리에서 해제된다.    
```swift
class TestClass {
	init() {
    	print("init")
    }
    
    deinit() {
   		print("deinit")
    }
}

var testClass: TestClass? = TestClass()
testClass = nil
```
객체를 생성한 후 객체와 참조 관계 다이어그램
![](https://velog.velcdn.com/images/minaje/post/3786a26c-a1fd-4821-84be-0d5ba076eb83/image.png)
testClass는 TestClass의 객체에 강한 참조를 가지고 있습니다.    
하지만 우리가 `testClass = nil` 이렇게 nil을 할당하면     
강한 참조는 사라지고 때문에 **TestClass 객체의 메모리는 해제**된다.

## 2. Retain Cycle!
```swift
class TestClass {
	var testClass: TestClass? = nil
    
    init() {
    	print("init")
    }
    
    deinit() {
   		print("deinit")
    }
}
var testClass1: TestClass? = TestClass()
var testClass2: TestClass? = TestClass()

testClass1?.testClass = testClass2
testClass2?.testClass = testClass1
```
TestClass 두 개는 서로를 가르키고 있다.
![](https://velog.velcdn.com/images/minaje/post/f9016efc-02eb-4576-ac23-ff93de5d57ec/image.png)
자 여기에 nil을 할당하면 deinit이 실행되고 메모리에서 해제될까요?
```swift
testClass1 = nil
testClass2 = nil
```
하지만 deinit도 실행되지않고 메모리에서도 해제가 안되었습니다!
![](https://velog.velcdn.com/images/minaje/post/33ebddb1-e034-478c-b43d-859b7c6d6afe/image.png)
그 이유는 각각의 객체는 강한 참조를 하나씩 잃었지만     
내부적으로 한개씩의 강한 참조를 가지고 있기 때문에 객체들의 메모리가 해제되지 않는다.    
심지어 두 객체에 대한 참조는 코드에서 더이상 존재하지 않는다.    
즉, 이 두 객체의 메모리를 해제하는 방법은 존재하지 않는다.     
 
이러한 현상을 **메모리 누수**라고 한다.     
어플리케이션에서 메모리 누수가 몇군데 발생하면   
사용할 때마다 메모리 사용량이 증가하고,      
사용량이 늘어나면 iOS는 앱을 kill한다.   
따라서 우리는 Retain Cycle을 잘 다뤄야합니다.     

## 3. 마무리
이번 글에서는 swift의 RetainCycle에 대해 알아보았다.     
다음 글에서는 이러한 현상을 예방하는 법을 알아봐야겠다!     
사진 출처 : https://www.thomashanning.com/retain-cycles-weak-unowned-swift/