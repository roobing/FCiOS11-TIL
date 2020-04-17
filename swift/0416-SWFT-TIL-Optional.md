## Optionals

* iOS 개발에서 많이 쓰이는 개념

### 값이 0인 것과 없는 것의 차이는?

* 값이 0인건 값이 있는 것
* 값이 없는건 아예 아무 것도 없는것

### 옵셔널 타입은 2가지 가능성을 지님

* 값을 전혀 가지고 있지 않음

* 값이 있으며, 그 값에 접근하기 위해 옵셔널을 벗겨(unwrap)낼 수 있음

  ```swift
  let possibleNumber = "123" // 123
  var convertedNumber = Int(possibleNumber) 
  type(of: convertedNumber) // optional(123). 왜냐, 변수 possiblenNumber는 String 타입이다(타입추론). 이 String 타입은 상황에 따라 형변환이 가능할수도(숫자 문자열인 경우"123")있고 아닐(문자 문자열인 경우"abc")수 도있다. 즉 값이 있을 수 도(변환이 가능한 경우) 있고 없을 수 도(변환이 불가능한 경우)있는 것이다. 따라서 convertedNumber는 optional이 된다.
  
  //type(of: Int("123")) // 이건 문자 리터럴이 형변환 되었으므로 optional Int
  //type(of: Int(3.14)) // 이건 숫자 리터럴이 형변환 되었으므로 무조건 Int
  //type(of: Int(3))
  
  ```

### Optional Type Declaration

* 옵셔널 타입 선언 방법

  ```swift
  //지금 당장 값이 없지만, 값이 들어올수도있으때 쓰는게 옵셔널
  var optionalType1: String?   // 자동 초기화 nil
  var optionalType2: Optional<Int> = nil   // 수동 초기화. (값 또는 nil). 잘 안쓴다.
  
  // Optional -> NonOptional (X)
  // Optional <- NonOptional (O)
  
  
  // 각 타입이 가질 수 있는 값의 범위
  // var nonOptionalNumber: Int    // 정수
  // var optionalNumber: Int?      // 정수 or nil
  ```

### Optional Unwrapping

* 변수에 적용된 Optional을 해제하는 두가지 방법
  1. Binding
  2. Foced Unwrapping

#### Optional Binding

* 비강제적인 optional 해제

* if 조건문 내에서 일반 상수(let)에 옵셔널 값을 대입하는 방식

  방법 1) 옵셔널 타입으로 선언된 변수 이용

  ```swift
  var optionalStr: String? = "Hello, Optional"
  //var optionalStr: String? = nil
  print(optionalStr)
  
  if let optionalStr = optionalStr {
    print(optionalStr) // optionalStr이 nil아 아닐때 할당된 내용을 print
  } else {
    print("valueless string") // optionalStr이 nil 일 때 "valueless string"을 print
  }
  ```

  방법 2) 형변환을 통해 옵셔널이 된 경우

  ```swift
  let someValue = "100"
  
  if let number = Int(someValue) {
    print("\"\(someValue)\" has an integer value of \(number)")
  } else {
    print("Could not be converted to an integer")
  }// 만약에 someValue를 "abc"로 할당하면 else 실행
  ```

  방법 3) 여러가지 조건을 한번에 사용

  ```swift
  // 여러 개의 옵셔널 바인딩과 불리언 조건을 함께 사용 가능
  if let firstNumber = Int("4"), // if문에서 ,는 AND 조건
    let secondNumber = Int("42"),
    firstNumber < secondNumber,
    secondNumber < 100 {
    print("\(firstNumber) < \(secondNumber) < 100")
  }
  ```

#### Forced unwrapping

* 강제적으로 옵셔널을 해제하는 방법(반드시 값이 있을거야! 라고 말하는거임ㅋㅋ)

* 옵셔널 타입 값 뒤에 '!'를 붙임

  ```swift
  var convertedNumber: Int? = 123 // 옵셔널 인트 타입.
  
  if convertedNumber != nil {
  //  print("convertedNumber has an integer value of \(convertedNumber).")
    print("convertedNumber has an integer value of \(convertedNumber!).")
  }
  
  print(convertedNumber) // warning. 옵셔널을 그대로 사용했으므로
  print(convertedNumber!) // ok. 옵셔널을 강제로 해제해줬으므로
  ```

* nil 이 할당된 옵셔널 타입에는 사용 불가

  ```swift
  convertedNumber = nil 
  convertedNumber // nil
  convertedNumber! // nil을 할당하고 forced unwrapping을 하면 run time error 발생. 왜냐, 반드시 값이 있다고 했는데 막상 주소찾아 가보니 nil.
  ```

#### Implicity Unwrapped Optional

* 옵셔널 타입으로 선언되었지만 정작 값을 사용할 때는 옵셔널이 해제되는 방법

* 쉽게 말해 옵셔널 타입을 논옵셔널 타입처럼 사용할 수 있게하는 방법

  ```swift
  // Non Optional 타입인 것처럼 함께 사용 가능
  var assumedString: String! = "An implicitly unwrapped optional string." // 느낌표가 붙은건 반드시 값이 있다라는걸 나타낼때 붙인다. 얘는 옵서녈이긴 한데 반드시 값이 있다. 언젠간 값이 할당될거다. 그러니까 일반 변수처럼 써도된다.
  let stillOptionalString = assumedString
  type(of: assumedString)
  type(of: stillOptionalString)
  print(assumedString)
  print(assumedString!)
  
  // String? -> String (x)
  // String! -> String (o)
  let implicitString: String = assumedString // 이렇게 옵셔널을 비옵셔널에 할당할 수 있게된다.
  type(of: implicitString)
  
  //assumedString = nil
  //print(assumedString!)
  ```

* 주의사항
  1. 추후 어느 순간에서라도 nil 이 될 수 있는 경우에는 이 것을 사용하지 말아야 함
  2. nil value 를 체크해야 할 일이 생길 경우는 일반적인 옵셔널 타입 사용
  3. 프로퍼티 지연 초기화에 많이 사용. 프로퍼티를 좀 뒤늦게 초기화해서 사용하고 싶을때 사용.

### NIL-coalescing operator

* coalesce : 큰 덩어리로 합치다

* 옵셔널을 좀 더 간결하게 사용하는 방법

  ```swift
  optionalStr = nil
  
  var result = ""
  if optionalStr != nil {
     result = optionalStr!
  } else {
     result = "This is a nil value"
  }
  print(optionalStr) // nil
  print(result) // "This is a nil value"
  
  //위의 내용을 아래처럼 간결하게
  
  let anotherExpression = optionalStr ?? "This is a nil value" // optionalStr이 nil이면 ?? 뒤에 값을 할당한다
  print(optionalStr) // nil
  print(anotherExpression) // "This is a nil value"
  ```

* 사용법의 좋은 예: 앱의 기본 색깔을 유저가 바꾸는 과정

  ```swift
  let defaultColorName = "red" // 앱을 깔았는데 기본 컬러가 레드다.
  var userDefinedColorName: String? // 유저가 할당할 수 있는 부분(옵셔널)
  
  var colorNameToUse = userDefinedColorName ?? defaultColorName // 유저가 정의한 컬러가 없으면(userDefinedColorName이 nil 이면) 기본 컬러를 쓴다(defaultColorName을 colorNameToUse에 할당한다.)
  print(colorNameToUse) // red
  
  userDefinedColorName = "green" // 유저가 그린으로 바꿨다
  colorNameToUse = userDefinedColorName ?? defaultColorName // 컬러에 그린을 할당한다.
  print(colorNameToUse) // green
  ```

* 삼항연산자(조건식 ? a : b)로 구현해보기

  ```swift
  optionalStrr != nil ? optionalStrr! : "This is a nil value"
  ```

### Optional chaining

* 옵셔널 타입으로 정의된 값이 하위 프로퍼티나 메소드를 가지고 있을 때, 이 요소들을 if 구문을 쓰지 않고 간결하게 사용할 수 있는 방법

  ```swift
  let greeting: [String: String] = [
    "John": "Wassup",
    "Jane": "Morning",
    "Edward": "Yo",
    "Tom": "Howdy",
    "James": "Sup"
  ]
  
  print(greeting["John"]) // key를 넣어서 value를 꺼냄. 근데 옵셔널로 나옴. 왜냐, key에 해당하는 value가 있을수도 있고 없을수도있기 때문이다.
  print(greeting["John"]?.count) // .count 말고도 다른 연관된 프로퍼티 혹은 메서드를 쓰기 위해선 옵셔널로 써줘야함. 따라서 결과적으로 .count도 옵셔널로 바뀜.
  print(greeting["NoName"]) // nil. 왜냐, NoName이라는 key가 없으므로 value도 없음
  
  // Optional Chaining
  print(greeting["John"]?.lowercased().uppercased()) // Optional(WASSUP). greeting["John"]은 옵셔널 타입이고 따라서 하위 프로퍼티나 메서드를 쓰려면 이렇게 optional chaining을 해줘야한다.
  print(greeting["John"]!.lowercased().uppercased()) // gretting["John"]에 대해 강제 옵셔널 해제해서도 하위  하지만 값이 없게되면 error.
  print(greeting["Alice"]?.lowercased().uppercased()) // nil
  print(greeting["Alice"]!.lowercased().uppercased()) // error.
  
  
  // Optional Binding
  if let greetingValue = greeting["John"] {
    print(greetingValue.lowercased().uppercased())
  }
  ```

### Optional Type Declaration의 두가지 방법

* [Int]? vs [Int?]

  ```swift
  var optionalArr1: [Int]? = [1,2,3]
  var optionalArr2: [Int?] = [1,2,3]
  
  var arr1: [Int]? = [1,2,3] // 배열의 각 요소값은 옵셔널이 아니고 반드시 인트. 배열자체가 옵셔널. 배열이 있으면 배열요소가 무조건 있는거고 배열이 없으면 값도 없는거임.
  //arr1.append(nil) // error. 배열 요소를 추가 불가능
  //arr1 = nil
  //print(arr1?.count) // Optional(3)
  //print(arr1?[1]) // Optional(2)
  
  var arr2: [Int?] = [1,2,3] // 배열자체는 무조건 있는거고 배열요소가 옵셔널임.
  arr2.append(nil) // [1, 2, 3, nil]
  //arr2 = nil // error. arr2=[]은 됨.
  print(arr2.count) // 4. 배열자체는 Optional이 아니기 때문. 따라서 ? 도 필요없음.
  print(arr2[1]) // Optional(2). 배열요소인 arr2[1]은 옵셔널임.
  print(arr2.last) // Optional(nil)
  ```

### Optional Function Type

* 함수에 Optional type 적용하기

  ```swift
  func voidFunction() {
    print("voidFunction is called")
  } // input, output이 void
  
  //var functionVariable: () -> () = voidFunction // void 함수를 변수에 할당
  var functionVariable: (() -> ())? = voidFunction // void 함수를 옵셔널 변수에 할당
  functionVariable?() // ().
  
  functionVariable = nil
  functionVariable?() // nil
  
  func sum(a: Int, b: Int) -> Int {
    a + b
  }
  sum(a: 2, b: 3)
  
  //var sumFunction: (Int, Int) -> Int = sum(a:b:) // 함수의 input, output 타입을 맞춰서 변수에 할당한거임
  var sumFunction: ((Int, Int) -> Int)? = sum(a:b:) // 옵셔널로 바꿔준거임.
  type(of: sumFunction)
  
  //print(sumFunction?(1, 2))
  //print(sumFunction!(1, 2))0
  
  //sumFunction = nil
  //sumFunction?(1, 2)
  //sumFunction!(1, 2) // !을 통해 무조건 값이 있을거라 했는데 nil을 할당
  
  var dict1: [String: String?] = [
      "A":nil //이런것도 가능
  ]
  dict1["C"] // 이렇게 하면 nil이 value로 반환되는거는 swift내부 로직이다.
  ```

  