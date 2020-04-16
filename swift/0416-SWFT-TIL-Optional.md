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

