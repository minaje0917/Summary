# Enum

## Enum이 뭔데?
Enum(Enumeration): 열거형!        
열거형? : 같은 주제로 연관된 데이터들을 맴버로 구성하여 나타내는 자료형      
## 선언은?
```swift 
enum game {
    case tanker
    case dealer
    case healer
}
```
사용 시에는 
```swift 
var player1: game = .tanker
var player2: game = .dealer
var player3: game = .healer
```
이런 식으로 사용 가능하다

## 원시 값이 있는 열거형?
### ① Number Type을 가지는 열거형 
```swift 
enum game: Int{
    case tanker // 0
    case dealer // 1
    case healer // 2
}
```
가장 먼저 선언된 case 부터 순차적으로 +1 씩 들어간다.      
RawValue를 직접 지정할 수도 있다.
```swift 
enum game: Int{
    case tanker = 0 // 0
    case dealer // 1
    case healer = 4 // 4
}
```
### ② String Type을 가지는 열거형 
```swift 
enum game: String{
    case tanker // tanker
    case dealer // dealer
    case healer = "heal"  // heal
}
```
### RawValue에 접근하는 방법!
```swift
var player1: game = .tanker
var player2: game = .dealer
var player3: game = .healer

player1.rawValue // tanker
player2.rawValue // dealer
player2.rawValue // heal
```

참고 : https://babbab2.tistory.com/116