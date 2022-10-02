# CAGradientLayer

## Declaration
```swift 
class CAGradientLayer: CALayer
```

## 1. CAGradientLayer 생성
```swift 
let gradient = CAGradientLayer() //객체 생성
gradient.frame = myView.bounds // 내가 적용할 뷰의 bounds로 지정
```
## 2. 색 설정

```swift 
let colors: [CGColor] = [
   .init(red: 0, green: 0, blue: 0, alpha: 1),
   .init(red: 0, green: 0, blue: 0, alpha: 1),
   .init(red: 0, green: 0, blue: 0, alpha: 1)
]
gradient.colors = colors
```
## 3. 시작지점, 끝 지점 설정

```swift 
gradientLayer.startPoint = CGPoint(x: 0.0, y: 0.0)
gradientLayer.endPoint = CGPoint(x: 1.0, y: 1.0)
```

![](https://blog.kakaocdn.net/dn/CGvDr/btqNLoftBSd/AUhTQuO9WxfRxIqd0N8kr0/img.png)





참고 및 출처 : https://babbab2.tistory.com/55