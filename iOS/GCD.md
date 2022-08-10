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
    3. GCD의 종류와 특성   
        GCD는 3가지 나뉜다.     
        (글로벌)메인, 글로벌, 프라이빗(커스텀)디스패치 큐로 나뉜다.    
        대기열마다 특성이 조금씩 다르다. 작업의 특성, 원하는 일처리에 따라 대기열(큐)의 특성에 맞게 작업을 보내면된다.     
        1. (글로벌)메인 큐     
            * 유일하게 한 개이다.
            * 직렬(serial)로 동작한다.
            * 메인 쓰레드를 의미한다.
                ```swift 
                DispatchQueue.main.async {
                   //작업을 메인쓰레드(1번 쓰레드)로 보낸다.(비동기적으로)  
                }

                //참고용 코드
                DispatchQueue.main.asyncAfter(.now() + 2) {
                    //지금으로부터 2초뒤에 메인쓰레드로 작업을 보낼거야.
                }
                ```
        2. 글로벌 큐 
            * 종류가 여섯개가 있다. (QOS)
            * 기본 설정이 Concurrent(동시)이다.

            1. 예시코드
                ```swift
                DispatchQueue.global().async{

                }
                ```
            2. QOS
                ``` swift
                //중요도 순서(위로 올라올수록 중요!)
                 
                DispatchQueue.global(qos: .userInteractive) 거의 즉시
                유저와 직접적 인터렉티브: UI업데이트, 애니메이션, UI반응관련 어떤 것이든(사용자와 직접 상호작용하는 작업에 권장)     
                (작업이 빨리 처리되지 않으면 상황이 멈춘 것처럼 보일만한 작업)

                DispatchQueue.global(qos: .userInitiated) 몇초
                유저가 즉시 필요하긴 하지만, 비동기적으로 처리된 작업 (ex. 앱 내에서 pdf파일 여는 것)

                DispatchQueue.global() //디폴트
                일반적인 작업

                DispatchQueue.global(qos: .utility) 몇초에서 몇분
                보통 Progress Indicator와 함께 길게 실행되는 작업, 계산, IO, Networking, 지속적인 데이터 feeds

                DispatchQueue.global(qos: .background) 속도보다 에너지효율성 중시, 몇분 이상
                유저가 직접적으로 인지하지 않고(시간이 안 중요한)작업, 데이터 미리 가져오기, 데이터베이스 유지

                DispatchQueue.global(qos: .unspecified)
                legacy API (스레드를 서비스 품질에서 제외시키는 )
        3. 프라이빗 큐     
            * 커스텀으로 만든다.
            * 디폴트 설정은 Serial(직렬)이지만 Concurrent(동시)로도 설정 가능하다.
            * QOS 설정이 가능하다.    
                예시 코드
                ``` swift 
                let queue = DispatchQueue(label: "레이블 명")

                queue.async {

                }

                let queue2 = DispatchQueue(label: "레이블 명", attributes: .concurrent)
                //동시로도 설정 가능.
                ```
            * QOS설정을 안 해도 OS가 알아서 QOS를 판단해 설정한다.
    4. 반드시 메인큐에서 해야하는 작업    
    * UI관련(화면) 일들은 __메인큐에서__ 처리해야한다.     
    코드 예시
        ```swift
        DispatchQueue.global().async{
            // 이미지 다운로드 등 관련 코드
            코드 1
            코드 2
            코드 3
            코드 4 

            DispatchQueue.main.async{
                // 다운로드한 이미지 표시 코드
                self.imageView.image = image
            }

        }
        ```
    5. sync메서드에 대한 주의사항
        1. 메인큐에서 다른큐로 보낼 때 sync메서드를 부르면 절대 안된다.
            > 즉, UI와 관련되지 않은 오래걸리는 작업(네트워크)들은    
            다른 쓰레드에서 일을 할 수 있도록 __async(비동기적)으로__ 실행하여야하며,    
            동기적으로 시키면 UI가 멈춰서 유저에게 반응을 늦게 하고 버벅거린다.

            ```swift 
            DispatchQueue.global().sync {
                // 현재 메인에서 일하고 있다면 절대절대 sync메서드를 쓰면 안된다.
                // sync (X) async(O)
            }
            ```
        2. 현재의 큐에서 현재의 큐로 "동기적으로" 보내서는 안된다.
            > 현재의 큐를 블락하는 동시에 다시 현재의 큐에 접근하기 때문에     
            <b>교착상태(DeadLock)</b>이 발생한다.
            ```swift 
            DispatchQueue.global().async {

                DispatchQueue.global().sync{

                }

            }
            ```

        

참고 : https://inf.run/pQ7u