# Access Control

## 정의

* 다른 모듈의 코드 또는 다른 소스 파일 등으로부터 접근을 제한하는 것
* 세부 구현 내용을 숨기고 접근할 수 있는 인터페이스 지정 가능

 ### Module

* import를 통해 다른 모듈로부터 불러들일 수 있는 하나의 코드 배포 단위
* Library, Framework, Application 등

### Source file

* 모듈 내에 포함된 각 .swift 파일

## Access level

* open public internal fileprivate private
  1. private: {}로 구분되는 범위 안에서만 다룸
  2. fileprivate: 해당 .swift 파일에서만
  3. internal: 기본값. 모듈내에서 접근 가능. 생략 가능.
  4. public: 외부 모듈에서 접근 가능
  5. open: 외부 모듈에서 접근 가능. subclass 및 오버라이딩 가능.
* 1 ~ 3: 어플리케이션 개발 단계
* 4 ~ 5: 프레임워크 개발 단계
* Int // ㅇ떤 문서에 public 혹은 open으로 선언돼있기 때문에 내가 가져다가 쓸 수 있는 것이다.

### private

```swift
class SomePrivateClass {
  private var name = "name"
  private var age = 0
  
  func someFunction() {
    print(name)
  }
}
//let somePrivateClass = SomePrivateClass()
//somePrivateClass.someFunction() // 접근가능
//somePrivateClass.name // 접근불가
//somePrivateClass.age // 접근불가
```

### fileprivate

```swift
class SomeFileprivateClass {
  fileprivate var name = "name"
  fileprivate var age = 0
}

let someFileprivateClass = SomeFileprivateClass()
someFileprivateClass.name
someFileprivateClass.age
```

### internal

```swift
class SomeInternalClass {
  internal var name = "name"
  internal var age = 0
}

//class SomeInternalClass {
//  var name = "name"
//  var age = 0
//}


let someInternalClass = SomeInternalClass()
someInternalClass.name
someInternalClass.age
```

### Nested Type

* 클래스의 접근제한레벨에 따른 프로퍼티의 접근제한레벨의 기본 레벨

* Private -> Fileprivate

  Fileprivate -> Fileprivate

  Internal -> Internal

  Public -> Internal

  Open -> Internal

  ```swift
  // 별도로 명시하지 않으면 someProperty는 Internal 레벨
  // public도 동일
  open class OClass {
    var someProperty: Int = 0
  }
  
  // 별도로 명시하지 않으면 someProperty는 Internal 레벨
  // 더 높은 레벨을 설정할 수는 있지만,
  // 애초에 클래스 자체에 접근할 수 있는 레벨이 낮으므로 의미 없음
  internal class IClass {
    open var openProperty = 0
    public var publicProperty = 0
    var someProperty: Int = 0
  }
  let iClass = IClass() // interanl
  iClass.publicProperty // public
  iClass.openProperty //
  // 별도로 명시하지 않으면 someProperty는 fileprivate 레벨
  fileprivate class FClass {
    var someProperty: Int = 0
  }
  
  // 별도로 명시하지 않으면 someProperty는 fileprivate 레벨
  private class PClass {
    var someProperty: Int = 0
  }
  
  fileprivate let pClass = PClass() // PClass 자체가 private 이므로 pClass도 최소 fileprivate으로 해야함
  ```

## Getter and Setter

* get, set에 대해 접근제한레벨 설정 기준

* Getter 와 Setter 는 그것이 속하는 변수, 상수 등에 대해 동일한 접근 레벨을 가짐

   이 때 Getter 와 Setter 에 대해서 **각각 다른 접근 제한자 설정 가능**

  case 1)

  ```swift
  class TrackedString {
    var numberOfEdits = 0
  
    var value: String = "" { // value를 수정할 때 마다 수정횟수 +1
      didSet {
        numberOfEdits += 1
      }
    }
  }
  let trackedString = TrackedString()
  trackedString.numberOfEdits // 0
  trackedString.value = "This string will be tracked."
  trackedString.numberOfEdits // 1
  
  trackedString.value += " This edit will increment numberOfEdits."
  trackedString.numberOfEdits // 2
  
  trackedString.value = "value changed"
  trackedString.numberOfEdits // 3
  
  trackedString.numberOfEdits = 0
  trackedString.numberOfEdits // 0. 이렇게 조작 가능
  ```

  case 2)

  ```swift
  class TrackedString {
    private var numberOfEdits = 0
    
    var value: String = "" {
      didSet {
        numberOfEdits += 1
      }
    }
  }
  let trackedString = TrackedString() // 아래 전부 error. 왜냐, numberOfEdits가 private.
  trackedString.numberOfEdits // 
  trackedString.value = "This string will be tracked."
  trackedString.numberOfEdits // 1
  
  trackedString.value += " This edit will increment numberOfEdits."
  trackedString.numberOfEdits // 2
  
  trackedString.value = "value changed"
  trackedString.numberOfEdits // 3
  ```

  case 3)

  ```swift
  class TrackedString {
    private(set) var numberOfEdits = 0 // set에 대해선 private, get에 대해선 internal
  
    var value: String = "" {
      didSet { // willSet에도 사용 가능
        numberOfEdits += 1
      }
    }
  }
  
  let trackedString = TrackedString()
  trackedString.numberOfEdits // 0
  trackedString.value = "This string will be tracked."
  trackedString.numberOfEdits // 1
  
  trackedString.value += " This edit will increment numberOfEdits."
  trackedString.numberOfEdits // 2
  
  trackedString.value = "value changed"
  trackedString.numberOfEdits // 3
  
  trackedString.numberOfEdits = 0 //Cannot assign to property: 'numberOfEdits' setter is inaccessible
  ```

  