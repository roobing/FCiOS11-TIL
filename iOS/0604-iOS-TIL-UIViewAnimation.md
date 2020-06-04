# UIViewAnimation

## 방법

1. UIView.animated()
2. UIView.animatedKeyFrames() + UIView.addKeyFrame()
3. UIView.transform()

### 1. animated()

* 초기위치 설정

```swift
override func viewWillAppear(_ animated: Bool) {
  super.viewWillAppear(animated)
  //화면 왼쪽에서 나타나도록 하기 위해 미리 위치 잡기(화면 왼쪽으로 벗어난 위치로 보낸다)
  userIdTextField.center.x = -view.frame.width
  passwordTextField.center.x = -view.frame.width
  loginButton.center.x = -view.frame.width
}
```

* 최종위치 및 초기위치와 최종위치 사이의 애니메이션 정의
  * tip. 아이디필드의 completion에 패스워드필드 애니메이션 내용을 넣어주면, 아이디필드 애니메이션이 끝나고 패스워드필드 애니메이션이 시작된다.

```swift
override func viewDidAppear(_ animated: Bool) {
  super.viewDidAppear(animated)
  UIView.animate(withDuration: 0.6, animations: {
      //        self.userIdTextField.center.x = self.view.center.x // view의 절대좌표를 그대로 대입하니까 정가운데가 아님
      self.userIdTextField.center.x = self.userIdTextField.superview!.bounds.midX
      print(self.userIdTextField.center.x) // 최종값이 출력됨
      // 1) -view.frame.width
      // 2) 이동하는 동안 계속 호출
      // 3) 중간값
      // 4) 최종값
  })
  UIView.animate(withDuration: 0.6, delay: 0.5, animations: { // delay 준다
      self.passwordTextField.center.x = self.passwordTextField.superview!.bounds.midX
  })
  
  // 타겟 위치 벗어났다가 돌아오기(usingSpringWithDamping 0 ~ 1)
  UIView.animate(withDuration: 0.6, delay: 1, usingSpringWithDamping: 0.5, initialSpringVelocity: 0, options: [.curveEaseIn], animations: { // delay 준다
      self.loginButton.center.x = self.loginButton.superview!.bounds.midX
}) { isFinished in
    print("isFinished: ", isFinished) // 애니메이션 성공 여부 반환
}
// option: 애니메이션의 속도 그래프. 기본값 curveEaseIn(점점빨라졌다가 유지했다가 점점느려진다)
// options: [.autoreverse, .repeat] 애니메이션 반복 옵션
// usingSpringWithDamping: 0 ~ 1
}
```

<br>

### 2. animatedKeyFrames()

* key frame 단위로 설정해주는 방법

```swift
@IBAction private func login(_ sender: Any) {
    addAnimatedKeyFrames()
}

func addAnimatedKeyFrames() {
    UIView.animateKeyframes(withDuration: 4, delay: 0, animations: {
        
        // 애니메이션 시작과 동시에 1초동안 우측으로 100만큼 이동
        UIView.addKeyframe(withRelativeStartTime: 0, relativeDuration: 0.25, animations: {
            self.loginButton.center.x += 100.0
        })// withRelativeStartTime: 시작 시간, elativeDurations: withDuration의 0.25배
        
        // 1초 뒤 1초간 아래로 이동
        UIView.addKeyframe(withRelativeStartTime: 0.25, relativeDuration: 0.25, animations: {
            self.loginButton.center.y += 100.0
        }) // withRelativeStartTime 4 * 0.25 = 1, 4 * 0.25 = 1
        
        // 2초 뒤 1초간 왼쪽으로 이동
        UIView.addKeyframe(withRelativeStartTime: 0.5, relativeDuration: 0.25, animations: {
            self.loginButton.center.x -= 100.0
        }) // withRelativeStartTime 4 * 0.5 = 2, 4 * 0.25 = 1
        
        // 3초 뒤 1초간 위으로 이동
        UIView.addKeyframe(withRelativeStartTime: 0.75, relativeDuration: 0.25, animations: {
            self.loginButton.center.y -= 100.0
        }) // withRelativeStartTime 4 * 0.5 = 2, 4 * 0.25 = 1
    })
}
```

<br>

### 3. transition()

```swift
func countDown() {
    countDownLabel.isHidden = false
    
    //하나의 뷰를 다른 뷰로 전환
      UIView.transition(from: <#T##UIView#>, to: <#T##UIView#>, duration: <#T##TimeInterval#>, options: <#T##UIView.AnimationOptions#>, completion: <#T##((Bool) -> Void)?##((Bool) -> Void)?##(Bool) -> Void#>)
    // 뷰 하나로 전환
      UIView.transition(with: <#T##UIView#>, duration: <#T##TimeInterval#>, options: <#T##UIView.AnimationOptions#>, animations: <#T##(() -> Void)?##(() -> Void)?##() -> Void#>, completion: <#T##((Bool) -> Void)?##((Bool) -> Void)?##(Bool) -> Void#>)
    
    
    UIView.transition(with: self.countDownLabel,
                      duration: 0.3, // 전환 애니메이션 지속 시간
                      options: [.transitionFlipFromRight],
                      animations: {self.count -= 1})
    { (_) in
        // 다음 카운트로 넘어갈 때까지 대기 시간
        DispatchQueue.main.asyncAfter(deadline: .now() + 0.3) { // 현재로부터 0.3초 뒤에 실행한다. main 이라는 큐에서 0.3초 마다 꺼내서 실행. UIview.transition에는 delay가 없어서 이걸로 0.3초의 딜레이를 준것임
            guard self.count == 0 else { return self.countDown()}
            self.count = 4
            self.countDownLabel.isHidden = true
            self.activityIndicatorView.stopAnimating()
        }
    }
}
```

<br>

## 기타

### 인디케이터(로딩아이콘) 사용법

```swift
@IBOutlet private weak var activityIndicatorView: UIActivityIndicatorView!
//...
self.activityIndicatorView.startAnimating()
//...
self.activityIndicatorView.stopAnimating()
```

<br>

### animate vs keyframes

animate는 그냥 withduration 안에서 통짜로 돌리는데

keyframse는 전체 애니메이션을 세분화 하는 느낌

animate도 그냥 여러개 쓰면 나눌수는 있음.