# Optional


## 1. What? 
Swift에서 Optional은 nil을 가질 수 있다.      
반대로 Optional이 아니라면 nil을 가질 수 없다.      

## 2. 선언 

```swift
var name: String?
```
Optional은 ```?```키워드를 사용한다.      
Optional의 기본 값은 nil을 가진다.       


## 3. 이용

Optional변수를 이용해서 작업할 때에는 Optional을 해제하는 과정이 필요하다.       
### 3-1. 옵셔널 해제

```swift
var num1 : Int = 100
var num2? : Int = 130

if num2{
    let sum = num2! + num2!
}
```
if로 num2가 nil인지 판단을 하고 ```!``` 키워드를 사용하여 강제로 값을 빼온다.    
만약 if로 nil을 체크하지 않고 ```!``` 사용 시 오류가 발생.  

### 3-2. 옵셔널 바인딩

```swift 
var num1 : Int = 100
var num2? : Int = 30

if let nonOptionalnum = num2{
    let sum = nonOptionalnum + num1
}
```
Optional 값을 새로운 상수로 받는다.      
새로운 상수로 받는 것이기 때문에 ```!```키워드를 안 써도된다.
