# Delegate

## 정의

특정 로직(델리게이트 메소드)를 내가 아닌 다른 객체(뷰 컨트롤러)가 구현하도록 위임하는 형태의 디자인 패턴

텍스트필드가 받은 이벤트(예: 터치)에 의해 호출되는 메소드(델리게이트 메소드)의 내용을 뷰 컨트롤러에 작성하는 것......?ㅠㅠ...

<br>

## 애플에서 제공하는 델리게이트 패턴

* AppDelegate, SceneDelegate, UITextFiledDelegate 등등

### 예시: AppDelegate

내용: 앱의 각 실행단계에서 특정 동작을 실행한다.

```swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
    
    // 초기화 작업이 끝나고 뷰컨이 보이기 전에 할거 있으면 여기다 작성해라
  // 이 메소드가 실행될 시점(코드의 위치)는 애플만이 알고 있다.
    return true
}

func applicationDidBecomeActive(_ application: UIApplication) {
    // 앱이 Active상태가 될 때 호출(기본)
    // +
    // 개발자한테 니가 더 하고 싶은게 잇ㅇ으면 여기서 해라(커스텀)
}// 스위프트 데이터 구조와 알고리즘
```

<br>

## 커스텀 델리게이트 패턴

### 예시: CustomViewDelegate

내용: 커스텀 뷰의 background 색상이 새로 할당되면 colorForBackground 델리게이트 메소드가 실행된다.

1) 커스텀 델리게이트를 정의한다.

```swift
protocol CustomViewDelegate: class {
  func colorForBackground(_ newColor: UIColor?) -> UIColor // CustomViewDelegate를 채택한(위임받은) 뷰컨트롤러에서 이 메소드를 구현(기능 작성)해줘야한다.
}
```

<br>

2) colorForBackground 델리게이트 메소드가 실행될 시점을 정의한다.

여기서는 CustomView 객체의 background 프로퍼티에 새로운 값이 할당될 때.

```swift
class CustomView: UIView {
    
    weak var delegate: CustomViewDelegate? // 뷰컨트롤러에서   = self로 작성하므로 순환 참조가 발생할 수 있어서 weak로 선언.
    
    override var backgroundColor: UIColor? {
        get {super.backgroundColor}
        set {
          let color = delegate?.colorForBackground(newValue) // 단순히 호출만 한 것. 실제 호출은 뷰컨트롤러에서 한다. 뷰컨트롤러의 주소 값이 delegate에 담길거다. 따라서 선언은 옵셔널로. 뷰컨트롤러에서 작성된 메소드내용을 가져다가 여기서 실행한다.커스텀뷰는 이 메소드가 어떤 기능을할지 모른다. 그건 뷰컨트롤러에서 작성된다. 이 메소드가 언제 호출되고 어떻게 쓰일지는 여기서 결정한다.
          let newColor = color ?? newValue ?? .gray
          super.backgroundColor = newColor
          print("새로 변경될 색은 :", newColor)
        }
    }
}
```

<br>

3) 커스텀 뷰 델리게이트를 채택한 뷰 컨트롤러에서 colorForBackground 델리게이트 메소드의 기능을 작성한다.

위임받은 메소드(colorForBackground)는 새로운 색상이 초록이면 파란색을, 나머지는 새로운 색상 그대로를 반환한다.

따라서 최종적으로 커스텀 뷰의 색상이 위와 같은 규칙으로 변경이된다.

```swift
class ViewController: UIViewController, CustomViewDelegate {
  @IBOutlet weak var customView: CustomView!
  override func viewDidLoad() {
    super.viewDidLoad()
  }
  customView.delegate = self // self = viewController가 올라간 메모리 주소가 할당됨.
  func colorForBackground(_ newColor: UIColor?) -> UIColor { // 커스텀뷰델리게이트의 메소드의 기능을 작성해야한다(실제구현은 뷰컨트롤러에서 하는거다). 하지만 이 메소드가 언제 호출되고 어떻게 쓰이는지는 모른다. 여기서 UIColor를 반환하면 델리게이트에서 받아다 쓴다.
    guard let color = newColor else { return .gray }
    return color == .green ? .blue : color
  }
}
```

