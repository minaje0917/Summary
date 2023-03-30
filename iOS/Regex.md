## 1. Regex ⁉️      
코드를 짜다보면 항상 어려웠던 부분 중 하나가 정규식을 짜는 것이다.    
기존에 정규식을 만드는 여러가지 방법도 있지만    
(Ex - range, NSPredicate, NSRegularExpression 등..)   
너무 어려운 관계로 다음 글에...    
오늘은! iOS 16이후에 나온 Regex! 이 친구에 대해 알아 볼 거다!

## 2. Regex를 만드는 여러가지 방법‼️     
### 1. String으로 만드는 방법      
let regex = try Regex(#"정규식"#)       
//이 패턴을 사용하면 String에 따로 escaping 문자를 작성 안 해도 된다.
### 2. / /로 나타내기         
/ / 안에 정규식을 작성하면 컴파일러가 알아서 Regex타입으로 변환해준다.

let regex = /정규식/      
// 이 패턴을 WWDC에서는 Regex Literal라고 부른다.      
### 3. RegexBuilder 사용하기          
RegexBuilder를 사용하기 위해서는 우선 Import를 해주어야한다.     
그러면 DSL(Domain Specific Language) 방식으로 정규식을 작성할 수 있다.      
```swift
import RegexBuilder   

let regex = Regex {
	"팔별하고 싶은 것"
    Repeat(.digit, count: 2)
    "!"
}
```
심지어 body에 Regex Literal을 넣어도 된다!
```swift
import RegexBuilder

let regex = Regex {
	"팔별하고 싶은 것"
    "Regex Literal 판별식"
}
```
## 3. Matching되는 strings 찾기       
RegexBuilder는 Regex에서 제공하는 메서드를 사용해도 되고,      
Foundation에서 제공하는 메서드를 사용해도된다.       
예를 들어 첫번째로 매칭되는 것을 찾고싶다! 라고 하면       
Regex메소드인 firstMatch(in:)을 사용하거나     
Foundation메소드인 firstMatch(of:)를 사용해도 된다.     
