## Enumarations

### 정의

* 연관된 값의 그룹에 대해 공통 타입을 정의한 뒤 type-safe 하게 해당 값들을 사용하기 위한 방법

  ```swift
  enum CompassPoint { // 타입명은 Pascal 명명법으로
    case north // 각각의 case는 Camel 명명법으로
    case south
    case east
    case west
  }
  enum Planet {
    case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune, pluto
  } // 가로형도 가능
  ```

### Matching Enumeration Values

* enum 값 사용하기(?)

  ```swift
  enum ClassesOfFc {
    case iOS, frontEnd, backEnd, dataScience, uxUi, disitalMarketing
  }
  
  let myClass: ClassesOfFc = .iOS // 또는
  let myClass = ClassesOfFc.iOS
  
  switch myClass {
    case .iOS:
    print("My class is iOS")
    default:
    print("other classes")
  }
  ```

### Raw Values

* 멤버 선언과 함께 값을 정해주는 것

* 멤버는 문자로써 그 의미를 나타내고 거기에 값을 할당함으로써 사용을 편리하게 한다.

* 반드시 타입 어노테이션으로 표기해줘야함

* 각각의 rawValue는 해당 enum안에서 유일해야함

  ```swift
  enum WeekdayAgain: Int {
    case sunday = 1, monday = 1, tuesday = 2, wednesday, thursday, friday, saturday 
  }// error. 왜냐, sunday == monday == 1
  ```

#### 기본형

* 기본적인 방법

  ```swift
  enum HTTPCode: Int {
    case OK = 200
    case NOT_MODIFY = 304
    case INCORRECT_PAGE = 404
    case SERVER_ERROR = 500
  }
  HTTPCode.OK.rawValue // 200
  ```

  

#### String, Double, Float, Character 형

* Int 타입 이외도 가능

  ```swift
  enum Gender: String {
    case male = "남자", female = "여자", other = "기타"
  }
  Gender.male // male
  Gender.male.rawValue // "남자"
  ```

#### 암시적 방법

* 타입 어노테이션과 멤버들 중 부분 할당을 통해 나머지를 암시적으로 설정.

  ```swift
  enum WeekdayAgain: Int {
    case sunday, monday = 100, tuesday = 101, wednesday, thursday, friday, saturday
  }// 따로 할당 안해주면 첫 요소는 0이 기본. 
  
  WeekdayAgain.sunday // sunday(이건 문자열 타입도 아니고 정수 타입도 아니고 WeekdayAgain 타입임)
  WeekdayAgain.sunday.rawValue // 0 (Int 타입)
  
  WeekdayAgain.wednesday // wednesday(sunday와 마찬가지)
  WeekdayAgain.wednesday.rawValue // 102 (Int 타입)
  
  
  
  enum WeekdayNameAgain: String {
    case sunday, monday = "mon", tuesday = "tue", wednesday, thursday, friday, saturday
  }
  WeekdayNameAgain.tuesday // tuesday (WeekdayNameAgain 타입)
  WeekdayNameAgain.tuesday.rawValue // "tue" (String 타입)
  
  WeekdayNameAgain.wednesday // wednesday (WeekdayNameAgain 타입)
  WeekdayNameAgain.wednesday.rawValue // "wednesday" (String 타입)
  ```

#### Initializing from a raw values

* 다른 부분(서버 등등)에서 정의된 값을 swift에서 사용하고자 할때(?)

  ```swift
  enum HTTPCode: Int {
    case OK = 200
    case NOT_MODIFY = 304
    case INCORRECT_PAGE = 404
    case SERVER_ERROR = 500
  }
  
  let codeFromServer = 404 // 서버로 부터 오류 코드를 받았다.
  if let codeNumber = HTTPCode(rawValue: codeFromServer) {
    switch codeNumber {
      case .OK:
      print("Request complete")
      default:
      print("Somethig wrong") // 이게 print 됨
    }
  }
  else {
    print("No response from server")
  }
  ```

### Associated Values

* enum의 멤버가 사용되는 시점에서 보조 값을 설정하는 방법.

  (선언과 함께 정해주는 값(=.rawValue로 접근하는 값)과 다른 거임)

  ```swift
  enum Barcode {
    case upc(Int, Int, Int, Int)
    case qrCode(String)
  }// 상품의 바코드 종류에 따라 upc 혹은 qrCode가 사용됨
  
  var productBarcode = Barcode.upc(8, 85909, 51226, 3)// 또는
  var productBarcode = Barcode.qrCode("ABCDEFGHIJKLMNOP") //가 쓰이면서 각 멤버에 보조값을 할당함. (지금 두줄은 error임. 변수 선언은 하나만 있어야함)
  
  switch productBarcode {
  case let .upc(numberSystem, manufacturer, product, check):
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
  case let .qrCode(productCode):
    print("QR code: \(productCode).")
  }// 상품의 바코드 종류에 따라 각 바코드에 해당하는 방법으로 상품정보가 print됨
  ```

* Raw values 방법과 함께 사용할 수 없다

  ```swift
  enum Barcode: Int { // error. rawValue 방법.
    case upc(Int, Int, Int, Int)
    case qrCode(String)
  }
  ```

### Nested

* eunm은 클래스나 구조체처럼 내부에 연산 프로퍼티와 메소드를 정의할 수 있다.

  ```swift
  enum Device: String {
    case iPhone, iPad, tv, watch
    
    func printType() { // enum에 함수도 포함할 수 있다. enum 내부에서 쓸때는 이미 값을 가진 상태에서 호출되기때문에 파라미터가 필요없다(?)이게 뭔소리여
      switch self { // self = device 타입이 가진 값 = iPhone, iPad, tv, watch
      case .iPad, .iPhone, .tv:
        print("device :", self)
      case .watch:
        print("apple Watch") // 
      }
    }
  }
  
  let iPhone = Device.iPhone
  iPhone.printType() // 아래 예시와 비슷하다고 생각하면됨
  // let str = "Hello"
  // str.count => 5
  Device.iPhone.printType()// 바로 위에 두줄을 줄인 것.
  ```

* enum 안에 enum 선언도 가능

  ```swift
  enum Wearable { // enum안에 enum 넣기
    enum Weight: Int {
      case light = 1
      case heavy = 3
    }
    
    case helmet(weight: Weight) // 변수명이 weight이고 타입은 Weight임(Weight라는 타입은 위에 enum으로 정의됨)
    case boots
    
    func info() -> Int {
      switch self {
      case .helmet(let weight):
        return weight.rawValue * 2
      case .boots:
        return 3
      }
    }
  }
  let boots = Wearable.boots
  boots.info() // 3
  
  var woodenHelmet = Wearable.helmet(weight: .light)
  woodenHelmet.info() // 1((weight: .light)의 rawValue) * 2 = 2
  
  var ironHelmet = Wearable.helmet(weight: .heavy)
  ironHelmet.info() // 3((weight: .heavy)의 rawValue) * 2 = 6
  ```

### Mutating

* enum 타입 내부 함수에서 자기 자신의 값을 바꿔야 하는 경우에 mutating 키워드 필요

  방법 1)

  ```swift
  enum Location {
    case seoul, tokyo, london, newyork
  
    mutating func travelToTokyo() {
      self = .tokyo
    }
  }
  
  var location = Location.seoul // location에 seoul을 할당했다.
  location // seoul
  
  location.travelToTokyo() // location의 값을 tokyo로 바꿨다
  location // tokyo
  ```

  방법 2)

  ```swift
  enum Location {
    case seoul, tokyo, london, newyork
    
    mutating func travel(to location: Location) {
      self = location
    }
  }
  
  var location = Location.seoul // location에 seoul을 할당했다.
  location // seoul
  
  location.travel(to: .london) //location에 매개변수 to로 전달될 값을 할당한다.
  location // london
  
  ```

  방법 3)

  ```swift
  enum Location {
    case seoul, tokyo, london, newyork
  
    mutating func travelToNextCity() {
      switch self {
      case .seoul: self = .tokyo
      case .tokyo: self = .newyork
      case .newyork: self = .london
      case .london: self = .seoul
      }
    }
  }
  
  var location = Location.seoul // location에 seoul을 할당했다.
  location // seoul
  
  location.travelToNextCity() // tokyo
  location.travelToNextCity() // newyork
  location.travelToNextCity() // london
  location // london
  
  ```

  

