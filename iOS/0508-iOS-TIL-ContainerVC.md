# Container View Controller

## Navigation Controller

네비게이션 컨트롤러: 화면이 present되는게 아니라 나와서 덮는것



프레젠트는

루트뷰를 기점으로 계속 띄우는 것



네비게이션 컨트롤러

루트뷰로 네비게이션컨트롤러를 할당하고 네비게이션컨트롤러

탑뷰는 네이게이션 스택(차일드뷰의 적층)의 가장위

비저블뷰는 가장 최종적으로 보이는 뷰(프레젠트된 뷰)

타이틀

프롬프트: 타이틀위에 작게 나오는

프리퍼 라지 타이틀

* 전환방법
  * Embed In-> 2nd View 추가 및 Storyboard ID 설정-> Root View에 navigation item 추가 -> navigation item에 bar button 추가 ->  bar button을 Root View에 IBAction 연결 -> instantiateViewController로 2nd View 인스턴스 생성-> pushViewController로 전환



## 코드로 네비게이션 컨트롤러 만들기

### 1. SceneDelegate.swift

```swift
class SceneDelegate: UIResponder, UIWindowSceneDelegate {

    var window: UIWindow?


    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        guard let windowScene = (scene as? UIWindowScene) else { return }

        // 위의 var window: UIWindow? 프로퍼티를 사용할때
//        let vc = ViewController()
//        self.window = UIWindow(windowScene: windowScene)
//        self.window?.rootViewController = vc
//        self.window?.makeKeyAndVisible()
//        self.window?.backgroundColor = .systemBackground
        
        // window 변수를 새로 만들어서 할때
//        let window = UIWindow(windowScene: windowScene)
//        let vc = ViewController()
//        window.rootViewController = vc
//        window.makeKeyAndVisible()
//        window.backgroundColor = .systemBackground
//        self.window = window
        
        // 네이게이션 컨트롤러 생성
        let vc = ViewController()
        let navigationController = UINavigationController(rootViewController: vc)
        
        // iOS 11 이상. 이거는 보통 신델리게이트에 선언한다. 뷰컨에서 해도됨
        navigationController.navigationBar.prefersLargeTitles = true
        
        window?.rootViewController = navigationController
        window?.backgroundColor = .systemBackground
        window?.makeKeyAndVisible()
    }
  // code...
}
  
```

<br>

### 2. ViewController.swift

```swift
class ViewController: UIViewController {

    // 프로퍼티로 버튼 만들때는 lazy var로 선언해야함.
    lazy var barButtonItem = UIBarButtonItem(title: "Next", style: .plain, target: self, action: #selector(pushViewController(_:)))
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 타이틀
        navigationItem.title = "FirstVC"
        
        // 함수안에서 만들때는 lazy var로 안만들어도됨
        let barButtonItem2 = UIBarButtonItem(title: "Next", style: .plain, target: self, action: #selector(pushViewController(_:)))
        // 기호모양 버튼 만들기. barButtonSystemItem 인자 사용
        let barButtonItem3 = UIBarButtonItem(barButtonSystemItem: .play, target: self, action: #selector(pushViewController(_:)))
        
        // large title 못쓰게
        navigationItem.largeTitleDisplayMode = .never
        
        // navigation bar에 버튼 추가
        navigationItem.rightBarButtonItems = [barButtonItem, barButtonItem3, barButtonItem2]
//        navigationItem.rightBarButtonItems = [barButtonItem]
        
    }
    
    @objc private func pushViewController(_ sender: Any) {
        // 두번째 뷰 인스턴스 생성
        let secondVC = SecondViewController()
        
        // navigation controller에서 다른 뷰 push하는 방법
        navigationController?.pushViewController(secondVC, animated: true)
        // navigation controller에서 다른 뷰 show하는 방법
//        show(secondVC, sender: sender)
    }
}
```

<br>

### 3. SecondViewController

```swift
class SecondViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        view.backgroundColor = .systemYellow
    }
    override func viewDidAppear(_ animated: Bool) {
        super.viewDidAppear(animated)
        
        // Back 버튼 없이 자동으로 복귀시키는 법
//        navigationController?.popViewController(animated: true)
    }
}
```

***

## Tabbar Controller

탭바

탭바 아이템마다 각각의 뷰컨이 연결됨

최대 5개까지만 표시됨. 그 이상은 ... 으로 표시됨.

아이템을 5개 초과해서 쓸 일은 거의 없음

## 코드로 탭바 컨트롤러 만들기

### 1. SceneDelegate.swift

```swift
class SceneDelegate: UIResponder, UIWindowSceneDelegate {

    var window: UIWindow?

    func scene(_ scene: UIScene, willConnectTo session: UISceneSession, options connectionOptions: UIScene.ConnectionOptions) {
        
        // 전달 인자 scene를 UIWindowScene 클래스로 다운 캐스팅 해서 인스턴스(windowScene) 생성
        guard let windowScene = (scene as? UIWindowScene) else { return }
        
        // 세 개의 뷰 컨트롤러 인스턴스 생성
        let vc = ViewController()
        let SecondVC = SecondViewController()
        let ThirdVC = ThirdViewController()
        
        // 각 뷰 컨트롤러에 탭 바 아이템 추가.
        vc.tabBarItem = UITabBarItem(tabBarSystemItem: .bookmarks, tag: 0)
        SecondVC.tabBarItem = UITabBarItem(title: "Second", image: UIImage(systemName: "person.circle"), tag: 1)// person.circle은 sf symbol꺼임
        ThirdVC.tabBarItem = UITabBarItem(title: "Third", image: UIImage(systemName: "person.circle"), tag: 2)// person.circle은 sf symbol꺼임

        // ThirdVC를 root view controller로 하는 navigation controller 인스턴스 생성
        let navigationBarController = UINavigationController(rootViewController: ThirdVC)
        
        // ThirdVC를 root view controller로 하는 navigation controller에 탭 바 아이템 추가.
        navigationBarController.tabBarItem = UITabBarItem(title: "Third", image: UIImage(systemName: "trash"), tag: 2)
        
        // 탭 바 컨트롤러 인스턴스(tabBarController) 생성
        let tabBarController = UITabBarController()
        // 탭 바 컨트롤러에 뷰 컨트롤러 추가
        tabBarController.viewControllers = [vc, SecondVC, navigationBarController]
        
        window = UIWindow(windowScene: windowScene)
        window?.rootViewController = tabBarController
        window?.makeKeyAndVisible()
        
    }
  // code...
}
```

<br>

### 2. ViewController.swift

```swift
class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        view.backgroundColor = .systemGreen
    }
}
```

<br>

### 3. SecondViewController.swift

```swift
class SecondViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        view.backgroundColor = .systemYellow
    }
}
```

<br>

### 4. ThirdViewController.swift

```swift
class ThirdViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        view.backgroundColor = .systemPink
        // 바 버튼 아이템 생성
        let barButtonItem = UIBarButtonItem(barButtonSystemItem: .play, target: self, action: #selector(pushViewController(_:)))
        // ThirdVC를 root view controller로 하는 navigation controller에 오른쪽 바 버튼 아이템 추가.
        self.navigationItem.rightBarButtonItems = [barButtonItem]
        
    }
    
    @objc private func pushViewController(_ sender: Any) {
        // 전환할 뷰컨트롤러 인스턴스(vc) 생성
        let vc = UIViewController()
        // navigation controller에서 다른 뷰 push하는 방법
        self.navigationController?.pushViewController(vc, animated: true)
        // navigation controller에서 다른 뷰 show하는 방법
//        show(vc, sender: sender)
    }
}
```

