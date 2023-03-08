# CGRect, CGSize, CGPoint

## CGPoint

```swift 
public struct CGPoint {

    public init()

    public init(x: Double, y: Double)

    
    public var x: Double

    public var y: Double
}
```
CGPoint는 <b> x, y</b>라는 변수를 가지고 있다.       
따라서 <b>View의 위치를 나타낼 때는 CGPoint</b>를 이용해야한다. 

## CGSize

```swift
public struct CGSize {

    public init()

    public init(width: Double, height: Double)

    public var width: Double

    public var height: Double
}
```
CGSize는 <b>width랑 height</b>를 가지고 있다.       
따라서 <b>View의 크기를 정할 때는 CGSize</b>를 사용한다.  

## CGRect  

```swift 
public struct CGRect {

    public init()

    public init(origin: CGPoint, size: CGSize)

    public var origin: CGPoint

    public var size: CGSize
}
```

CGRect는 CGPoint 타입의 origin, CGSize 타입의 size가 둘 다 들어 있다.      
따라서 View를 나타낼 때 origin으로 x, y를 size라는 것으로 width, height를 나타내면 됨.            
