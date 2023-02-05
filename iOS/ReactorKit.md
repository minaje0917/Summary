# ReactorKit 

## ReactorKit이 뭔데?
- RxSwift의 강점인 <b>비동기 처리가 편함.</b>
- MVVM과 같은 아키텍처는 규격화된게 없어서 개발자, 회사마다 모두 달라 일관적이지 않은 반면에 <b>ReactorKit은 형식이 갖추어진 형태 </b>


## ReactorKit의 컴포넌트?
- UI에 해당하는 View, UI에 반응하여 <b>비지니스 로직을 처리하는 Reactor로 구성 </b>       
- Reactor에 action을 정의해 두고 해당 action을 처리하여 다시 View에 상태 값을 넘김
- View는 인터렉터 이벤트들을 Reactor의 action에 보내고 reactor에 state를 구독해 UI를 업데이트 시키는 것. 

## View
- UI가 있다.
- UI들의 action을 reactor로 넘김.
- reactor의 state를 구독하고 있다.

## Reactor 
- Action
    - view로부터 받은 action을 enum으로 정의
- Mutation
    - view로부터 action을 받은 경우, 해야할 작업단위를 enum으로 정의
- State
    - 현재 상태를 기록, view에 해당 정보를 사용하여 UI를 업데이트한다.
- mutate(action:) -> Observable<Mutation\>
    - action이 들어온 경우, 어떤 처리를 할 것인지 Mutation에서 정의한 작업 단위를 사용하여 Observable로 방출
    - 해당 부분에서, Rxswift의 concat 연산자를 이용하여 비동기 처리가 유용 
    - concat : 여러 Observable이 배열로 주어지면 순서대로 실행
- reduce(state:mutation:) -> State
    - 현재 상태(state)와 작업단위(mutation)을 받아서, 최종 상태를 반환
    - mutate(action:) -> Observable<Mutation\>이 실행된 후 바로 해당 메소드 실행

* 출처 : https://ios-development.tistory.com/782