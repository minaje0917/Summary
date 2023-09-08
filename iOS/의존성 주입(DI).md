## 🍤 개요
오늘은 드디어 아기다리 고기다리던!
DI! Dendency Injection! 단어만 들었울 때는 아주 무섭죠..!
하지만 객체지향을 한다면! 자주 사용하고 꼭 알아야하는 녀석이죠!
따라서 오늘은 DI 뿌수기! 저와 함께 가보시죠!
포스팅은 편의 말투로 진행됩니다.

## 🍦 Dendency??
Dependency 즉! 의존성이란?
객체 지향 프로그래밍에서 서로 다른 객체 사이에 의존관계가 있다는 것을 말한다!
말이 어려우니까 풀어서 설명하자면 A 객체가 B 객체를 의존한다고 가정해보자! 
이때 B 객체가 수정이 되면 A 객체까지 영향을 받는 것이다! 이것이 의존성이다.

자 코드로 살펴보자
``` swift
import UIKit

struct Study {
	func swift() {
    	print("스위프트")
    }
    
    func kotlin() {
    	print("코틀린")
    }
}

struct Minaje {
	var todayStudy: Study
    
    func ios() {
    	todayStudy.swift()
    }
    
    func aos() {
    	todayStudy.kotlin()
    }
}
```
Minaje 객체는 Study객체를 인스턴스로 사용하고 있으므로! Study객체에 의존성이 생긴다.
만약 이때 Study객체에서 오류나 수정사항이 발생한다면! Minaje 객체도 영향을 받는다.

따라서 의존성을 가지는 코드가 많아진다면, 재활용성이 떨어지고 수정 사항이 발생 시 매번 의존성을 가지는 객체들을 함께 수정해 주어야하는 번거로운 문제가 발생함!

이 상황을 해결하기 위해 나온 개념이 바로 DI(Dependency Injection) 즉 의존성 주입이다!

## 🍬 Injection??
Injection 즉 주입이란?
외부에서 객체를 생성하여 넣는 것을 의미한다.

매우 쉬우니 그냥 바로 코드를 보자
``` swift
class Study: Phone {
	var swift: String
    var kotlin: String
    
    init(swift: String, kotlin: String) {
    	self.swift = swift
        self.meal = meal
    }
    
    func studyIOS() {
    	print("스위프트")
    }
    
    func studyAOS() {
    	print("코틀린")
    }
}
```
init처럼 외부에서 값을 주입하는 것을 주입이라고 부른다!

## 🧁 Dependency Injection
자 위에서 설명했던 두 개를 합칠 시간입니다!

그러면 의존성 주입을 하는 이유를 알아봅시다!
* Unit Test가 용이해짐
* 코드의 재활용성을 높여준다
* 객체 간의 의존성을 줄이거나 없앨 수 있다.
* 객체 간의 결합도를 낮추면서 유연한 코드를 짤 수 있다.

Dependecy Injection을 해보기 전에! 진짜 마지막으로
의존 광계 역전 법칙을 알아야한다.

## 🍡 Dependency InVersion Principle
Dependency InVersion Principle 즉 의존 관계 역전 법칙은 
객체 지향 프로그래밍 설계의 5가지 원칙 SOLID 중 하나이다.

자 이제 뭔지 알아보자
> "추상화 된 것은 구체적인 것에 의존하면 안되고 구체적인 것이 추상화된 것에 의존해야 한다"

한 마디로 구체적인 객체는 추상화된 객체에  의존해야한다!

아직도 이해가 잘 안된다..! 추상화된 객체? 구체적인 객체? 뭔 말이지..
Swift로 설명해보면 추상화된 객체는 Protocol이다!

코드로 계속 이어나가보자!
``` swift
protocol App {
	func studyIOS()
    func studyAOS()
}

// Study 클래스는 App Protocol을 채택 후 App의 함수를 실체화 시킨다.

class Study: App {
	var swift: String
    var kotlin: String
    
    init(swift: String, kotlin: String) {
    	self.swift = swift
        self.kotlin = kotlin
    }
    
    func studyIOS() {
    	print("스위프트")
    }
    
    func studyAOS() {
    	print("코틀린")
    }
}

// 이제부터 중요!!
struct Minaje {
	var todayStudy: App //todayStudy변수가 추상적인 객체! App에 의존하게 된다.
    
    func studyIOS() {
    	todayStudy.studyIOS()
    }
    
    func studyAOS() {
    	odayStudy.studyAOS()
    }
    
    mutating func changeApp(app: App) {
        self.todayStudy = app
    }
}
```
이렇게 구현을 하면 Minaje 객체와 Study 객체는 거의 독립적인 객체가 된다.
둘 중 하나를 수정해도 상대 객체한테 영향을 주지 않는다!
``` swift
let app = Study(swift: "스위프트", kotlin: "코틀린")
let anotherApp = Study(swift: "오브젝티브 씨", kotlin: "자바")

var minaje = Minaje(todayStudy: app)

minaje.studyIOS() // print 스위프트
minaje.changeApp(app: anotherApp)
minaje.studyIOS() // print 오브젝티브 씨
```

## 🍯 마무리!
이렇게! 어려운 줄 알았지만 그렇게 어렵지는 않은! 의존성 주입에 대해 알아보았습니다!
다음 시간에는 이 의존성 주입을 더 편하게 할 수 있는 해주는 Swinejct에 대해 알아보도록 할게요!

Refernce - https://80000coding.oopy.io/68ee8d89-5d05-449d-87e2-5fba84d604ca