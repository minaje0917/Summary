# Property

## 1. 저장 프로퍼티(Stored Properties)

단순히 연산 값을 저장하는 프로퍼티이다.     
이니셜라이저를 자동으로 생성한다.
```swift 
struct minjae {
    var age: Int //Stored Property
    var name: String //Stored Property
}
let human: minjae = minjae(age: 18, name: "선민재")
```

## 2. 지연 저장 프로퍼티(Lazy Stored Properties)
lazy라는 키워드를 사용해서 선언, 호출이 있어야 값을 초기화함.      
상수는 인스턴스가 완전히 생성되기 전에 초기화되야하므로 변수를 사용    
잘 사용하면 불필요한 성능 저하나 공간 낭비를 줄일 수 있다.     
```swift
struct minjae {
    var age: Int //Stored Property
    var name: String //Stored Property
}

class School {
    lazy var student: minjae = minjae()
    let people: Int

    init(people: Int) {
        self.people = people
    }
}

let school: School = School(people: 19) //School이라는 객체를 생성됬지만 아직 minjae 객체에 생성되지 않은 상태.

print(School.student) //이 코드를 통해 student프로퍼티로 처음 접근할 때 student 프로퍼티의 minjae가 생성된다.
```

## 3. 연산 프로퍼티 (Computed Proteries)
저장 프로퍼티와 달리 저장공간을 가지지않는다.     
다른 저장프로퍼티의 값을 읽어 연산하거나, 프로퍼티로 전달받은 값을 다른프로퍼티에 저장한다.     
때문에 항상 var로 선언.     
```swift 
class Person {
    var name: String = "Sodeul"
 
    var alias: String {
        get { //getter (다른 저장 연산프로퍼티의 값을 얻거나 연산하여 리턴할 때 사용)
            return self.name + " 바보"
        }
        set(name) {  //setter (다른 저장프로퍼티에 값을 저장할 때 사용)
            self.name = name + "은 별명에서 지어진 이름"
        }
    }
}
```
name과 같이 읽거나 쓸 수 있는 "저장 프로퍼티"가 존재해야한다.

```swift 
let sodeul: Person = .init()
 
// get에 접근
print(sodeul.alias) // Sodeul 바보
 
// set에 접근
sodeul.alias = "소들"
print(sodeul.name) // 소들은 별명에서 지어진 이름
```
set은 생략이 가능하지만 get은 생략이 불가능.    

## 4. 타입 프로퍼티 (Type Properties)
정의 : 저장 타입 프로퍼티와 연산 타입 프로퍼티가 존재.      
저장 타입 프로퍼티의 경우 선언할 당시에 값이 항상 초기화 되어 있어야한다.     
"static"을 이용해서 선언, 자동으로 lazy로 작동.      
저장 타입 프로퍼티, 연산 타입 프로퍼티: 저장 프로퍼티, 연산프로퍼티에 static이랑 키워드를 붙이면된다.
``` swift
class Person {
    static name: String = "minjae" //저장 타입 프로퍼티
    static minjae: String { 
        return name + "은 바보" //연산 타입 프로퍼티
    }
}

Person.name //접근방식

class Human {
    static var name = "minjae"
    static let age = 18
}
 
Human.name                  // "minjae"
Human.name = "Unknown"
Human.name                  // "Unknown"
```

```swift
class Human {
    class var alias: String {
        return "Human Type Property"
    }
}
 
class Sodeul: Human {
    override class var alias: String {
        return "Sodeul Type Property"
    }
}
 
Human.alias             // "Human Type Property"
Sodeul.alias            // "Sodeul Type Property"
```
이렇게 class로 선언한 연산 프로퍼티의 경우도 연산 타입 프로퍼티이다.     
Subclass에서 위처럼 연산 타입 프로퍼티를 오버라이딩해서 쓸 수 있다.
하지만 static으로 선언한 경우 오버라이딩이 안된다.     

## 5. 프로퍼티 옵저버 (Property Observer)
정의 : 프로퍼티 값의 변화를 관찰하는 것, "저장 프로퍼티"에 추가 가능.     
새 값의 속성이 현재와 같더라도 속성 값이 설정되면 호출한다.      
종류 : willSet, didSet

### 1. willSet
값이 변경되기 직전에 호출된다.
```swift
var name: String = "Unknown" {
    willSet(newName) {
        print("현재 이름 = \(name), 바뀔 이름 = \(newName)")
    }
}
```

### 2. didSet 
값이 저장된 직후에 호출된다.      
```swift
var name: String = "Unknown" {
   didSet {
        print("현재 이름 = \(name), 바뀌기 전 이름 = \(oldValue)")
    }
}
```
### 3. 연산 프로퍼티에 프로퍼티 옵저버 적용하기 
"저장 프로퍼티"에만 추가할 수 있다며!     
사실 연산 프로퍼티에도 적용할 수 있다. 하지만 조건이 있다.       
부모 클래스의 연산 프로퍼티를 오버라이딩 할 경우 프로퍼티 옵저버에 추가할 수 있다.
```swift 
class Human {
    var name = "Unknown"
    var alias: String {
        get {
            return name + " 바보"
        }
        set {
            name = newValue + "별명에서 붙여진 이름"
        }
        willSet { }       // error! 'willSet' cannot be provided together with a getter
        didSet  { }       // error! 'didSet' cannot be provided together with a getter
    }
}


class Sodeul: Human {
    override var alias: String {
        willSet {
            print("현재 alias = \(alias), 바뀔 alias = \(newValue)")
        }
        didSet {
            print("현재 alias = \(alias), 바뀌기 전 alias = \(oldValue)")
        }
    }
}

let sodeul: Sodeul = .init()
sodeul.alias = "소들이"
```