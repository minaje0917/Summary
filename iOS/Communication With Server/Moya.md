# Moya

## 1. Why?
    iOS로 서버 통신을 하면 URLSession을 사용하는 것이 가장 기본적이다.

    URLSession
    - 장점 
    로우레벨의 코드를 작성할 수 있다, 라이브러리가 필요 없다.
    - 단점 
    사용이 복잡하고, 코드의 가독성이 좋지 않다.


    그러면 가장 보편적으로 많이 사용되는 Alamofire은?
    
    Alamofire
    - 장점
    인터페이스로 네트워킹을 단순화 해준다.
    - 단점
    유지보수와 유닛테스트가 힘들다.

    그래서 Moya가 뭐가 좋은데?

    Moya
    - 장점 
    Network Layer를 템플릿화 해서 재사용성을 높히고,
    개발자가 request와 response에만 신경쓰도록 해준다.

## 2. How?