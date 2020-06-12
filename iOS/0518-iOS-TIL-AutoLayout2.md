# AutoLayout Anchors

## iOS 6버전 이하

### 1. NSLayoutConstraint

지금은 잘 사용하지 않는 방식으로 코드가 매우 길다

```swift
private func activateNSLayoutConstraintConstraints() {
    firstView.translatesAutoresizingMaskIntoConstraints  = false
    secondView.translatesAutoresizingMaskIntoConstraints  = false
    
    // FirstView <-> View ( 부모 뷰 )
    NSLayoutConstraint(
      item: firstView,
      attribute: .top,
      relatedBy: .equal,
      toItem: view,
      attribute: .top,
      multiplier: 1,
      constant: 50
      ).isActive = true // 이게 한줄 표현
    
    NSLayoutConstraint(item: firstView, attribute: .leading,
                       relatedBy: .equal,
                       toItem: view, attribute: .leading,
                       multiplier: 1, constant: 50).isActive = true
    
    NSLayoutConstraint(item: firstView, attribute: .bottom,
                       relatedBy: .equal,
                       toItem: view, attribute: .bottom,
                       multiplier: 1, constant: -50).isActive = true
    
    NSLayoutConstraint(item: firstView, attribute: .trailing,
                       relatedBy: .equal,
                       toItem: view, attribute: .trailing,
                       multiplier: 1, constant: -50).isActive = true
    
    // SecondView <-> FirstView ( 자식 뷰 )
    NSLayoutConstraint(item: secondView, attribute: .top, relatedBy: .equal, toItem: firstView, attribute: .top, multiplier: 1, constant: 40).isActive = true
    NSLayoutConstraint(item: secondView, attribute: .leading, relatedBy: .equal, toItem: firstView, attribute: .leading, multiplier: 1, constant: 40).isActive = true
    NSLayoutConstraint(item: secondView, attribute: .bottom, relatedBy: .equal, toItem: firstView, attribute: .bottom, multiplier: 1, constant: -40).isActive = true
    NSLayoutConstraint(item: secondView, attribute: .trailing, relatedBy: .equal, toItem: firstView, attribute: .trailing, multiplier: 1, constant: -40).isActive = true
  }
```

### 2. Visual Format

```swift
private func activateVisualFormat() {
    /***************************************************
     Visual Format Syntax
     https://developer.apple.com/library/archive/documentation/UserExperience/Conceptual/AutolayoutPG/VisualFormatLanguage.html
     ***************************************************/
    
    firstView.translatesAutoresizingMaskIntoConstraints = false
    secondView.translatesAutoresizingMaskIntoConstraints = false
    
    let views: [String: Any] = ["firstView": firstView, "secondView": secondView]
    let metrics: [String: Any] = ["margin": 50, "padding": 40]
    let visualFormats: [String] = [
      "H:|-margin-[firstView]-margin-|",
      "V:|-margin-[firstView]-margin-|",
      "H:|-padding-[secondView]-padding-|",
      "V:|-padding-[secondView]-padding-|",
    ]// 정의한 views와 metrics를 visualFormat에 설정해서 쓴다.
    // Superview와 제약 조건 설정
    
    var allConstraints: [NSLayoutConstraint] = []
    for visualFormat in visualFormats {
      allConstraints += NSLayoutConstraint.constraints(
        withVisualFormat: visualFormat,
        metrics: metrics,
        views: views
      )
    }
    NSLayoutConstraint.activate(allConstraints)
  }
```

<br>

## iOS 9버전 이상

### 1. AutoresizingMask

부모 뷰가 바뀔때 자식뷰가 어떻게 바뀔건지 설정하는 메소드

1. 뷰 생성

```swift
ate func setupAutoresizingMask() {
// Width, Height
// TopMargin, LeftMargin, BottomMargin, RightMargin

firstView.frame = CGRect(
  x: 50, y: 50,
  width: view.frame.width - 100, height: view.frame.height - 100
)
secondView.frame = CGRect(
  x: 40, y: 40,
  width: firstView.frame.width - 80, height: firstView.frame.height - 80
)
```
2. autoresizingMask 설정

```swift
// 기본값 []
// 속성 값으로 Width, Height, TopMargin, LeftMargin, BottomMargin, RightMargin 을 가진다.
  firstView.autoresizingMask = [.flexibleWidth, .flexibleHeight]
// view의 크기에 따라 firstView의 width, height를 자동으로 맞추겠다.
  secondView.autoresizingMask = [.flexibleTopMargin, .flexibleLeftMargin]
// firstView의 크기에 따라 secondView의 top-margin, left-margin을 자동으로 맞추겠다.
```
3. 변화 확인 코드

```swift
  // 수퍼뷰의 위치나 크기가 변했을 때 서브뷰의 변화 확인
    firstView.frame = CGRect(
      x: 50, y: 100,
      width: 200, height: 200
    )
  
    firstView.frame.size.width = 100
    firstView.frame.size.width = 250
    firstView.frame.size.height = 400
}
```

<br>

### 2. Layout Anchors

현 시점에서 가장 보편적으로 사용되는 방법

1. 필수 설정

   ```swift
   firstView.translatesAutoresizingMaskIntoConstraints = false // auto resizing 속성 사용하지 않겠다. default값은 true이고, 이때는 autolayou을 사용하지 않을때.
   ```

2. view.topAnchor - 뷰에 설정 시

	```swift
private func activateLayoutAnchors() {
    
    // isActive를 통한 제약조건 활성화
    firstView.translatesAutoresizingMaskIntoConstraints = false // auto resizing 속성 사용하지 않겠다. default값은 true이고, 이때는 autolayou을 사용하지 않을때.
    // 실제 auto layout을 잡는 코드
    firstView.topAnchor.constraint(equalTo: view.topAnchor, constant: 50).isActive = true // view의 top에서 50만큼 떨어진 곳에 선을 그어라
    firstView.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 50).isActive = true
    firstView.bottomAnchor.constraint(equalTo: view.bottomAnchor, constant: -50).isActive = true // -값인 이유는 쉽게 말해서 '밑에서 얼마'이기 때문이다. 양수로 쓰려면 wiew의 y값에서 -50 한 값을 쓰면 된다.
    firstView.trailingAnchor.constraint(equalTo: view.trailingAnchor, constant: -50).isActive = true
    
    
    // activate를 통한 제약조건 활성화
    secondView.translatesAutoresizingMaskIntoConstraints = false
    NSLayoutConstraint.activate([
      secondView.topAnchor.constraint(equalTo: firstView.topAnchor, constant: 40),
      secondView.leadingAnchor.constraint(equalTo: firstView.leadingAnchor, constant: 40),
      secondView.bottomAnchor.constraint(equalTo: firstView.bottomAnchor, constant: -40), // bottom anchor를 빼면 height anchor를 넣어줘야한다. secondView.hei
      secondView.trailingAnchor.constraint(equalTo: firstView.trailingAnchor, constant: -40),
    ])
    }
	```

3. SafeArea에 설정 시

   ```swift
   private func activateLayoutAnchors() {
     	
     	let guide = view.safeAreaLayoutGuide // 생략시, equalTo에 계속 view.safeAreaLayoutGuide를 써줘야함
       
       // isActive를 통한 제약조건 활성화
       firstView.translatesAutoresizingMaskIntoConstraints = false // auto resizing 속성 사용하지 않겠다. default값은 true이고, 이때는 autolayou을 사용하지 않을때.
       // 실제 auto layout을 잡는 코드
       firstView.topAnchor.constraint(equalTo: guide.topAnchor, constant: 50).isActive = true // view의 top에서 50만큼 떨어진 곳에 선을 그어라
       firstView.leadingAnchor.constraint(equalTo: guide.leadingAnchor, constant: 50).isActive = true
       firstView.bottomAnchor.constraint(equalTo: guide.bottomAnchor, constant: -50).isActive = true // -값인 이유는 쉽게 말해서 '밑에서 얼마'이기 때문이다. 양수로 쓰려면 wiew의 y값에서 -50 한 값을 쓰면 된다.
       firstView.trailingAnchor.constraint(equalTo: guide.trailingAnchor, constant: -50).isActive = true
       
       
       // activate를 통한 제약조건 활성화
       secondView.translatesAutoresizingMaskIntoConstraints = false
       NSLayoutConstraint.activate([
         secondView.topAnchor.constraint(equalTo: firstView.topAnchor, constant: 40),
         secondView.leadingAnchor.constraint(equalTo: firstView.leadingAnchor, constant: 40),
         secondView.bottomAnchor.constraint(equalTo: firstView.bottomAnchor, constant: -40), // bottom anchor를 빼면 height anchor를 넣어줘야한다. secondView.hei
         secondView.trailingAnchor.constraint(equalTo: firstView.trailingAnchor, constant: -40),
       ])
     }
   ```

   



# Intrinsic Content Size

### 정의

Object 마다 가지는 고유의 Content 크기

예시)

Label ⇒ Text의 길이 / 크기에 따라 결정되는 Label의 크기

Button ⇒ Title의 길이 / 크기에 따라 결정되는 Button의 크기

ImageView ⇒ 삽입된 Image의 크기에 따라 결정되는 ImageView의 크기

```swift
// Label
testLabel.backgroundColor = .systemGreen
testLabel.text = "라벨"
print("testLabel intrinsic: \(testLabel.intrinsicContentSize)")
view.addSubview(testLabel)
testImgaeView.translatesAutoresizingMaskIntoConstraints = false
testImgaeView.centerYAnchor.constraint(equalTo: testView.centerYAnchor).isActive = true
testImgaeView.centerXAnchor.constraint(equalTo: testView.centerXAnchor).isActive = true

// Button
testButton.backgroundColor = .systemBlue
testButton.setTitle("이것은 버튼이다", for: .normal)
print("testButton intrinsic: \(testButton.intrinsicContentSize)")
view.addSubview(testButton)
testButton.translatesAutoresizingMaskIntoConstraints = false
testButton.centerXAnchor.constraint(equalTo: safeArea.centerXAnchor, constant: 0).isActive = true
testButton.centerYAnchor.constraint(equalTo: safeArea.centerYAnchor, constant: 50).isActive = true
```

<figure>
  <img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0518-iOS-TIL-AutoLayout2-01.png" alt="0518-iOS-TIL-AutoLayout2-01" style="zoom:50%;" />
  <figcaption>width와 height를 제한(Constraint)하지 않는 경우</figcaption>
</figure>

<br>

<figure>
  <img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0518-iOS-TIL-AutoLayout2-02.png" alt="0518-iOS-TIL-AutoLayout2-02" style="zoom:50%;" caption="abc"/>
  <figcaption>라벨과 버튼 각각의 intrinsic size</figcaption>
</figure>

<br>

### ContentHugging / Content Compression Resistance

ContentHugging(CH): 'Intrinsic Size 보다 커짐'에 대한 제한

Content Compression Resistance(CR): 'Intrinsic Size보다 작아짐' 에 대한 제한

CH 또는 CR 을 이용하는 경우가 일반적이고 둘다 설정하는 경우는 별로 없음

결론: 객체 적용된 Constraint와 CH(또는 CR)의 priority를 비교해서 우선순위가 높은 설정을 적용하는 것

view.setContentHuggingPriority(.defaultHigh, for: .horizontal)

view.setContentCompressionResistancePriority(.required, for: .vertical)

그래픽 예시 참고: [오토 레이아웃으로 iOS 앱 쉽게 개발하기](https://academy.realm.io/kr/posts/ios-autolayout/)

<br>

### Constraint 활용

애니메이션에 활용

예시)

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0518-iOS-TIL-AutoLayout2-03.gif" alt="0518-iOS-TIL-AutoLayout2-03" style="zoom:50%;" />

Storyboard로 적용한 Constraint를 IBOutlet으로 연결해서 설정가능하다.

view.layoutIfNeeded()로 레이아웃 새로 렌더링

방법 1) Constraint를 두개 만든 후 하나의 우선순위를 다르게하여 애니메이션

```swift
class ViewController: UIViewController {

    @IBOutlet weak var testView: UIView!
    @IBOutlet var testViewCenterYConstraints: NSLayoutConstraint! // 제한 역시 IBoutlet연결 가능.
    @IBOutlet var testViewCenterYConstraints2: NSLayoutConstraint!
    var toggle = false
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
    }
    @IBAction func buttonDidTap2(_ sender: Any) {
        UIView.animate(withDuration: 0.5, animations: {() -> () in
            
            // 1번. 제약조건을 두개 만든 후, 그 중 하나의 우선순위를 다르게 하여 이동시키는 방법
            let prior = self.toggle ? 250 : 900
            self.testViewCenterYConstraints2.priority = UILayoutPriority(Float(prior))
            print(self.toggle)
            self.view.layoutIfNeeded()
            self.toggle = !self.toggle
				})
		}
}
```

<br>

방법 2) 제약조건을 두개 만든 후, 그 중 하나를 install/unintstall 시켜서 이동시키는 방법

```swift
class ViewController: UIViewController {

    @IBOutlet weak var testView: UIView!
    @IBOutlet var testViewCenterYConstraints: NSLayoutConstraint! // 제한 역시 IBoutlet연결 가능.
    @IBOutlet var testViewCenterYConstraints2: NSLayoutConstraint!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
    }
    var toggle = false
    @IBAction func buttonDidTap2(_ sender: Any) {
        UIView.animate(withDuration: 0.5, animations: {() -> () in
            
            // 2번. 제약조건을 두개 만든 후, 그 중 하나를 install/unintstall 시켜서 이동시키는 방법
            self.testViewCenterYConstraints2.isActive.toggle() // 꺼지는 순간 제약조건이 사라진다. 따라서 제약조건을 strong으로 선언해야한다.
            self.view.layoutIfNeeded()
				})
		}
}
```

<br>

방법 3) 제약조건 값 자체를 변경해서 이동시키는 방법

```swift
class ViewController: UIViewController {

    @IBOutlet weak var testView: UIView!
    @IBOutlet weak var testViewCenterYConstraints: NSLayoutConstraint! // 제한 역시 IBoutlet연결 가능.
    @IBOutlet var testViewCenterYConstraints2: NSLayoutConstraint!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
    }
    var toggle = false
    @IBAction func buttonDidTap2(_ sender: Any) {
        UIView.animate(withDuration: 0.5, animations: {() -> () in
            
            // 3번. 제약조건 값을 변경해서 이동시키는 방법
            let position = self.toggle ? 100 : -100
            self.testViewCenterYConstraints.constant = CGFloat(position) // 실제로 적용되는 시점은 이 함수 밖이기 때문에
            self.view.layoutIfNeeded() // 레이아웃을 다시 잡아주는 메소드를 실행해야한다.
            self.toggle = !self.toggle
        })
    }  
}
```