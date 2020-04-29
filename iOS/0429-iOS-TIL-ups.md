# ups 특강1

## Storyboard를 쓰지 않을때 설정

### 	1. SceneDelegates.swift 삭제

### 	2. info.plist의 Application Scene Manifest 삭제

### 	3. TARGET-General-Deployment Info-Main interface 삭제

### 	4. AppDelegates.swift에서 //MARK SCENE 이하 코드 삭제



## 뷰 전환시 데이터 전달을 위한 문법

### 1. 타입캐스팅(다운)

* 상위 클래스 타입을 하위 클래스 타입으로 캐스팅할때
* 즉, 하위 클래스 인스턴스가 상위 클래스에 접근 가능하게 한다?
* 아래 vc는 상위 클래스 FirstViewController 프로퍼티에 접근할 수 있다.



### 2. nil-coalescing 연산자(nil 병합 연산자)

* A??B => A가 nil이 아니면 A를 반환하고, A가 nil이면 B를 반환한다



### 3. 코드

* 버튼 이벤트에 의해 SecondViewController의 textField객체의 내용이 FirstViewController의 content에 할당되고 최종적으로 Lable로 전달된다.

```swift
//SecondViewController 부분
@objc private func buttonAction(_ sender: UIButton) {
    switch sender {
    case cancelButton:
      break
    default:
      guard let vc = presentingViewController as? FirstViewController else { return } //타입다운캐스팅
      vc.content = contentTextField.text ?? "" // nil-coalescing
      
      break
    }
dismiss(animated: false)

  
//FirstViewController 부분
  var content = "Display" { // SecondViewController에서 content내용이 바뀌게 되고 아래 didSet 구문 실행
  didSet {
    displayLabel.text = content
  }
}
```



## 모달창 직접 만들기

* baseView를 만들어서 그 위에 각종 요소들(Label, Button 등등)을 올린다.

  ```swift
  private let baseView = UIView()
  let titleLabel = UILabel()
  let messageLabel = UILabel()
  private let contentTextField = UITextField()
  private let cancelButton = UIButton()
  private let enterButton = UIButton()
  
  view.addSubview(baseView)
  //...
  baseView.addSubview(titleLabel)
  //...
  baseView.addSubview(messageLabel)
  //...
  baseView.addSubview(contentTextField)
  //..
  baseView.addSubview(cancelButton)
  //..
  baseView.addSubview(enterButton)
  //...
  ```

  

## 기타

### 표현식 좌측의 타입을 잘 알아야한다.

ex) nextButton.center = ... 에서 center의 타입이 무엇인가?를 잘 알아야 적절하고 효과적으로 속성을 활용할 수 있다.