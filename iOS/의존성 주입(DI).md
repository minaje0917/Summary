# 의존성 주입(DI)

## 의존성(Dependency)??
함수에 필요한 클래스나 참조 변수에 의존하는 것      
```swift
class minaje{
    var name: String = "선민재"
}
class class{
    var student = minaje()
}

let school = class()
print(school.student.name) // 선민재
```
class는 minaje 내부의 변수를 사용하고 있다.      
이로써 class는 minaje클래스에 의존성이 생긴다.       
