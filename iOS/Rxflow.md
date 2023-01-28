# <b>Rxflow</b>

## 1. Rxflow란?
- Reactive Flow Coordinator 패턴을 기반으로 한 iOS navigation 프레임워크      

## 2. Rxflow 사용 목적
- UIViewController에서 모든 navigation 매커니즘 제거
- 종속성 주입에 용이

## 3. Rxflow 용어
<b>Flow</b> : 어플리케이션 내부의 navigation 공간을 의미       
<b>Step</b> : 어플리케이션 내부의 navigation 상태를 의미
<b>Stepper</b>  : Step을 생성할 수 있는 모든 것. Stepper는 Flow 내부의 모든 navigation 행동을 트리거할 수 있다.
<b>Presentable</b> : 표현할 수 있는 무언가를 추상화한 것. 예) UIViewController, Flow    
<b>NextFlowItem</b> : 반응형 메커니즘에서 새로운 Step을 발생시킬 다음 무언가에 대해 Coordinator에 전달해 주는 역할

## 4. Rxflow 사용법!
### Step 정의
Step을 정의할 때는 Enum을 사용한다.

```swift 
import Rxflow

enum ExStep: Step {
    //Login
    case loginIsRequired
    case userIsLoggedIn

    //Settings
    case settingsAreRequired
    case settingsAreComplete

    //home
    case homeIsRequired
}
```
Step을 정의할 때 가장 주의해야하는 부분은, step case를 가능한 독립적으로 유지하는 것이다.(무슨 말이지..?)

### Flow와 함께!
navigation의 기반인 Root Presentable을 선언하는 것이다.         
navigate(to: )함수를 구현하여 Step을 navigation action으로 바꾼다.          
```swift 
import Rxflow
import UIKit

class AppFlow: Flow {
    private let rootViewController = UINavigationController()
    private let service: ExService

    var root: Presentable {
        return self.rootViewController
    }

    init(withService service: ExService) {
        self.service = service
    }

    func navigate(to step: Step) -> FlowContributors {
        guard let step = step as? ExStep else { return .None}

        switch step {
            case .loginIsRequired:
                return self.navigationToLogin()
            case .homeIsRequired:
                return self.navigationToHome()
        }
    }
}
```

### Stepper 정의!
Stepper는 VC가 될수도 VM이 될수도 있다!        
이번에는 VC에 Stepper를 만들어 navigaion aciton을 트리거 해보자!     
#### [LoginViewController]
```swift 
import UIKit
import RxFlow
import RxCocoa

class LoginViewController: UIViewController, Stepper {
    var steps = PublishRelay<Step>()

    var btn = UIButton().then {
        // 코드들..

        //예제니 addTarget으로 ... ㅎㅎ
        $0.addTarget(self, action: #selector(btnDidTap), for: .touchUpInside)
    }

    @objc btnDidTap() {
        //steps를 트리거.
        self.steps.accept(ExStep.homeIsRequired)
    }
}
```

#### [HomeViewController]
```swift 
import UIKit
import RxFlow
import RxCocoa

class HomeViewController: UIViewController, Stepper {
    var steps = PublishRelay<Step>()

    var btn = UIButton().then {
        // 코드들..

        //예제니 addTarget으로 ... ㅎㅎ
        $0.addTarget(self, action: #selector(btnDidTap), for: .touchUpInside)
    }

    @objc btnDidTap() {
        //steps를 트리거.
        self.steps.accept(ExStep.homeIsRequired)
    }
}
```

### AppStepper 정의!
정의하는 이유? : 앱을 런치하면 어떤 화면이 랜딩될 것이고, 그 랜딩은 step에 정의 되어있을거다.      
어느 화면으로든 시작은 해야니 ``initialStep``이라는 친구가 필요하다.      
``initialStep``을 정의하기 위해 AppStepper를 작성한다.
```swift 

class AppStepper: Stepper {

    let steps = PublishRelay<Step>()
    private let disposeBag = DisposeBag()

    init() {}

    var initialStep: Step {
        return DemoStep.loginIsRequired
    }
}
```

### SceneDelegate 수정!
```swift 
class SceneDelegate: UIResponder, UIWindowSceneDelegate {

    var window: UIWindow?
    var coordinator = FlowCoordinator()
    var disposeBag: DisposeBag = DisposeBag()

    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        guard let windowScene = (scene as? UIWindowScene) else { return }
        let window = UIWindow(windowScene: windowScene)
        self.window = window
        
        let appFlow = AppFlow(window: window)
        self.coordinator.coordinate(flow: appFlow, with: AppStepper()) ✅
        window.makeKeyAndVisible()

    }
    ...
}
```

### LoginFlow, HomeFlow 정의

[LoginFlow]
```swift 
import Foundation
import RxFlow

class LoginFlow: Flow {

    var root: Presentable {
        return self.rootViewController
    }

    private lazy var rootViewController: UINavigationController = {
        let viewController = UINavigationController()
        return viewController
    }()

    init() {}

    func navigate(to step: Step) -> FlowContributors {
        guard let step = step as? ExStep else { return .none }
        switch step {
        case .loginIsRequired:
            return self.navigateToLogin()
        case .homeIsRequired:
            return .end(forwardToParentFlowWithStep: DemoStep.homeIsRequired)
        }
    }
    
    private func navigateToLogin() -> FlowContributors {
        let viewController = LoginViewController()
        self.rootViewController.setViewControllers([viewController], animated: false)
        return .one(flowContributor: .contribute(withNext: viewController))
    }
}
```

[HomeFlow]
```swift 
import Foundation
import RxFlow

class HomeFlow: Flow {

    var root: Presentable {
        return self.rootViewController
    }

    private lazy var rootViewController: UINavigationController = {
        let viewController = UINavigationController()
        return viewController
    }()

    init() {}

    func navigate(to step: Step) -> FlowContributors {
        guard let step = step as? ExStep else { return .none }
        switch step {
        case .homeIsRequired:
            return self.navigateToHome()
        case .homeIsRequired:
            return .end(forwardToParentFlowWithStep: DemoStep.homeIsRequired)
        }
    }
    
    private func navigateToLogin() -> FlowContributors {
        let viewController = HomeViewController()
        self.rootViewController.setViewControllers([viewController], animated: false)
        return .one(flowContributor: .contribute(withNext: viewController))
    }
}
```

### AppFlow의 navigateTo~ 로직 추가
```swift 
func navigate(to step: Step) -> FlowContributors {
    guard let step = step as? DemoStep else { return .none }
    switch step {
    case .loginIsRequired:
        return self.navigateToLogin()
    case .homeIsRequired:
        return self.navigateToHome()
    }
    
}
private func navigateToLogin() -> FlowContributors {
    let loginFlow = LoginFlow()
    Flows.use(loginFlow, when: .created) { (root) in
        self.window.rootViewController = root
    }
    return .one(flowContributor: .contribute(withNextPresentable: loginFlow, withNextStepper: OneStepper(withSingleStep: DemoStep.loginIsRequired)))
}

private func navigateToHome() -> FlowContributors {
    let homeFlow = HomeFlow()
    Flows.use(homeFlow, when: .created) { (root) in
        self.window.rootViewController = root
    }
    return .one(flowContributor: .contribute(withNextPresentable: homeFlow, withNextStepper: OneStepper(withSingleStep: DemoStep.homeIsRequired)))
}
```

### 코드 로직!
BtnDidTap (LoginVC)     
↓       
homeIsRequired (LoginVC)      
↓    
end(LoginFlow)      
↓    
homeIsRequired (Appflow)      
↓    
navigateToHome (Appflow)