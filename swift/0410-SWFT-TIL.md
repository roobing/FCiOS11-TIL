## github repositiory 생성 tip

* gitignore.io에서 xcode, macOS, swift 키워드로 ignore 내용 생성 후 깃허브에 생성된 초기 .gitignore의 기존 내용에 덮어쓰기해서 사용한다.



## 1. Playground

### 실행

* cmd+shift+enter : 전체 실행
* shift+enter : 커서가 있는 해당 라인까지만 실행
* maually run: 내가 원할 때 원하는 부분까지 실행
* automatically run: 내용이 바뀌면 자동 재실행
* debug영역에 결과가 출력되는 함수: print()

### 주석

* ///: 세개짜리 주석은 주석뿐 아니라 quick help에 추가된다.
* 멀리라인주석 안에 멀티라인주석 가능하다.

### 세미콜론

* swift는 안쓰는 것이 기본. 하지만 한다고 해서 문제가 될건 없다.

* 한 라인에 여러 statement를 쓰고 싶을때는 반드시 사용

  ```swift
  print(1); print(2); print(3);
  ```



## 2. Constants and Variables

### Declare multiple constants and variables

* 상수(Constants): 한 번 설정되면 값 변경 불가. **let**

  ex) 최대 로그인 시도 횟수

  _**변수로 선언된 경우라도 선언 후 값 변경이 되지 않으면 상수로 바꿀 것을 에디터가 권장함**_

* 변수(Variables): 설정한 값을 변경 가능. __var__

  ex) 로그인 시도 횟수

  변수를 선언하기 전에 값을 할당 하면 안된다.

  ```swift
  currentLoginAttempt = 1
  var currentLoginAttempt = 0
  currentLoginAttemp++
  ```

* 잘 지켜야할 세가지

  네이밍 의미, 네이밍 컨벤션, 선언 순서

### Naming

* 조건을 잘 지켜서 이름을 지어라. 

## 3. Type annotation and type inference

* type annotation: 변수 선언 시 사용될 자료의 타입을 명확하게 지정하는 것

  다양한 선언 방식이 있음. 자료 참고.

* type inference: 변수 선언 시 초기화로 사용되는 값의 타입을 통해 변수의 타입을 추론하여 적용하는 것. 컴파일 타임 동안 적용됨. <span style="font-weight: 700;">단, return 값에는 추론이 적용 안됨.</span>

  ```swift
  var weight = 68.7
  type(of: weight) //결과로는 double이 나옴
  var isDog = true
  type(of: isDog) //결과로는 string이 나옴
  ```

* _**타입을 명시하는 것과 추론하는 것 사이의 컴파일 시간은 매우 대형 프로그램이 아니면 그 차이가 유의미한 수준은 아님.**_



## 4. Literals and Types

### Literals

* 소스코드에서 고정된 값으로 표현되는 문자(데이터) 그 자체

* 정수, 실수, 문자, 문자열 모두 가능

* 숫자의 경우 _를 통해 자릿수를 구분할 수 있다.

  ```swift
  var bigNum = 1_000_000_000
  var bigHexNum = 7DF_777_77D
  ```

### integer types

* Int의 경우 아이폰6부터 32bit가 아닌 64bit를 지원하므로 8byte가 된다.

* 사이즈 계산법

  ex)

```swift
Int8 // (-2^15) ~ (2^15 - 1). 부호비트가 존재 하므로 15비트
UInt8 // 0 ~ (2^16 - 1)
```

* Q. Int에 Int32.max ~ Int64.max 사이의 숫자를 넣었을 경우 생각해야 할 내용은?

```swift
let myNumber: Int = Int(Int32.max + 1)
//위의 statement는 iPhone5에서는 오류가 난다.
//iPhone6는 64bit 프로세서
//iPhone5는 32bit 프로세서
//즉 Int는 기기에 따라 그 크기가 다를 수 있음을 염두하고 설계해야한다.
```

위에서 발생하는 overflow를 무시할 수 있는 연산자가 존재

### overflow operation

연산자 앞에 &를 붙여준다.

```swift
Int8.max &+ 1 // 결과는 -128.
// 01111111 이게 Int8.max
// 01111111 &+ 1 = 10000000(= Int8.min=-128)
```

### Floating-point Literal

* 고정 소수점: 소수점 밑으로 고정

* 부동 소수점

  ```swift
  //precision: 소수점 특정 자리 이하로의 정확도는 보장하지 않는 다는말
  ```

### Boolean Literal

* true or false: 반드시 전부 소문자이어야함.

### String Literal

* 문자열 리터럴
* **기본적으로는 스택에 저장되지만, 일정 수준이상 길어지면 힙에 저장됨.**

### Character Literal

* 문자 리터럴
* 타입을 명시하지 않으면 한문자라도 기본적으로는 String 타입이다. 따라서 Character라고 타입을 명시해줘야함

### Typealias

* 문맥상 더 적절한 이름으로 기존 타입의 이름을 참조하여 사용하고 싶을 경우 사용

  ```swift
  typealias Name = String
  
  let name: Name = 'Tory' // let name: String = 'Tory'와 같다.
  ```

## 5. Type conversion

* swift는 같은 타입끼리의 연산이 아니면 오류로 판단.

  ```swift
  let height = Int8(5) // height는 Int8로 정해짐
  let width = 10 // width는 Int로 타입 추론됨
  let area = height * width // Int8과 Int는 타입이 다르므로 연산 오류 발생
  print(area) // 오류 발생
  
  let h = Int8(5) // h는 이미 Int8로 정해짐
  let x = 10 * h // *연산이 x에 대한 타입 추론보다 선행되기 때문에 10은 Int8로 타입 추론된다. 따라서 연산에서 오류가 나지 않고 x는 Int8로 타입 추론된다.
  print(x) // 즉, 오류가 나지 않는다.
  ```



## 6. Basic operation

### Terminology

### Assignment operation

* ++, -- 연산자 미지원. value += 1 로 구현.

* Tuple: 여러개의 값을 동시에 할당

  ```swift
  var (x, y) = (1, 2)
  ```

### Precedence

* 연산자 우선순위 참고(Playground > Swift > Swift Standard Library > Operator Declarations)

### Comparison operators

* 문자열 비교는? ASCII 기준으로 비교.

  ```swift
  "iOS" > "Apple" // true. 첫문자끼리 비교하고 동률이면 그 다음, 또 동률이면 그 다음.
  "Swift5" <= "Swift4" //false
  ```

### Logical operator

* AND(&&), OR(||), Negation(!)

* 연산 순서가 존재. 따라서 논리 연산자는 순서에 주의해야한다.

  ```swift
  func returnTrue() -> Bool {
    print("function called")
    return true
  }
  
  // 아래 3개의 케이스에서 returnTrue 메서드는 각각 몇 번씩 호출될까?
  // 우선순위 : && > ||
  
  print("\n---------- [ Case 1 ] ----------\n")
  returnTrue() && returnTrue() && false || true && returnTrue()
  //각각 1번씩. 총 3번.
  
  print("\n---------- [ Case 2 ] ----------\n")
  returnTrue() && false && returnTrue() || returnTrue()
  //첫 함수 1번, 마지막 함수만 1번. 총 2번.첫 함수는 무조건 호출된다.
  
  print("\n---------- [ Case 3 ] ----------\n")
  returnTrue() || returnTrue() && returnTrue() || false && returnTrue()
  //첫 함수만 1번. 첫 함수에서 true를 반환하면 뒤에는 뭐가 나와도 true이므로(||연산으로 묶임) 나머지 함수들은 호출 안된다.
  ```

### Range operators

* swift에만 있는 개념인가??

* 범위를 .을 이용해 표현

  a...b:  open

  a..<b: half open

  ..<b / a... /...b: one-side open

* 순서를 반대로 적용하는 방법

  .reserved()

  stride(from: b, through: a, by: -1)

## 7. Function

* 어떤 기능을 수행하기 위한 코드 묶음
* 함수는 왜 사용하는가? _**코드의 재사용성**_
* 함수의 종류
  1. Input과 Output이 모두 존재하는 함수
  2. Input이 없고 Output이 존재하는 함수
  3. Input이 있고 Output이 없는 함수
  4. Input과 Output이 모두 없는 함수
* Input이 없다 = 매개변수(Parameters)가 없다 / Output이 없다 = return 값이 void다

### Omit return

* 함수내에 statement가 하나밖에 없다면 return을 생략할 수 있다.

  ```swift
  func addTwoNumbers(a: Int, b: Int) -> Int {
    a + b
  //  return a + b   // 동일
  }
  ```

  

### Function scope

* 변수의 함수 내 사용 함수 외 사용에 대한 내용. 전역변수, 지역변수에 대한 내용인듯.

### Argument Label

* parameter name은 매개변수로써 기본이고, argument label은 매개변수에 별명 붙여주는거

  ```swift
  func multiplyNumber(lhs num1: Int, rhs num2: Int) {
    num1 + num2
  }
  
  multiplyNumber(lhs: 10, rhs: 10)
  //함수 내부에서 매개변수를 사용할때는 parameter name을 사용하고
  //함수 외부에서 함수를 호출하여 사용할때는 argument label을 사용한다.
  ```

* Omitting argument labels: argument label을 생략하는 경우

  ```swift
  // Omitting Argument Labels
  
  func someFunction(_ first: Int, second: Int) {
    print(first, second)
  }
  
  //someFunction(first: 1, second: 2)
  someFunction(1, second: 2)
  ```

* argument label 유무에 따른 호출 방법 종류

  |                    |        | 호출방식          |                   |                     |                    |         |          |
  | ------------------ | ------ | ----------------- | ----------------- | ------------------- | ------------------ | ------- | -------- |
  |                    |        | parmeter: literal | argument: literal | parameter: variable | argument: variable | literal | variable |
  | argument/parameter | n/y    | ok                | -                 | ok                  | -                  | nok     | nok      |
  |                    | y/y    | nok               | ok                | nok                 | ok                 | nok     | nok      |
  |                    | 생략/y | nok               | -                 | nok                 | -                  | ok      | ok       |

  

* Q. 왜 argument label을 쓰나요?

  ```swift
  func speak(to name: String) {
    print(name) // 매개변수이름(parameter name)을 내부에서 쓸 때와
  }
  speak(to: "Tory") // 외부에서 쓸 때 좀 더 인간적 언어에 가깝게 표현하기 위함인듯
  
  func speak(name: String) {
    print(name)
  }
  speak(name: "Tory") // 사람의 언어적 관점에서 보면 위의 경우와 비교해 좀 어색함. 
  ```

### Default parameter values

* parameter의 기본값을 설정해서 매개변수 없이 함수 호출될때 기본적인 매개변수 값을 갖도록 한다.

  ```swift
  func functionWithDefault(param: Int = 12) -> Int {
    return param
  }
  
  functionWithDefault(param: 6)
  // param is 6
  
  functionWithDefault()
  // param is 12
  ```

### Variadic parameters

* 매개변수의 갯수를 유동적으로 바꿀 수 있도록 매개변수를 선언하는 방법

  ```swift
  func arithmeticAverage(_ numbers: Double...) -> Double {
    var total = 0.0
    for number in numbers {
      total += number
    }
    return total / Double(numbers.count)
  }
  
  arithmeticAverage(1, 2, 3)
  arithmeticAverage(1, 2, 3, 4, 5)
  arithmeticAverage(3, 8.25, 18.75)
  ```

* 유동적 매개변수 중에서 하나를 선택하는 방법

  ```swift
  func arithmeticAverage2(_ numbers: Double..., _ last: Double) {
    print(numbers)
    print(last)
  }
  
  arithmeticAverage2(1, 2, 3, 5)
  
  
  func arithmeticAverage3(_ numbers: Double..., and last: Double) {
    print(numbers)
    print(last)
  }
  
  arithmeticAverage3(1, 2, 3, and: 5)
  ```

  

### Nested Function

* 함수안의 함수

* 함수안의 함수는 외부에서 호출할 수 없다.

  ```swift
  func chooseFunction(plus: Bool, value: Int) -> Int {
    func plusFunction(input: Int) -> Int { input + 1 }
    func minusFunction(input: Int) -> Int { input - 1 }
    
    if plus {
      return plusFunction(input: value)
    } else {
      return minusFunction(input: value)
    }
  }
  
  
  var value = 4
  chooseFunction(plus: true, value: value)
  chooseFunction(plus: false, value: value)
  plusFunction(input: 20) // error!!! 함수안의 함수는 호출 불가
  ```

  

오늘 할일

TIL repo 다시 제대로 만들기

블로그 다시 정리하기