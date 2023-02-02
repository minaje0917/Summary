# RxSwift

## Why?

```
비동기 프로그래밍을 관찰 가능한 흐름으로 지원해주는 API
옵저버 패턴과 이터레이터 패턴 그리고 함수형 프로그래밍을 조합한 반응형 프로그램
```

결론적으로 RxSwift는 비동기 프로그래밍을 위해서 만들어진 API이다.

## Observable?

Observable이란 여러 이벤트들을 생성할 수 있는 대상.
### Jsut
Just는 하나의 값을 방출한다. 
```swift
let observable1 = Observable.just(1)

observable1.subscribe { event in
    print(event)
}

//next(1)
//completed 
```
observable를 구독하니 1이 나오고 완료 메세지가 나옴.
### Of

Of는 여러개의 값을 방출한다.
```swift 
let observable2 = Observable.of(1, 2, 3)
observable2.subscribe { event in
    print(event)
}

let observable3 = Observable.of([1, 2, 3])
observable3.subscribe { event in
    print(event)
}

//next(1)
//next(2)
//next(3)
//completed

//next([1, 2, 3])
//completed
```
observable2를 구독하니 1이 나오고 이어서 2, 3이 나옴.
observable3를 구독하니 배열 [1, 2, 3]이 나옴.

### From

From은 배열의 값을 방출한다.

```swift
let observable4 = Observable.from([1, 2, 3, 4, 5])

observable4.subscribe { event in
    print(event)
}

observable4.subscribe { event in
    if let element = event.element {
        print(element)
    }
}

//next(1)
//next(2)
//next(3)
//next(4)
//next(5)
//completed

//1
//2
//3
//4
//5 
```
observable4를 구독하니 배열이 연속해서 나온다.


```swift
let subscription4 = observable4.subscribe(onNext: { element in
    print(element)
})

subscription4.dispose()

//1
//2
//3
//4
//5
```
observable은 옵져버가 계속해서 구독하기 때문에 메모리 누수가 생길 수도 있다.      따라서 dispose를 해줘야한다.
### DisposeBag

DisposeBag은 dispose할 객체들을 담는 쓰레기통이라고 생각하면 된다.

```swift
let disposeBag = DisposeBag()

Observable.of("A", "B", "C")
    .subscribe{
        print($0)
    }.disposed(by: disposeBag)

//next(A)
//next(B)
//next(C)
//completed
```
여러가지 Observable객체들을 disposs할 수 있다.

## Operators