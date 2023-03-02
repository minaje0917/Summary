# DI

## What?
DI : Dependency Injection (의존성 주입)    
"<b>의존성</b>"을 클래스에 "<b>주입</b>"시키는 것이고, "<b>의존성 분리</b>"의 조건을 만족해야한다.

## Why?
장점 : 코드의 재 사용성 증가, 리펙토링 과정 수월, Unit test 용이

## 의존성?  
클래스 A가 있고 클래스 B가 있을 때, B 클래스의 값이 바뀔 때 A 클래스의 값도 함께 바뀐다면 <b>"A 클래스는 B 클래스에게 의존성을 가진다"</b> 라고 한다!
### Example
```swift 
final class A {
    var a = "A"
}

final class B {
    var b = "B"
}

final class minaje {
    var A: String?
    var B: String?

    init() {
        self.A = A().a
        self.B = B().b
    }
}

let seon = minaje()
seon.A //A
seon.B //B

//여기서 a의 값을 C로 변경한다면?

final class A {
    var a = "C"
}
 
seon.A //C
// 이렇게 seon 객체의 A 값도 C로 변경된다.
```

## 주입(Injection)
위의 Example 코드를 이렇게 수정할 수 있다.
``` Swift
final class minaje {
    var A: String?
    var B: String?
    
    init(result: String, result2: String) {
        self.A = result
        self.B = result2
    }
}
//외부에서 클래스 내부에 값을 주입
let seon = minaje(result: "A", result2: "B") 
seon.A //A
seon.B //B
```
<b> 객체의 값을 외부에서 결정한 뒤 내부에 넣어주는 것을 주입</b>이라고 한다.

## 의존성 주입 (Dependency Injection)

<b>의존성 + 주입</B>     
클래스의 의존성을 외부에서 주입하면 의존성 주입이 된다.    

``` Swift 
final class A1 {
    var a = "A"
}

final class B1 {
    var b = "B"
}

final class minaje {
    var A: String?
    var B: String?

    init(result: A1, reslut2: B1) {
        self.A = A1().a
        self.B = B1().b
    }
}

//외부에서 값 주입, 클래스의 의존 관계 생성
let seon = minaje(result: A1(), result2 : B1())
seon.A
seon.B
``` 
이 예제는 객체의 외부에서 값을 <b>주입</b>했고, 그 값은 A1과 B1 클래스에게 <b>의존성</b>을 갖는다.    

## 의존성 분리
우리는 <b>일반적으로 의존성을 주입시킨 것 만으로는 DI라고 부르지 않는다.</b>      
<b>의존성 분리</b> 조건까지 만족 시켜야 DI라고 한다.     
그리고 이 의존성 분리는 "<b>의존 역전의 원칙</b>"을 기반으로 분리를 실행한다.        

의존 역전의 원칙은 <b>SOLID 5원칙</b> 중 <b>Dependency Inversion Principle</b>을 의미한다. 

### DIP 원칙?
* 의존 관계를 맺을 땐, <b>변화하기 쉬운 것보다 변화하기 어려운 것에 의존해야한다.</b>
* 변화하기 어려운 것이란 <b>추상 클래스나 인터페이스</b>를 말하고 변화하기 쉬운 것은 <b>구체화된 클래스</b>를 의미한다.        

따라서 Swift에 Protocol을 사용한다.      
```swift 
protocol ExProtocol: AnyObject {
    var result: String { get set }
}

final class A1: ExProtocol {
    var result = "A1"
}

final class A2: ExProtocol {
    var result = "A2"
}

final class minaje {
    var A1: ExProtocol
    var A2: ExProtocol

    init (a1: A1, a2: A2) {
        self.A1 = a
        self.A2 = a2
    }
}

let seon = minaje(a1: A1(), a2: A2())
seon.A1.result // A1
seon.A2.result // A2
``` 
위 코드에서 Protocol이 바뀌면 모든 클래스들이 에러를 가진다.       
이 말의 뜻은 A1과 A2 클래스의 <b>제어 주체가 모두 "프로토콜"</b>에게 있다.        
이것이 <b>클래스에서 프로토콜로 의존의 방향이 역전</b>되었다고 한다.        
= <b>IOC(Inversion Of Control)</b>, 제어 반전이라고도 한다. 
