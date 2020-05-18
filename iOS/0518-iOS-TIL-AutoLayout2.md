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

1. view.topAnchor - 뷰에 설정 시
2. view.safeAreaLayoutGuide.topAnchor - SafeArea에 설정 시

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





# Intrinsic Content Size

### 정의

뷰 마다 가지는 고유의 콘텐츠 크기

레이아웃

뷰 마다 가지는 고유의 사이즈

이 사이즈에 따라서 제한할수 있는 최소 조건이 다름(이미지뷰의 경우 버티컬/호리젠털 제한만 있어도 가능)

인트린식 사이즈기준으로 chcr 가짐

ch 또는 cr 을 이용하는 경우가 일반적이고 둘다 설정하는 경우는 별로 없음



view.setContentHuggingPriority(.defaultHigh, for: .horizontal) view.setContentCompressionResistancePriority(.required, for: .vertical)



제한을 두개 만들어서 두개의 우선순위를 변경해서 애니메이션 적용하는 방법도 있음

