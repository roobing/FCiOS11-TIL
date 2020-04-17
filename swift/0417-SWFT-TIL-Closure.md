## Closure

* 코드에서 사용하거나 전달할 수 있는 독립적인 기능을 가진 블럭
* 함수도 클로저의 일종
* 어떤 기능을 변수에 담아서 사용할 수 있다(?)

### 장점

* 문법 간소화 / 읽기 좋은 코드
* 지연 생성. 내가 필요할때 만들어진다.
* 주변 컨텍스트의 값을 캡쳐하여 작업 수행 가능
* 함수는 미리 만들어놔야(정의해놔야) 사용(단순 호출, 매개변수로써 전달 등등)할 수 있는데, 클로저를 이용하면 함수 정의와 동시에 사용할 수 있다.

### 유형

1. Global functions: 이름을 가지며, 어떤 값도 캡쳐하지 않는 클로저. 전역변수. 프리펑션

```swift
print(1)
max(1, 2)
func globalFunction() { }

func A() { // Global
  
  func B() { // 이건 Global 아님
    
  }
}
```

2. Nested functions: 이름을 가지며, 감싸고 있는 함수의 값을 캡쳐하는 클로저. 쉽게 말해 함수안에 함수.

```swift
func outsideFunction() -> () -> () { // () -> () 이게 리턴 타입이 함수임을 표시.
  var x = 0
  
  func nestedFunction() { // 중첩함수. Nested
    x += 1    // 그 자신의 함수가 가지지 않은 값을 사용. 이것은 Capture에 의해 가능하다.
    print(x)
  }
  
  return nestedFunction // 함수를 반환.
}
let nestedFunction = outsideFunction()
nestedFunction()
nestedFunction()
nestedFunction()
```

3. Closure: 주변 문맥(Context)의 값을 캡쳐할 수 있으며, 간단한 문법으로 쓰여진 <span style="color: red;">이름 없는</span> 클로저
   1. 기본형

   ```swift
   func aFunction() { // 보통 함수
     print("This is a function.")
   }
   aFunction() //보통 함수 호출
   aFunction()
   
   ({ // 이름이 없다.
     print("This is a closure.")
   })() // 정의와 동시에 호출
   ```

   2. 변수에 할당

   ```swift
   // 클로저를 변수에 담아 이름 부여 가능
   let closure = {
     print("This is a closure.")
   }
   closure()
   closure()
   
   // 함수도 변수로 저장 가능
   func aFunction() { // 보통 함수. 이 함수의 타입은 () -> ()
     print("This is a function.")
   }
   var function = aFunction // 아래의 줄임형
   var function: () -> () = aFunction // function의 타입은 () -> ()
   function()
   
   
   // 같은 타입일 경우 함수나 클로저 관계없이 치환 가능
   function = closure
   function()
   type(of: function)
   type(of: closure)
   ```

### Closure syntax

* 함수를 같은 내용의 클로저로 만드는 법

* 클로저 내에 in 다음이 함수의 내부 code 부분

  ```swift
  // 파라미터 + 반환 타입을 가진 함수
  func funcWithParamAndReturnType(_ param: String) -> String {
    return param + "!"
  }
  print(funcWithParamAndReturnType("function"))
  ```

  방법 1)

  ```swift
  // 파라미터 + 반환 타입을 가진 클로저 1
  // Type Annotation
  let closureWithParamAndReturnType1: (String) -> String = { param in
    return param + "!"
  }
  print(closureWithParamAndReturnType1("closure"))
  // Argument Label은 생략. 함수의 Argument Label을 (_)로 생략한 것과 동일
  ```

  방법 2)

  ```swift
  // 파라미터 + 반환 타입을 가진 클로저 2
  let closureWithParamAndReturnType2 = { (param: String) -> String in
    return param + "!"
  }
  print(closureWithParamAndReturnType2("closure"))
  ```

  방법 3)

  ```swift
  // 파라미터 + 반환 타입을 가진 클로저 3. 타입 추론과 비슷한 느낌.
  // Type Inference
  let closureWithParamAndReturnType3 = { param in
    param + "!" // 이게 달랑 하나 있으니까 이걸 return이라고 생각해서 타입 추론
  }
  print(closureWithParamAndReturnType3("closure"))
  ```

### Syntax optimization (문법 최적화)

* 문맥을 통해 매개변수 및 반환 값에 대한 타입 추론

* 단일 표현식 클로저에서의 반환 키워드 생략

* 축약 인수명

* 후행 클로저 문법

  등을 활용하여 최적화 한다.

  ```swift
  // 입력된 문자열의 개수를 반환하는 클로저를 함수의 파라미터로 전달하는 예
  func performClosure(param: (String) -> Int) {
    param("Swift")
  }
  //param의 타입은 (String) -> Int 이 자체. 따라 param의 타입은 클로저(함수)다. func sth(_ some: String) -> Int 이거와 다른것이다.
  
  performClosure(param: { (str: String) -> Int in
    return str.count
  }) // {}이 부분이 전달인자. 전혀 생략되지 않은 완전체.
  
  performClosure(param: { (str: String) in
    return str.count
  }) // 반환타입이 생략. return에서 타입 추론.
  
  performClosure(param: { str in
    return str.count
  }) // 입력 타입 생략. 이미 함수에 정의되어있음.
  
  performClosure(param: {
    return $0.count
  }) // 매게변수명 생략. 매게변수가 여러개면 $1, $2, ...
  
  performClosure(param: {
    $0.count
  })// return 생략. Statement가 한개만 있을 때
  
  performClosure(param: ) {
    $0.count
  } // {}부분을 param의 값으로 인식
  
  performClosure() {
    $0.count
  } // 마지막 파라미터에 한해서 생략가능
  
  performClosure { $0.count }
   // 최종
  ```

### Inline closure

* 함수의 인수(Argument)로 들어가는 클로저

  ```swift
  func closureParamFunction(closure: () -> Void) {
    closure()
  }
  func printFunction() {
    print("Swift Function!")
  }
  let printClosure = {
    print("Swift Closure!")
  }
  
  closureParamFunction(closure: printFunction) // 위에서 미리 정의된 함수를 매개변수로 넘김
  closureParamFunction(closure: printClosure) // 위에서 미리 정의된 클로저를 매개변수로 넘김
  
  // 인라인 클로저 - 변수나 함수처럼 중간 매개체 없이 사용되는 클로저
  closureParamFunction(closure: {
    print("Inline closure - Explicit closure parameter name")
  })
  ```

### Trailing closure(후행 클로저)

* 마지막 인자의 경우 ()가 닫힌 후 {}안에 작성될 수 있다.

* 매우 긴 내용이 클로저에 사용하면 유용하다.(매우 긴 내용의 클로저를 마지막 인자로 사용하는 것)

  ```swift
  closureParamFunction(closure: {
    print("Inline closure - Explicit closure parameter name")
  }) // Inline 방식
  closureParamFunction() {
    print("Trailing closure - Implicit closure parameter name")
  } // Trailing 방식
  closureParamFunction {
    print("Trailing closure - Implicit closure parameter name")
  } // Trailing 방식. ()까지 생략해벌임.
  
  func multiClosureParams(closure1: () -> Void, closure2: () -> Void) {
    closure1()
    closure2()
  }
  
  multiClosureParams(closure1: {
    print("inline")
  }, closure2: {
    print("inline")
  }) // trailing closure 형식이 아닌것
  
  multiClosureParams(closure1: {
    print("inline")
  }) {
    print("trailing")
  } // closure2가 trailing closure 방식으로 작성됨
  ```

  

