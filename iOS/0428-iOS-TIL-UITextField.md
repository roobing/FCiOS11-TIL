# UITextField

## Storyboard

### Attributes inspector

* Dynamic type: 접근성(손쉬운사용)을 고려한 텍스트 속성

* Min Font Size: 입력창에 타이핑이 길어질 수 록 글자가 작아지는데, 이때의 최소 폰트 크기를 지정

* Secure Text Entry: 비밀번호 입력 시 안보이게 하는 설정

### Connections inspector

* Sent Events
  *  Did End On Exit: 텍스트 입력후 return 입력하면 키입력 창 내려간다



## UITextField Life Cycle

### 터치-텍스트입력-리턴 패턴

1. Text Field 터치(키보드 올라옴) - Editing Did Begin
2. t입력 - Editing Changed
3. t입력 - Editing Changed
4. t입력 - Editing Changed
5. t입력 - Editing Changed
6. return 입력(키보드 내려감) - Did End On Exit
7. 6번 직후 - Primary Action Triggered
8. 7번 직후 - Editing Did End

### 리턴 없이 터치로 입력 칸 옮기는 패턴

1. Text Field 터치(키보드 올라옴) - Editing Did Begin
2. 다른 Text Field 터치(키보드 유지) - Editing Did End
3. 2번 직후 - Editing Did Begin

### viewDidLoad vs viewWillAppear

* 시점차이
  * viewDidLoad: 실행될때 메모리에 딱 한번 올라감
  * viewWillAppear: 실행 될때 마다 메모리에 올라갔다 내려갔다 함



## Text Field 구분 법

* Text Filed 요소가 여러개 있지만 모두 같은 IBAction으로 연결되어있을때, 어떤 Text Field로부터의 입력인지 구분하는 방법

### 1. 스토리보드로 만든 경우

#### 1) 입력된 텍스트로 구분하는 법

```swift
@IBAction func editingDidEnd(_ sender: UITextField) { // sender의 타입을 UITextField로 바꿔주고
         print("editingDidEnd")
        print(sender.text) // 입력된 text를 print로 찍어본다
    }
```



#### 2) @IBOutlet으로 설정해서 구분하는 법

```swift
@IBOutlet weak var idTextField: UITextField! // 첫번째 Text Field 요소
@IBOutlet weak var pwTextField: UITextField! // 두번째 Text Field 요소
```



#### 3) Tag 값을 설정해서 sender.tag로 구분하는 법

* Attributes inspector의 View 메뉴에서 Tag 값을 지정해줘서 호출한다

  ```swift
  @IBAction func editingDidEnd(_ sender: UITextField) {
      if sender.tag == 1 {
        print("From Text Field 1")
      }
      else if sender.tag == 2{
        print("From Text Field 2")
      }
  }
  ```

  

#### 4) Accessabiltity label을 설정해서

* Identity Inspector의 Accessability의 Label값을 설정해서 호출한다.



### 2. 코드로 생성한 경우

* .addTarget( ) 메서드를 이용해서 연결한다.(해당 selector는 @objc 함수로 생성해야함)

```swift
nameTextField.frame = CGRect(x: 30, y: 100, width: 300, height: 40)
nameTextField.placeholder = "이름 입력"
nameTextField.font = .preferredFont(forTextStyle: .title1)
nameTextField.textAlignment = .center
nameTextField.isSecureTextEntry = true
nameTextField.keyboardType = .URL
view.addSubview(nameTextField)

idTextField.addTarget(self, action: #selector(editingDidBegin(_:)), for: .editingDidBegin)

@objc func editingDidBegin(_ sender: UITextField) {
  //code...
}
```



## 최초 응답자(First Responder)

* 터치 관련 이벤트 발생시, UIWindow가 우선적으로 응답할 객체를 가리키는 포인터
* 사용자가 텍스트 필드를 터치하면 윈도우는 최초 응답자 포인터를 해당 텍스트 필드로 옮겨준다
* 웹의 포커스(Focus)와 유사한 개념
* .becomeFirstResponder()와 .resignFirstResponder()

```swift
override func viewWillAppear(_ animated: Bool) {
  super.viewWillAppear(animated)
  idTextField.becomeFirstResponder() // viewWillAppear 단계에서 idTextField라는 Text Field 객체가 최초 응답자로 됨. 해당 입력칸에 대해 키보드가 올라옴.
}
    
@IBAction private func didTapButton(_ sender: Any) {
    idTextField.resignFirstResponder() // didTapButton이라는 Event가 발생하면 idTextField라는 Text Field 객체가 최초 응답자에서 해제됨(키보드 내려감)
    view.endEditing(true) // didTapButton이라는 Event가 발생하면 모든 입력 종료(키보드 내려감)
}
```

