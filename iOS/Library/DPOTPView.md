# DPOTPView

## What?
![](https://github.com/Datt1994/DPOTPView/raw/master/Add%20Class.png) 
DPOTP는 이러한 OTP인증 서비스 UI를 구현할 때 유용하게 쓸 수 있는 라이브러리이다.     

## Why?
일반적인 코드로 위 사진과 같이 UI를 구성하려면 TextField를 여러 개 써야한다.    
하지만 이는 매우 좋지 않다.

``` 
    좋지 않은 이유    
1. 중복된 코드가 많아져 코드가 더러워진다.
2. 서버로 text를 POST할 때 TextField 값을 전부 합쳐서 보내야 하기 때문에 매우 귀찮다.
```
```
    DPOTP를 써야하는 이유
1. TextField를 하나만 써도 된다.
2. 서버로 값을 POST할 때 하나의 TextField의 값만 보내면 된다.
```

## How?

설치 방법     
``` 
$ gem install cocoapods
```
  
코드 예시
``` swift
let txtOTPView = DPOTPView(frame: CGRect(x: (self.view.frame.width - 250)/2, y: txtDPOTPView.frame.origin.y + 50, width: 250, height: 60))
txtOTPView.count = 5
txtOTPView.spacing = 10
txtOTPView.fontTextField = UIFont(name: "폰트-이름", size: CGFloat(25.0))!
txtOTPView.dismissOnLastEntry = true
txtOTPView.borderColorTextField = .black
txtOTPView.selectedBorderColorTextField = .blue
txtOTPView.borderWidthTextField = 2
txtOTPView.backGroundColorTextField = .lightGray
txtOTPView.cornerRadiusTextField = 8
txtOTPView.isCursorHidden = true
//txtOTPView.isSecureTextEntry = true
//txtOTPView.isBottomLineTextField = true
//txtOTPView.isCircleTextField = true
view.addSubview(txtOTPView)
```