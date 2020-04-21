## OOP 4대 특징

### 1) 추상화

* 대상의 불필요한 부분을 무시하여 복잡성을 줄이고 목적에 집중할 수 있도록 단순화시키는 것 (디자인 레벨)

  1. 사물들 간의 공통점만 취하고 차이점을 버리는 일반화를 통한 단순화
  2. 중요한 부분의 강조를 위해 불필요한 세부 사항을 제거하는 단순화

  ex) 지하철 노선도, 캐리커쳐

* 추상화는 대상에 대한 관점과 사용 목적에 따라 달라질 수 있음.

  ex) 자동차( 일반 고객의 관점 != 개발엔지니어의 관점)

  일반 고객은 '가격, 디자인, 크기, 색깔'

  개발엔지니어 '안정성, 미낭럼니아러'

#### protocol

* 스위프트에서는 protocol 이라는 것으로 표현가능. 스케치 느낌?

  예제 1)

  ```swift
  protocol Human {
    var name: String { get }
    var age: Int { get }
    var gender: String { get }
    var height: Double { get }
    
    func sleep()
    func eat()
    func walk()
  }
  
  //class User: Human {
  //} 요거를 playground를 통해서 fix 해버리면
  // 아래의 이런식으로 뼈대를 만들어줌
  class User: Human {
      var name: String
      
      var age: Int
      
      var gender: String
      
      var height: Double
      
      func sleep() {
          <#code#>
      }
      
      func eat() {
          <#code#>
      }
      
      func walk() {
          <#code#>
      }
      
  }
  ```

  예제 2)

  ```swift
  protocol Jumpable {
    var canJump: Bool { get set }
    var jumpHeight: Double { get }
  }
  
  
  class Cat: Jumpable { // 생성되는 클래스의 프로퍼티는 protocol의 그것과 이름이 같아야함. 그러나 세부 값은 다를 수 있음.
  //  let canJump = true  // get만 있을 땐 let도 가능. var도 가능
    var canJump = true  // get set을 둘다 해야하니까 var로 선언
  
    private var _jumpHeight = 30.0
    var jumpHeight: Double {
      get { _jumpHeight }
  //    set { _jumpHeight = newValue } // protocol에는 get만 있지만 실제 class로 만들때는 set도 추가가능. 만약에 get set이었으면 반드시 둘 다 있어야함.
    }
  }
  
  let cat = Cat()
  if cat.canJump {
    print(cat.jumpHeight)
  }
  ```

### 2) 캡슐화

* 구현 레벨에서의 개념

  1. 데이터 캡슐화: 연관된 상태와 행동을 하나의 단위(객체)로 캡슐화
  2. 정보 은닉화: 외부에 필요한 것만 알리고 불필요하거나 감출 정보는 숨김

  ex) 알약 = 캡슐안에 필요한 약재(class, methods, variables)를 담는다

  3. 객체가 독립적으로 자신의 상태와 역할을 책임지고 수행할 수 있도록 자율성 부여
  4. 접근 제한자(private)를 이용해 데이터를 외부로부터 보호하여 무결성을 강화하고 변화에 유연하게 대응
  5. 자세히 몰라도 되는 내부 동작방법을 숨기고 사용하는 방법만을 외부로 노출
  6. 외부에서 요청을 전달하면 수신 객체는 어떻게 처리할 지를 결정. 외부에서 그 내용을 자세히 알 필요 없음

  ex ) 선풍기, 핸드폰, 리모콘, 카메라, 캡슐 등

* 예시 )

  ```swift
  class 회사 {
    let 직원1: 직원 = 직원()
    let 직원2: 직원 = 직원()
  }
  class 직원 {
    private func 밤샘() {}
    private func 코피() {}
    
    func 결과물산출() {
      밤샘()
      코피()
    }
  }
  
  let company = 회사()
  company.직원1.결과물산출() // 회사는 직원1의 결과산출물만 알면된다
  company.직원2.결과물산출() // 회사는 직원2의 결과산출물만 알면된다
  // 직원이 어떻게 결과산물출을 만드는지(밤샘+코피 함수)는 직원이 알아서 하는 것
  ```

### 3) 상속

* 하나의 클래스의 특징(부모 클래스)을 다른 클래스가 물려받아 그 속성과 기능을 동일하게 사용하는 것
* 범용적인 클래스를 작성한 뒤 상속을 이용해 중복되는 속성과 기능을 쉽게 구현 가능
* 주요목적: 재사용과 확장
* 부모 클래스와 자식 클래스는 is-a 관계. like 'Bird is a animal', 'Human is a animal'

예시 )

```swift
class Cat {
  let leg = 4
  
  func cry() {
    print("miaow")
  }
}

let cat = Cat()
print(cat.leg) // 4
cat.cry() // "miaow"


class KoreanShortHair: Cat {
  let color: String = "black"
}

let koshort = KoreanShortHair()
koshort.leg // 4
koshort.cry() // "miaow"

koshort.color // "black"
//cat.color // error.
```

#### final

* 상속을 받지 못하도록 한다.

  ```swift
  print("\n---------- [ Final ] ----------\n")
  
  class Shape {
  }
  
  final class Circle: Shape {
  }
  
  class Oval: Circle { // error.
  }
  ```

### 4) 다형성

* 다양한 형태로 나타날 수 있는 능력
* 동일한 요청에 대해 각각 다른 방식으로 응답할 수 있도록 만드는 것
* 오버라이딩(상속과 관련) 오버로딩(상속과 무관)이 있으며 언어에 따라 오버라이딩만 지원하기도 함

#### 오버라이딩

* 상위 클래스에서 상속 받은 메서드를 하위 클래스에서 필요에 따라 재정의하는 것

* 동일 요청이 객체에 따라 다르게 응답

  ```swift
  class Shape {
    var title = "Shape"
    var color: UIColor
    // 오버라이딩 불가
    final var lineWidth = 3
    
    init(color: UIColor) {
      self.color = color
    }
    func draw() {
      print("draw shape")
    }
  }
  
  let shape = Shape(color: .cyan)
  shape.color
  shape.draw()
  
  
  print("\n---------- [ Rectangle ] ----------\n")
  
  class Rectangle: Shape {
    var cornerRadius: Double
    
    // 저장 프로퍼티 X
  //  override var color: UIColor  = .green
    
    // 계산 프로퍼티 O
    override var color: UIColor {
      get { super.color } // 부모 클래스의 프로퍼티 super.
      set { super.color = newValue }
    }
    override var title: String {
  //    get { "Rectangle" } // Rectangle
      get { super.title + " => Rectangle" } // Shape Rectangle
      set { super.title = newValue }
    }
    
    init(color: UIColor, cornerRadius: Double = 10.0) {
      self.cornerRadius = cornerRadius // 클래스 본인을 먼저 초기화해야
      super.init(color: color) // 부모클래스의 값을 초기화활 수 있다.
    }
  }
  
  //let rect = Rectangle(color: .blue) // color는 블루, 래디우스 10. 왜냐, 기본값이 10으로 선언되어있다. 135번줄
  //let rect = Rectangle(color: .blue, cornerRadius: 10.0) // color는 블루, 래디우스 10
  let rect = Rectangle(color: UIColor.blue)
  rect.color // blue
  rect.color = .yellow
  rect.color // yellow
  
  rect.cornerRadius // 10
  rect.lineWidth // 3
  rect.draw() // Rectangle
  
  shape.title // "Shape"
  rect.title //"Shape=>Rectangle"
  ```

#### 오버로딩

* 동일한 이름의 메서드가 매개 변수의 이름, 타입, 개수 등의 차이에 따라 다르게 동작하는 것

* 동일 요청이 매개 변수에 따라 다르게 응답

  1) 매개 변수의 이름으로 오버로딩

  ```swift
  func printParameter() {
    print("No param")
  }
  
  //func printParameter() -> String {   // Error
  //  "No param"
  //}
  
  func printParameter(param: Int) {
    print("Input :", param)
  }
  
  func printParameter(_ param: Int) {
    print("Input :", param)
  }
  
  print("=====")
  printParameter()
  printParameter(param: 1)
  printParameter(1)
  ```

  2) 매개 변수 타입으로 오버로딩

  ```swift
  func printParameter(param: String) {
    print("Input :", param)
  }
  
  func printParameter(_ param: String) {
    print("Input :", param)
  }
  
  func printParameter(name param: String) {
    print("Input :", param)
  }
  
  //func printParameter(param name: String) {   // Error. 호출될때 첫번째 함수와 같다고 판단한다. 호출 statement가 똑같기 때문.
  //  print("Input :", name)
  //}
  
  
  print("=====")
  printParameter(param: "hello")
  printParameter("hello")
  printParameter(name: "Hello")
  ```

  3) 매개 변수의 갯수로 오버로딩

  ```swift
  func printParameter(param: String, param1: String) {
    print("Input :", param, param1)
  }
  
  func printParameter(_ param: String, _ param1: String) {
    print("Input :", param, param1)
  }
  
  func printParameter(_ param: String, param1: String) {
    print("Input :", param, param1)
  }
  
  func printParameter(param: String, _ param1: String) {
    print("Input :", param, param1)
  }
  
  
  print("=====")
  printParameter(param: "hello", param1: "world")
  printParameter("hello", "world")
  printParameter("hello", param1: "world")
  printParameter(param: "hello", "world")
  ```

  