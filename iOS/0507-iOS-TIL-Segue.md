





# Segue

## 개념도

![0507-iOS-TIL-Segue01](/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0507-iOS-TIL-Segue01.png)



## Action segue

### 1) 정의

버튼, 테이블 셀 등의 이벤트 트리거가 출발점인 경우



### 2) 네이게이션 바를 사용하는 경우의 segue

viewcontroller(navigation bar포함) 인 경우

show type segue: 전환된 뷰에 navigation bar 포함됨

present modal: 전환된 뷰에 navigation bar 불포함됨

viewcontroller(default) 인 경우

show / present modal 둘 다 present modal style로 실행됨



### 3) 다른 뷰에 데이터 전달하기

#### (1) 트리거(버튼 등)와 전환될 뷰를 1:1 로 Segue 연결하고 Attirbutes Inspector에서 Identifier를 정의한다.

![0507-iOS-TIL-Segue02](/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0507-iOS-TIL-Segue02.png)



#### (2) 데이터를 전달할 뷰를 설정한다

```swift
class SecondViewController: UIViewController {
    
    @IBOutlet weak var IBsegButtonTitle: UILabel!
    @IBOutlet weak var IBsegID: UILabel!
    @IBOutlet weak var sbtLabel: UILabel!
    @IBOutlet weak var siLabel: UILabel!
    
    var segButtonTitle: String = ""
    var segID: String = ""
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        // 저장프로퍼티로 전달 받은 데이터를 IB 프로퍼티로 할당한다.
        IBsegButtonTitle.text = segButtonTitle
        IBsegID.text = segID
    }

}
```



#### (3) 해당 ViewController에 커스텀 prepare 메소드를 작성한다.

prepare 메소드: 다음 뷰로 넘어가기 위한 사전작업. 따라서 호출되는 시점이 화면이 전환되기 전이다. 따라서 전달할 데이터를 담는 작업을 여기서 하게된다.

```swift
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
  super.prepare(for: segue, sender: segue)
  
  guard let secondVC = segue.destination as? SecondViewController else { return }
  // destination의 타입이 UIViewController인 이유:
  // 띄울 뷰컨트롤러를 개발자가 어떤 인스턴스로 생성할지 모르기 때문에 그냥 상위 클래스로 선언해놓은것이다.
  // 따라서 해당 뷰컨트롤러(SecondViewController)로 다운 캐스팅 해줘야 접근이 가능하다.
  
  // 주의사항. 띄울 뷰컨트롤러의 IB 프로퍼티를 바로 참조할 수 없다.
  secondVC.IBsegID = segue.identifier ?? "" // error
  secondVC.IBsegButtonTitle = (sender as? UIButton)?.currentTitle ?? "" // error
  
  // 따라서. 띄울 뷰컨트롤러의 저장프로퍼티를 참조해서 할당한다.
  secondVC.segID = segue.identifier ?? "" // 해당 Segue의 identifier을 전달한다.
  secondVC.segButtonTitle = (sender as? UIButton)?.currentTitle ?? ""
  // 이것도 마찬가지로 Any타입인 sender를 UIButton으로 다운캐스팅해서,
  // sender(해당 Segue와 1:1로 연결되어있는 Button)에 접근해서 currentTitle 프로퍼티를 참조해서 전달한다.
  

  //...
}
```



### 4) 디스미스 시키기

#### (1) ViewController에 unwind 메소드 작성

```swift
@IBAction func unwindToViewController(_ unwindSegue: UIStoryboardSegue) {
    
}
```

#### (2) SecondViewController의 버튼을 unwind 메소드 연결

![0507-iOS-TIL-Segue03](/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0507-iOS-TIL-Segue03.png)



### 5) 디스미스 하면서 데이터 전달하기(TextField 이용)

#### (1) 내려갈 뷰에서 전달할 데이터를 저장파라미터로 저장하기

```swift
class SecondViewController: UIViewController {
    
    @IBOutlet weak var IBsegButtonTitle: UILabel!
    @IBOutlet weak var IBsegID: UILabel!
    
    var segButtonTitle: String = ""
    var segID: String = ""
    var name: String = "" // 입력된 이름을 저장할 프로퍼티
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        IBsegButtonTitle.text = segButtonTitle
        IBsegID.text = segID
    }
    
    @IBAction func inputName(_ sender: UITextField) {
        let nameInput = sender.text ?? "" // TextField에서 입력 받은 텍스트를 옵셔널 해제
        name = nameInput // ViewController에 전달하기 위해 저장프르퍼티에 할당
    }
}
```

##### tip) 데이터 입력 팁

숫자가 아닌 값을 입력하면 무시하게 하고 싶을때

```swift
@IBAction func textField(_ sender: UITextField) {
    guard let inputVal = Int(sender.text ?? "0") else { return }
    self.textFieldVal = inputVal
}
```



#### (2) viewController에서 데이터 받아오기

unwindTo'뷰를 전환시킬 뷰'( ) 작성하기

```swift
@IBAction func unwindToViewController(_ unwindSegue: UIStoryboardSegue) {
  // unwindSegue.source의 타입이 UIViewController이고 SecondViewController로 다운캐스팅해서 인스턴스 생성
  guard let secondVC = unwindSegue.source as? SecondViewController else { return }
  // 이전 뷰의 저장프로퍼티를 참조해서 데이터 가져오기
  yourName.text = secondVC.name
}
```



### 6) 아래 두개의 차이점

```swift
let secondVC = SecondViewController()
guard let secondVC = segue.destination as? SecondViewController else {
    return
}
```

let secondVC = SecondViewController()는 그냥 스토리보드랑 상관없이 오직 코드 생성된 뷰 컨트롤러가 할당된다.

이런 이유보다도 뷰가 여러개 연결되어있을때는 정확하게 destination을 명시해서 사용해야한다.



### 7) shouldPerformSegue( ) 메소드

세그웨이를 실행할지 말지를 결정한다. true false를 리턴한다.

return id == "PlusOne" ? true : false

```swift
override func shouldPerformSegue(withIdentifier identifier: String, sender: Any?) -> Bool {
    // 합계가 40을 넘어가면 segueway 정지
    var plus: Int  = 0
    if identifier == "plusOne" {
        plus = 1
    }
    else {
        plus = 10
    }
    if count + plus >= 40 {
        return false // 40 넘어가면 false 반환하면서 정지
    }
    else {
        return true // 40 전까지는 true 반환하면서 뷰 전환
    }
}
```



## Manual Segue

1:1로 연결은 action seg

