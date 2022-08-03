# GCD

#### [이 강의](https://inf.run/pQ7u)를 들으며 배운 내용을 정리합니다.
     
        
       
1. GCD / Operation
- 직접 쓰레드를 관리하지 않고, <b>"큐(Queue)"(대기열/대기행렬)</b>라는 개념을 이용해 작업을 분산처리 : 쓰레드를 생성하지 않고, 큐(Queue)만 만들어서 그 안에 작업을 넣으면 iOS가 알아서 쓰레드를 생성하여 분산처리한다.     
- GCD / Operation이 알아서 쓰레드 숫자를 관리한다. (직접 쓰레드를 생성하는 것은 하드웨어나 일의 부하(load)와 같은 시스템에 대한 지식 없이 사용하면, 오히려 앱이 느려질 수 있다.)      
- 쉽게 다른 쓰레드에서 (오래 걸리는) 작업들이 "비동기적으로 동작"하도록 만들어준다.
    1. 코드 예시 
        ``` swift 
        DispatchQueue.global().async {
        //큐에 보낼거야  글로벌 큐에 비동기적으로
            //여기 들어오는 작업을 글로벌 큐에 비동기적으로 보낸다는 뜻.
        }


        let queue = DispatchQueue.global()
        //보낼 큐의 종류를 선언

        
        queue.asnyc {
            //(해당큐에) 비동기적으로 보낼거야
        }


        //작업의 단위 ===> "하나의 클로저" 안에 보내는 작업 자체가 묶이는 개념.
        DispatchQueue.global().async {
            print("Task 1 시작")
            print("Task 1의 중간 작업 1")
            print("Task 1의 중간 작업 2")
            print("Task 1의 중간 작업 3")
            print("Task 1의 중간 작업 4")
            print("Task 1 종료")
            // 하나의 작업이기때문에 순서적으로 작동한다.
        }

        ```
    2. GCD / Operation     
    GCD: Grand Central Dispatch = 디스패치 큐      
        - 간단한 일 
        - 함수를 사용하는 작업 (메서드 위주)   

        Operation: 오퍼레이션 큐    
        - 복잡한 일 (커뮤니케이션의 양)
        - 데이터와 기능을 캡슐화한 객체  

        Operation은 GCD를 기반으로해서 만들어진 것이다.     
        GCD + 여려가지 기능(취소 / 순서지정 / 일시정지) = Operation     
        프로젝트의 효율성, 사례 적합성 등을 고려해서 GCD or Operation을 선택해서 사용하자. 
        

        

        




참고 : https://inf.run/pQ7u