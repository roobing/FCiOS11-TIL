# Struct

## Struct vs Class

### 공통점

1. 값을. 저장하기 위한 프로퍼티
2. 기능을 제공하기 위한 메서드
3. 초기 상태를 설정하기 위한 생성자
4. 기본 구현에서 기능을 추가하기 위한 확장(Extension)
5. 특정 값에 접근할 수 있는 첨자(Subscript)
6. 특정한 기능을 수행하기 위한 프로토콜 채택
7. Upper Camel Case 사용
```swift
class SomeClass {
  var someProperty = 1
  func someMethod() {}
}
struct SomeStruct {
  var someProperty = 1
  func someMethod() {}
}

let someClass = SomeClass()
let someStruct = SomeStruct()
```
<br>

### Class만의 특성

1. 상속 (Inheritance)
2. 소멸자 (Deinitializer). deinit 메소드는 heap에서 해제될때만 호출되는 메소드 이므로.
3. 참조 카운트 (Reference counting)
```swift
// Struct는 제공하지 않는다

// 상속
struct ParentS {}
//struct Child: Parent {}   // 오류

// 소멸자
struct Deinit {
//  deinit { }    // 오류
}

// 참조 카운트(Reference Counting)  X
```
<br>

### 참조 타입 vs 값 타입

```swift
class Dog {
  var name = "토리"
}
struct Cat {
  var name = "릴리"
}

let dog = Dog()
let cat = Cat()

//dog.name = "릴리" // 인스턴스 dog가 가리키는 주소상의 .name을 변경하는 것이므로 let, var 상관없음. 하지만 .name이 let이면 .name 또한 수정 불가능함
//cat.name = "토리" // struct cat이 let(상수)로 선언됨. stack에 올라가는 데이터


let dog1 = dog
var cat1 = cat
dog1.name = "뽀삐"
cat1.name = "뽀삐"
dog.name
cat.name

// 참조 비교 연산자(heap을 참고하는 연산자)
//dog === dog1 // true. 참조하는 Class가 동일하다(주소가 같다)
//cat === cat1 // struct는 heap에 없다.
```
<br>

### 생성자 비교

#### 1. var로 선언된 변수

```swift
class UserClass1 {
  var name = "홍길동"
}
struct UserStruct1 {
  var name = "홍길동"
}

let userC1 = UserClass1()
let userS1_1 = UserStruct1()
let userS1_2 = UserStruct1(name: "깃봇") // Struct는 기본 생성자를 제공. Class는 init() 메소드를 따로 만들어야함.
userS1_1.name
userS1_2.name
```
<br>

#### 2. 프로퍼티에 초기값이 없을 때

```swift
class UserClass2 {
  var name: String
  // 초기화 메서드 없으면 오류
  init(name: String) { self.name = name }
}

struct UserStruct2 {
  var name: String
  var age: Int
  
  // 초기화 메서드 자동 생성
  // 단, 생성자를 별도로 구현했을 경우 자동 생성하지 않음
//  init(name: String) {
//    self.name = name
//    self.age = 10
//  }
}

let userC2 = UserClass2(name: "홍길동")
let userS2 = UserStruct2(name: "홍길동", age: 10)
```
<br>

#### 3. 저장 프로퍼티 중 일부만 값이 같을 때

```swift
class UserClass3 {
  let name: String = "홍길동"
  // 저장 프로퍼티 중 하나라도 초기화 값이 없는 경우 생성자를 구현해야 함
//  let age: Int
}

struct UserStruct3 {
  let name: String = "홍길동"
  let age: Int
}
// 초기화 값이 없는 저장 프로퍼티에 대해서만 생성자로 전달
let userS3 = UserStruct3(age: 10)
```
<br>

#### 4. 지정(Designated) 생성자와 편의(Convenience) 생성자

```swift
class UserClass4 {
  let name: String
  let age: Int
  init(name: String, age: Int) {
    self.name = name
    self.age = age
  }
  convenience init(age: Int) { // 일부를 직접 초기화(name)하고 나머지(age)는 생성자(init())를 호출하여 초기화
    self.init(name: "홍길동", age: age)
  }
}

struct UserStruct4 {
  let name: String
  let age: Int
  init(name: String, age: Int) {
    self.name = name
    self.age = age
  }
  
  // Convenience 키워드 사용 X, 지정과 편의 생성자 별도 구분 없음
//  convenience init(age: Int) {
  init(age: Int) {
    self.init(name: "홍길동", age: age)
  }
}

// 따라서 extension에서도 생성자 추가 가능
extension UserStruct4 {
  init(name: String) {
    self.name = name
    self.age = 10
  }
}
```
<br>

### Extension

* 상속-수직확장(상위에서 기능 물려받음)
* 확장-수평확장(자기 자신에게 기능 추가)
```swift
var someNumber = 1

extension Int {
    func addOne() -> Int {
        return self + 1
    }
}

someNumber.addOne()

class Dog1 {
    var name = "a"
}

extension Dog1 {
    func bark() {
        print("lala")
    }
}
```
<br>

### 프로퍼티 변경

1. Struct 특성

```swift
struct PointStruct {
  var x = 0
  
  func updateX() {
    self.x = 5
  } // error. 본인의 프로퍼티를 수정하려면 mutating을 붙여줘야한다.

  var updateProperty: Int {
    get { x }
    set { x = newValue }    // 연산 프로퍼티의 setter는 기본적으로 mutating
  }
}


var p2 = PointStruct()
p2.updateX() // error. 본인의 프로퍼티를 수정하려면 func updateX 함수에 mutating을 붙여줘야한다.
p2.updateProperty = 3 
p2.updateProperty // 3
```
2. Class 특성
```swift
class PointClass {
  var x = 0
  
  func updateX() { //  mutating 필요없다.
    self.x = 5
  }
}

let p1 = PointClass()
p1.updateX() //  mutating 필요없다.
p1.x // 5
```
<br>

### 애플 권장 사항

* 구조체를 기본으로 사용(swift는 구조체를 더 선호. SwiftUI는 구조체로 기반.)
* 클래스를 사용해야 할 때
  1. Objective-C와 호환성이 필요할 때
  2. equality(동등성) 외에 identity(정체성, 동일성)를 제어해야 할 때
  3. RC(Reference Counting)와 소멸자(deinitialization)가 필요할 때
  4. 값이 중앙에서 관리되고 공유되어야 할 때
<br>

### 기타

* heap 영역에 올라가는 데이터 특징
  1. RC를 관리해야한다.
  2. 값을 공유한다.
  3. 동시다발적인 멀티 쓰레딩 작업시, 여러군데에서 값 참조가 들어오므로 관련된 로직 동작으로 인해 속도적으로 stack을 쓰는것보다 느리다.
<br>
* copy on writing
  * 구조체가 크기가 커지면 데이터를 넘길때 느려지는것을 방지한다. 실제 호출될때만 값을 복사한다.

