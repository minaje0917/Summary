# StackView

## StackView가 뭔데?
StackView는 UI요소를 가로 혹은 세로 묶어주는 역할을 한다.        

## 사용법!
<b>Vertical Stack View - 세로</b> 방향으로 묶어주는 것       
<b>Horizontal Stack View - 가로</b> 방향으로 묶어주는 것

## 장점!
### 쉽다!
복잡한 화면에서 Auto Layout를 작업하려면     
엄청나게 많은 조건들이 필요하고 계산이 복잡해진다.      
하지만 UI안에 Stack View를 안에 추가하고,        
<b>Distribution, Alignment, Spacing</b> 설정만 딱 잡아주면      
개발자가 직접 추가해줘야하는 조건이 많이 줄어든다.      
왜냐하면 Stack View가 자동으로 잡아주기 때문이다!

           
### 빠르다!
Stack View 자체는 보여줄 컨텐츠가 없다.       
따라서 iOS가 화면을 띄울 때 별도의 렌더링이 필요하지 않아,         
일반 뷰보다 빠르개 그려진다.        

### 뷰를 추가하거나 삭제할 때 편하다!
StackView는 View를 추가하기만 하면, 알아서 설정에 맞게 레이아웃을 잡아준다.           

## StackView의 설정
StackView의 핵심 기능은 4개가 있다.     
가로 세로를 결정하는 <b>Axis</b>,          
뷰의 사이 여백을 결정하는 <b>Spacing</b>,        
뷰의 크기 배분을 하는 <b>Distribution</b>,      
뷰의 위치를 정렬하는 <b>Alignment</b>      

### Axis
View의 방향을 결정한다.     
```swift
StackView.axis = .horizontal // 가로    
StackView.axis = .vertical // 세로
```

### Spacing
View 사이의 여백을 결정한다.
```swift
StackView.spacing = 10 // 모든 view 사이의 여백이 10이 된다.
```

### Distribution
설명은 세로 기준입니다. 가로는 방향만 바꾸면 똑같아요.
1. Fill : StackView의 세로 공간을 모두 채울 수 있도록 하위뷰를 늘린다.
2. Fill Equally : StackView 내의 공간을 n분의 1 하여 똑같이 나눈다.
3. Fill Proportionally : StackView 내의 공간을 원래 사이즈를 유지하되 나눠서 배분한다.
4. Equal Spacing :  Stack view 공간을 뷰의 크기만큼만 채운다. 뷰의 크기는 그대로 두고, 남는 공간을 동일한 크기의 여백으로 채운다.
5. Equal Centering : Stack view 공간을 뷰의 크기만큼만 채운다. 각 뷰의 중심축 사이 간격이 똑같도록 빈 여백으로 채운다.

### Alignment
Horizontal 일때:
1. Fill: 스택 뷰의 높이만큼 내부 요소들의 높이를 결정해 스택뷰의 높이만큼 채운다.
2. Top: 스택 뷰의 내부 요소들의 높이를 유지하면서 스택 뷰 상단에 붙여준다.
3. Center: 스택 뷰의 내부 요소들의 높이를 유지하면서 스택 뷰 중앙에 정렬한다.
4. Botton: 스택 뷰의 내부 요소들의 높이를 유지하면서 스택 뷰 하단에 정렬한다.

Vertical 일 때:
1. Fill: 스택 뷰의 가로 길이만큼 내부 요소들의 너비를 결정해 스택 뷰의 가로만큼 채운다.
2. Leading: 스택 뷰의 내부 요소들의 너비를 유지하고 기준 지역에 따라 글자가 시작하는 방향으로 당겨서 채웁니다. 영어는 문장을 왼쪽에서 오른쪽 방향으로 읽기 때문에 왼쪽으로 당겨서 정렬된다.
3. Center: 스택 뷰의 내부 요소들의 너비를 유지하고 중앙에 정렬한다.
4. trailing: 스택 뷰의 내부 요소들의 너비를 유지하고 기준 지역에 따라 글자가 끝나는 방향으로 당겨서 채웁니다. 영어는 문장을 왼쪽에서 오른쪽 방향으로 읽기 때문에 오른쪽으로 당겨서 정렬된다.