## Conditional Statements

### if statements

* if, if / else, if / else-if / else

* 동작이 없더라도 else는 반드시 넣어주는게 좋다.

  ```swift
  if(condition) {
    statements
  }
  else if(condition) {
    statements
  }
  else {
    /*do nothing*/
  }
  ```

  

* Q) if 문을 2개하는 것과 차이점?

  ```swift
  var number = 9
  if number < 10 {
    print("10보다 작다")
  } else if number < 20 {
    print("20보다 작다")
  }
  //위는 조건에 맞는 부분만 한번 실행한다.
  //아래는 무조건 if문 2개를 모두 실행한다.
  if number < 10 {
    print("10보다 작다")
  }
  if number < 20 {
    print("20보다 작다")
  }
  ```

### switch statements

* **가능한 모든 사례를 반드시 다루어야 함(Switch must be exhaustive)**

  ```swift
   switch <#value#> {
   case <#value 1#>:
       <#respond to value 1#>
   case <#value 2#>,
        <#value 3#>:
       <#respond to value 2 or 3#>
   default:
       <#otherwise, do something else#>
   }//default를 이용해 모든 경우의 수에 대해 동작을 정의해야함. 단, true/false의 경우엔 default가 없어도 괜찮다.
  ```

* Interval matching

  범위 지정자를 통해 조건을 생성할 수 있다.

  ```swift
  let approximateCount = 30
  
  switch approximateCount {
  case 0...50: // awesome!
    print("0 ~ 50")
  case 51...100: // awesome!
    print("51 ~ 100")
  default:
    print("Something else")
  }
  ```

* Compound cases

  여러개의 값을 ,를 사용해서(or(||) 연산) 조건을 생성할 수 있다.

  ```swift
  let someCharacter: Character = "e"
  
  switch someCharacter {
  case "a", "e", "i", "o", "u":
    print("\(someCharacter) is a vowel")
  case "b", "c", "d", "f", "g", "h", "j", "k", "l", "m",
       "n", "p", "q", "r", "s", "t", "v", "w", "x", "y", "z":
    print("\(someCharacter) is a consonant")
  default:
    print("\(someCharacter) is not a vowel or a consonant")
  }
  ```

* Value binding

  두개의 값을 묶어서 조건을 생성할 수 있다.

  ```swift
  // x, y 좌표
  let somePoint = (9, 0)
  
  switch somePoint {
  case (let distance, 0), (0, let distance): //distance에 상관없이, y가 0 이거나 x가 0인 경우.
    print("On an axis, \(distance) from the origin")
  default:
    print("Not on an axis")
  }
  ```

* Where clause

  ```swift
  let anotherPoint = (1, -1)
  switch anotherPoint {
  case let (x, y) where x == y: // case (let x, let y)와 같은 표기, where x ==y 는 if x==y랑 같음.
      print("(\(x), \(y)) is on the line x == y")
  case let (x, y) where x == -y:
    print("(\(x), \(y)) is on the line x == -y")
  case let (x, y):
    print("(\(x), \(y)) is just some arbitrary point")
  }
  ```

* Fallthrough

  해당 키워드가 들어간 case가 실행되고 그 바로 아래 case까지 실행된다.

  ```swift
  let integerToDescribe = 3
  var description = "The number \(integerToDescribe) is"
  
  switch integerToDescribe {
  case 2, 3, 5, 7, 11, 13, 17, 19:
    description += " a prime number, and also"
    fallthrough
  default:
    description += " an integer."
  }
  print(description) // The number 3 is a prime number, and also an integer.가 출력된다.
  ```

### Early exit

* guard 조건이 true가 되어야 다음 statements들이 실행된다.

  ```swift
  func someFunction() {
    // ...
    // ...
    
    // if 문의 조건이 맞으면 어떤 코드를 수행할 것
    if Bool.random() {
      // ...
    }
  
    // gaurd문의 조건을 만족할 때만 다음으로 넘어갈 것
    guard Bool.random() else { return }
    
    // ...
    // ...
  }
  if true {
    
    if true {
  		
      if true {
        print(1)
      }
    }
  }
  guard true else {return}
  guard true else {return}
  guard true else {return}
  print(1)
  ```

## Tuples

* 여러개의 값을 갖는 변수를 생성

### Unnamed tuple

* 이름을 지정하지 않음

  ```swift
  asdf
  ```

### Decomposition

* 각 튜플을 할당하면서 이름을 지정

  ```swift
  let numbers = threeNumbers
  numbers
  numbers.0
  numbers.1
  
  
  let (first, second, third) = threeNumbers // 다음과 같이 할당하면서 이름 설정
  first
  second
  third
  
  
  // wildcard pattern
  let (_, second1, third1) = threeNumbers
  second1
  third1
  ```

### Nanmed tuple

* 각 튜플의 이름을 선언과 함께 지정

  ```swift
  //let iOS = (language: "Swift", version: "5")
  //let iOS: (language: String, version: String) = (language: "Swift", version: "5")
  //let iOS: (language: String, version: String) = ("Swift", "5")
  let iOS = (language: "Swift", version: "5")
  
  iOS.0
  iOS.1
  
  iOS.language
  iOS.version
  ```

### 함수에서의 활용

* 매게변수와 반환값에 사용 가능하다.

  ```swift
  func 튜플(a: Int, b: (Int, Int)) -> (first: Int, second: Int) {
    return (a + b.0,  a + b.1)
  }
  
  let result = 튜플(a: 10, b: (20, 30))
  result.first // 30. result.0 과 같다
  result.second // 40
  ```

### Compariton operators

* Tuple 은 7개 미만 요소에 대한 비교 연산자가 포함되어 있음

* 7개 이상의 요소 비교를 위해서는 비교 연산자를 직접 구현해야함.

  ```swift
  var something1: (Int, Int, Int, Int, Int, Int) = (1,2,3,4,5,6)
  var something2: (Int, Int, Int, Int, Int, Int) = (1,2,3,4,5,6)
  something1 == something2 //튜플 비교 연산 가능
  
  var something3: (Int, Int, Int, Int, Int, Int, Int) = (1,2,3,4,5,6,7)
  var something4: (Int, Int, Int, Int, Int, Int, Int) = (1,2,3,4,5,6,7)
  something3 == something4 //튜플 비교 연산 불가능
  
  // 다음 튜플 비교 연산에 대한 결과는?
  (1, "zebra") < (2, "apple") // true. 1 < 2
  (3, "apple") > (3, "bird") // false. apple > bird
  ("blue", 1) > ("bluesky", -1) // true. blue < bluesky
  ("일", 1) > ("이", 2.0) // true. ㅇ + ㅣ + ㄹ > ㅇ + ㅣ.
  
  (1, "zebra") < ("2", "apple") // error. 1과 "2"는 타입이 다름
  ("blue", false) < ("purple", true) // error. Boolean 타입은 비교 연산 불가.
  ```

### Tuple matching

* tuple을 조건문에 사용하는 방법. **순서에 유의한다.**

  ```swift
  let somePoint = (1, 0)
  
  switch somePoint {
  case (0, 0):
    print("\(somePoint) is at the origin")
  case (_, 0):
    print("\(somePoint) is on the x-axis")
  case (0, _):
    print("\(somePoint) is on the y-axis")
  case (-2...2, -2...2):
    print("\(somePoint) is inside the box")
  default:
    print("\(somePoint) is outside of the box")
  }
  // 순서 유의. 만약에 case(_, 0)이 case(0, 0)보다 먼저 온다면 case(0, 0)은 절대 실행되지 않음.
  ```

### Dictionary Enumeration

* key-value 쌍을 다루는 자료구조 = Dictionary

* 각각의 key-value가 tuple 형태로 되어있음

  ```swift
  let fruits = ["A": "Apple", "B": "Banana", "C": "Cherry"]
  
  for (key, value) in fruits {
    print(key, value)
  }
  print()
  
  
  for element in fruits {
      let x = 0;
      print(element.x, element.x);
      x = x + 1;
  //    print(element.0, element.1)
  //    print(element.key, element.value)
    // 어떤 식으로 출력해야 할까요?
  }
  ```

## Loops

* 반복문

### For-In loops

* items를 하나씩 item에 넣어서 code를 실행

  ```swift
   for <#item#> in <#items#> {
     <#code#>
   }
  ```

* 여러가지 사용법

  ```swift
  //for (int i = 0; i <= 5; i++) {
  //   C 스타일의 for 문
  //}
  
  for index in 1...4 {
    print("\(index) times 5 is \(index * 5)")
  } // 1 times 5 is 5
    // 2 times 5 is 10
    // 3 times 5 is 15
  
  // Wildcard Pattern
  
  for _ in 0...3 {
    print("hello")
  }// 단순 반복. item을 _로 처리한다.
  
  
  for chr in "Hello" {
    print(chr, terminator: " ") // terminator의 기본 값은 \n임. 따라서 print(chr)으로 선언하면 Hello가 세로로 출력됨.
  }
  print()
  
  
  let list = ["Swift", "Programming", "Language"]
  for str in list {
    print(str)
  }
  ```

### While loop

* C언어의 while문과 동일함(단, do-while == repeat-while)

## Control transfer statement

* 특정 코드에서 다른 코드로 제어를 이전하여 코드 실행 순서를 변경하는 것

  1. continue - 현재 _**반복문의 작업을 중단**_하고 다음 반복 아이템에 대한 작업 수행. <span style="color: red;">반복문을 아예 빠져버리는 break와 다름</span>

     ```swift
     for num in 0...15 {
       if num % 2 == 0 {
         continue
       }
       print(num)
     }
     // 결과값은? num이 짝수일때 for문이 중단되어 버리므로 홀수만 print된다.
     ```

     

  2. break - break가 포함된 해당 _**제어문의 흐름을 즉각 중단**_ (반복문, switch 문)

     ```swift
     for i in 1...100 {
       print(i)
       break
     }
     // 결과값은? 1
     
     for i in 1...100 {
       print(i)
       return
     }
     // 결과값은? error(왜냐, return은 함수에서만 쓰일 수 있다.)
     
     for num in 0...8 {
       if num % 2 == 0 {
         break
       }
       print(num)
     }
     // 결과값은? 아무것도 안나온다
     
     for i in 0...3 {
       for j in 0...3 {
         if i > 1 {
           break
         }
         print("  inner \(j)")
       }
       print("outer \(i)")
     }
     // 결과값은?
     //inner 0
     //inner 1
     //inner 2
     //inner 3
     //outer 0
     //inner 0
     //inner 1
     //inner 2
     //inner 3
     //outer 1
     //outer 2
     //outer 3
     ```

     

  3. fallthrough - _**switch 문**_에서 매칭된 case 의 실행이 종료된 후 그 다음의 case 까지 실행

  4. return - _**함수를 즉시 종료**_하고, return type에 해당하는 자료를 반환

     ```swift
     func isEven(num: Int) -> Bool {
       if num % 2 == 0 {
         return true
       }
       return false
     } // return num % 2 == 0 과 같은 코드
     
     isEven(num: 1)  // false
     isEven(num: 2)  // true
     
     
     func returnFunction() -> Int {
       var sum = 0
       
       for _ in 1...100 {
         sum += 20
         return sum
     //    return 5
       }
       return 7
     }
     print(returnFunction())
     // 결과값은? return sum에서 함수가 바로 종료되버리므로 20이 반환됨.
     ```

     

