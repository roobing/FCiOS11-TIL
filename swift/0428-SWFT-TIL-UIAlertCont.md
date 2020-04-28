# UIAlertController

* 특정 이벤트 트리거(버튼, 텍스트입력 등등)에 호출하여 사용

  ```swift
     @IBAction private func button(_ sender: UIButton) { // 터치 이벤트 버튼
       
       // UIAlertController 인스턴스 생성   
       let alertController = UIAlertController()
       // UIAlertAction 인스턴스 생성
       let addCountAction = UIAlertAction() { /*클로저*/ }
       let resetAction = UIAlertAction() { /*클로저*/ }
       let cancleAction = UIAlertAction() { /*클로저*/ }
  		 // 팝업 구성	
       alertController.addAction(addCountAction)
       alertController.addAction(resetAction)
       alertController.addAction(cancleAction)
       alertController.addTextField() { /*클로저*/ }
       // 팝업 생성
       present(alertController, animated: true)
      }
  ```

## 1. 문법

```swift
let alertController = UIAlertController(title:"Counter", message: "메세지", preferredStyle: .alert)
```

## 2. 프로퍼티

* title: 팝업되는 창의 텍스트 콘텐츠
* message: title 하위에 생성되는 텍스트 콘텐츠
* preferredStyle: 팝업되는 창의 스타일
  1. alert: 화면 가운데 팝업. 버튼 갯수에 따라 버튼 배치 다름.(2개 이하: 가로, 3개 이상: 세로)
  2. actionSheet: 화면 하단에서 팝업.
  3. **alert vs actionSheet**
     1. aler: 창이 닫히기 전까지는 버튼 이외 다른 곳은 터치 불가(**모달창**).
     2. actionSheet: 다른 곳을 터치하면 들어감.



## 3. UIAlertAction

### 1) 문법

```swift
let resetAction = UIAlertAction(title: "Reset", style: .destructive) {_ in // 클로저로 기능을 정의한다.
    print("Reset") 
    self.countStorage = 0
    self.resultLabel.text = String(self.countStorage)
}
```

### 2) 프로퍼티

* title: 팝업되는 창에 생성되는 버튼의 텍스트 콘텐츠
* style: 버튼의 텍스트 콘텐츠의 스타일
  1. destructive: 빨간색 폰트로 출력된다.
  2. cancel: 파란색 폰트로 출력 (**하나의 UIAlertController에 최대 한개만 가능**)
  3. default: 기본



## 4. .addAction

* UIAlertController에 의해 UIAlertAction이 호출된다.

* add하는 순서대로 팝업에 반영된다.(단, cancel은 항상 마지막에 배치)

  ```swift
  alertController.addAction(addCountAction) // add하는 순서대로 팝업에 반영
  alertController.addAction(resetAction)
  alertController.addAction(cancelAction)
  ```



## 5. .addTextField

* 팝업창의 title아래에 Text Field를 추가한다.

* 클로저 형태로 기능을 추가한다.

  ```swift
  alertController.addTextField() { // 클로저 형태로 기능 추가
      $0.placeholder = "정수 값 입력" // 입력란에 placeholder 추가
  }
  ```

  



