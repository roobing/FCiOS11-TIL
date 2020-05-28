# QnA

#### Q1) 매개변수에 개별?튜플?

매개변수를 여러개 써야할 경우 각각 개별 인자가 좋은지 tuple로 한번에 넘기는게 좋은지?

#### A1) 완료

그냥 상황에 따라 다름



#### Q2) 가변 인자 파라미터(Variadic parameter)

함수에서 사용하는 가변 인자 파라미터(Variadic parameter)기법은 변수 선언에서는 사용불가능 한지?

#### A2) 완료

값이 비어있는 배열 선언하는 것과 같다. 왜냐, 함수에서 입력된 가변인자를 배열로 처리한다. (.count는 배열에 사용할 수 있다.)

```swift
func multiParam(_ numbers: Double...) -> Double { // 가변인자 파라미터 => Double...
    var sum4average: Double = 0.0
    for eachNum in numbers{
        sum4average += eachNum
    }
    print(numbers.count) // 5. 
    return sum4average / Double(numbers.count)
}
print(multiParam(1,2,3,4,5))

var numbersArry: [UInt] = []
numbersArry = [1,2,3,4,5]
print(numbersArry.count) // 5. .count는 배열변수에만 사용 가능한 것 같다.
```



#### Q3) 인자 전달

각 데이터 타입(기본자료형, 집단자료형, 열거형, 구조체, 클래스)을 인자로 넘기는 방법에 대한 정리

#### A3) 



#### Q4) .index() 메소드

arry.index()를 사용할때 만약 같은 값을 같는 요소가 있으면 정확한 인덱스 값을 알 수 없다.

```swift
let arry: [Int] = [1,2,3,2,5]

for i in arry {
    let indexOfarry = arry.index(of: i)
    print("Type of i: \(type(of: i)), Value of i: \(i), index of i: \(indexOfarry!)")
}
//Type of i: Int, Value of i: 1, index of i: 0
//Type of i: Int, Value of i: 2, index of i: 1
//Type of i: Int, Value of i: 3, index of i: 2
//Type of i: Int, Value of i: 2, index of i: 1 // 여기가 3이 아니라 1이다.
//Type of i: Int, Value of i: 5, index of i: 4
```

#### A4) 완료

.index(of: )는 값을 기준으로 찾기 때문에 절대 index값(?)은 알수없다. 그래서 같은 값에 대해서는 그냥 .firstIndex(of: )랑 같은거임

해결방법은 아래처럼 많이 쓴다.

```swift
let arr = [1, 2, 4, 6, 2, 10]
for (index, value) in arr.enumerated() {
  print(index)
  print(value)
}
```



#### Q5) 클로저와 self.

클로저 내부에서 self. 를 사용하는 이유?

#### A5) 

ARC, Capture, escaping, noescaping 등의 개념이 필요함



#### Q6) UIApplicationDelegate vs UIViewController

App의 Life Cycle과 연관된 메서드를 정의해놓은 UIAppicationDelegate는 protocol인데, 왜 View의 Life Cycle과 연관된 메서드를 정의해놓은 UIViewController는 class인가?

#### A6) 세모완료

커스텀 코드를 작성하는 부분은 class이고, 그 커스컴 코드를 작성할 메서드들을 정의해놓은게 protocol

따라서 App Life Cycle => AppDelegate 클래스 // View Life Cycle => UIViewController 클래스



#### Q7) dismiss와 다운캐스팅

아래 코드 차이점?

#### A7) 완료

코드1

```swift
    @objc func bButtonAction(_ sender: UIButton) {
        if let vcB = self.presentingViewController as? ViewControllerB {
          goToCButton.setTitleColor(.systemRed)
          vcB.dismiss(animated: true)
          //...
        }
```

코드2

```swift
    @objc func bButtonAction(_ sender: UIButton) {
        if let vcB = self.presentingViewController {
          goToCButton.setTitleColor(.systemRed)
          vcB.dismiss(animated: true)
          //...
        }
```

dismiss는 presentingViewController(UIViewController 클래스)에 들어있는 메소드이기 때문에 결과가 같지만, goToCButton은 presentingViewController에 없는 프로퍼티이기 때문에 vcB를 ViewControllerB로 타입캐스팅(다운)해줘야 오류가 발생하지 않고 정상 동작하게 된다.



#### Q8) dismiss와 데이터 전달과 타입캐스팅

B view를 present 시킬때 A view에서는 그냥 인스턴스 생성해도 되고, B에서 A로 dismiss 할 때는 왜 타입캐스팅이 필요한가?

#### A8) 완료

<span style="color: red;">dimiss와 타입캐스팅</span>
아니다. 잘못 알고 있었다. dismiss랑 타입 캐스팅은 아무런 관련이 없다. 단순히 dismiss가 목적이라면 인스턴스를 생성할 필요도 없이 현재 뷰의 presentingViewController 프로퍼티의 dismiss 메소드를 호출하면된다.

```swift
self.presentingViewController?.dismiss(animated: true)
```

<span style="color: red;">데이터전달과 타입캐스팅</span>

그러나 전환된 뷰(presented)에서 전환시킨(presenting) 뷰의 프로퍼티나 메소드에 접근하고 싶을 때는 전환된 뷰에서 전환시킨 뷰의 인스턴스를 생성하고 다운캐스팅해서 접근하는 것이다.

```swift
guard let parentView = self.presentingViewController as? parentViewController else {
  return
}
parentView.goChildButton.setTitleColor(.systemRed, for: .normal)
parentView.dismiss(animated: true) // self.presentingViewController?.dismiss(animated: true) 해도 같은 결과
```



#### Q9) dismiss와 인스턴스 생성

전환된 뷰에서 전환시킨 뷰로 돌아가기 위한 dismiss 메소드를 사용하기 위해 전환시킨 뷰의 인스턴스를 생성할 때 representingViewController 프로퍼티 안쓰면 안되는 이유

```swift
guard let vcA = self.presentingViewController as? ViewControllerA else {
   return
}
// 위 처럼 하면 dismiss가 되지만
let vcA: ViewControllerA = ViewControllerA() // 이렇게 인스턴스를 생성하면 오동작한다.

vcA.countFromB = countFromA + 1
vcA.dismiss(animated: true)
```

#### A9) 완료

간단하게 말해, presentingViewController와 ViewController = ViewControllerA( )와 생성 주소가 다르다. 결국 아예 다른놈이 생성되는 것이므로 dimiss가 동작 안한다.





#### Q10) present와 인스턴스 생성

전환될 뷰의 인스턴스를 생성할 때 presentedViewController 프로퍼티를 사용하면 오동작 하는 이유

```swift
let vcB: ViewControllerB = ViewControllerB()
// 위 처럼 하면 present가 되지만
guard let vcB = self.presentedViewController as? ViewControllerB else {
      return
} // 이렇게 인스턴스를 생성하면 오동작한다.

vcB.countFromA = countFromB + 3
vcB.modalPresentationStyle = .fullScreen
present(vcB, animated: true)
```

#### A10) 완료

presentedViewController는 present( )이 후에 생성되기 때문에 의미가 없다.