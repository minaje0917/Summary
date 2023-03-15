# RxSwift

이 정리는 곰 튀김님의 강의를 듣고 내용을 정리한 것입니다.     
출처 : https://www.youtube.com/watch?v=w5Qmie-GbiA&list=PL03rJBlpwTaAh5zfc8KWALc3ADgugJwjq&index=2&ab_channel=%EA%B3%B0%ED%8A%80%EA%B9%80         


## Rxswift?
Rxswift란 <b>비동기 코드를 조금 더 간결하게</b> 쓰기 위해서 만들어졌다.

## Observable

Obserable이 return된다.       
onNext로 값을 넘긴다.          
subscribe로 그 값을 받아서 처리한다.    
D isposeBag로 Observable이 동작하고 있는 동안에 모든 작업을 dispose(취소) 시킬 수 있다.            

```swift 
var disposeBag : DisposeBag = DisposeBag()

@objc func onLoadImage(_ sender: Any) {
        imageView.image = nil

        rxswiftLoadImage(from: LARGER_IMAGE_URL)
            .observeOn(MainScheduler.instance)
            .subscribe({ result in
                switch result {
                case let .next(image):
                    self.imageView.image = image

                case let .error(err):
                    print(err.localizedDescription)

                case .completed:
                    break
                }
            })
            .disposed(by: disposeBag)
}

@objc func onCancel(_ sender: Any) {
    disposeBag = DisposeBag()
}

func rxswiftLoadImage(from imageUrl: String) -> Observable<UIImage?> {
     return Observable.create { seal in
        asyncLoadImage(from: imageUrl) { image in            
            seal.onNext(image) // Next로 image를 전달한다. 
            seal.onCompleted()
        }
        return Disposables.create()
    }
}
```
