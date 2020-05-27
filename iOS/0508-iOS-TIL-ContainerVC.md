# Container View Controller

##  Navigation Controller

### 특징

* 수직적 관계를 가지는 화면 흐름을 관리
* 탭바 컨트롤러와 뷰 컨트롤러는 일종의 Segue(관계형 Segue)로 연결됨
* 화면이 present되는게 아니라 나와서 덮는것
  * presetn는 rootview를 기점으로 계속 생성 하는 것
* 뷰 컨트롤러들의 포인터 내비게이션 스택을 이용하여 관리

### 전환방식

1. push - pop
   1. 스택의 최상위에 뷰 컨트롤러를 추가할 때 pushViewController 메소드 사용
   2. 스택의 최상위의 뷰 컨트롤러를 제거할 때 popViewController 메소드 사용
2. present - dismiss
   1. 뷰컨트롤러 직접 호출 방식

<br>

### 스토리보드로 네비게이션 컨트롤러 만들기

1. Embed In으로 네비게이션 컨트롤러 및 root view 추가

2. 두번째 뷰 추가 및 Inspector 항목 중 Storyboard ID 에 ID 입력

3. root view에 navigation item 추가

4. navigation item에 bar button 추가

5. bar button을 root view에 IBAction 연결

6. IBAction로 두번째 뷰 인스턴스 생성 및 pushViewController(<span style="color: red;">반드시 withIdentifier를 인자로 갖는 놈으로!</span>)

   ```swift
   @IBAction func pushNextBySBID(_ sender: UIBarButtonItem) {
       guard let secondVC = self.storyboard?.instantiateViewController(withIdentifier: "secondVC") else { return } // Storyboard에서 생성한 두번째 뷰를 해당 ID을 이용해 인스턴스 생성
       self.navigationController?.pushViewController(secondVC, animated: true) // push
   }
   ```

7. (복귀)두번째 뷰에 버튼 추가 후 IBAction으로 root view 인스턴스 참조 및 pop

   ```swift
   @IBAction func popBack(_ sender: UIButton) {
       if let nc = self.navigationController { // root view 인스턴스 생성 및 옵셔널 바인딩
           nc.popViewController(animated: true) // pop
       }
       else {
           print("navigationController is nil")
       }
   }
   ```

<br>

### 코드로 네비게이션 컨트롤러 만들기

#### 1. SceneDelegate.swift

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

#### 2. ViewController.swift

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

#### 3. SecondViewController

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

<br>

## Tabbar Controller

### 특징

* 수평적 관계를 가지는 화면 흐름을 관리함
* 탭바 아이템마다 각각의 뷰 컨트롤러가 연결됨
* 탭바 컨트롤러와 뷰 컨트롤러는 일종의 Segue(관계형 Segue)로 연결됨

최대 5개까지만 표시됨. 그 이상은 ...(more) 으로 표시됨.

아이템을 5개 초과해서 쓸 일은 거의 없음

<br>

### 스토리보드로 탭바 컨트롤러 만들기

<ol>
  <li>Embed In 으로 탭바 컨트롤러 및 첫번째 뷰 컨트롤러 생성</li>
  <li>생성된 뷰 컨트롤러의 Inspector의 Bar item Title 설정</li>
  <li>두번째 뷰 컨트롤러 생성하여 탭바 컨트롤러로 부터 ctrl+드래그로 'Relation Segue'연결</li>
  <li>두번째 뷰 컨트롤러의 Inspector의 Bar item Title 설정</li>
</ol>

추가(탭바 컨트롤러의 뷰 컨트롤러를 네비게이션 컨트롤러와 연결하는 법)

5. 해당 뷰 컨트롤러를 네비게이션 컨트롤러로 Embed In
6. 이후 내용은 '스토리보드로 네비게이션 컨트롤러 만들기'와 같음

주의사항

* 각 탭바 아이템에 대한 설정은 탭바 컨트롤러가 아닌 해당 뷰 컨트롤러의 탭바 아이템 속성을 수정해야한다.

  (단, 탭바 아이템의 탭바 내에서의 순서는 탭바 컨트롤러에서 드래그로 수정 가능하다.)

<br>

### 코드로 탭바 컨트롤러 만들기

#### 1. SceneDelegate.swift

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

#### 2. ViewController.swift

```swift
class ViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()

        view.backgroundColor = .systemGreen
    }
}
```

<br>

#### 3. SecondViewController.swift

```swift
class SecondViewController: UIViewController {

    override func viewDidLoad() {
        super.viewDidLoad()
        
        view.backgroundColor = .systemYellow
    }
}
```

<br>

#### 4. ThirdViewController.swift

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

