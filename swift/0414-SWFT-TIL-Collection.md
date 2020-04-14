# Collection

* 데이터의 모음
  1. <span style="color: red;">Array</span>: 순서가 있다.
  2. Set: 순서가 없다. 하지만 유니크한 값들이어야한다.
  3. <span style="color: red;">Dictionary</span>: 순서가 없다. key-value 쌍이 하나의 값이다.

## Array

* 순서가 있는 콜렉션

* 0 베이스 index: a[0], a[1], ...

* var, let 데이터 타입 똑같이 적용

* 타입 추론 적용

  ex) var arrayFromLiteral = [1, 2, 3] -> 각 배열요소는 Int로 전체는 배열로 타입 추론됨

  ex) let emptyArray = [ ] -> 배열안의 타입 추론 불가하므로 error

  ex) let emptyArray: [String] = [ ] -> 가능

### 초기화 방법

1. Annotation

   ```swift
   let strArray1: Array<String> = ["apple", "orange", "melon"]
   let strArray2: [String] = ["apple", "orange", "melon"]
   ```

2. Inference

   ```swift
   let strArray3 = ["apple", "orange", "melon"]
   let strArray4 = Array<String>(repeating: "iOS", count: 5)
   ```

3. error(단, Any 타입으로 하면 가능)

   ```swift
   let strArray5 = ["apple", 3.14, 1] // 여러가지 타입이 혼용된 배열은 불가
   ```

### 배열 요소 갯수

```swift
let fruits = ["Apple", "Orange", "Banana"]
let countOfFruits = fruits.count // 요소 갯수 반환

if !fruits.isEmpty {// '해당 배열이 비어있으면' 이라는 조건을 나타냄
  print("\(countOfFruits) element(s)")
} else {
  print("empty array")
}
```

### Retrieve an Element(개별 요소 접근)

```swift
//              0        1         2          3
// fruits = ["Apple", "Orange", "Banana", endIndex]

fruits[0]
fruits[2]
//fruits[123]

fruits.startIndex
fruits.endIndex
type(of: fruits.endIndex) //fruits.starIndex/endIndex의 데이터 타입은 Int이다.

fruits[fruits.startIndex]
//fruits[fruits.endIndex] 이건 error다. 왜냐, 실제 데이터는 fruits[2]까지만 있다.
fruits[fruits.endIndex - 1]

fruits.startIndex == 0
fruits.endIndex - 1 == 2
```

### Searching

다양한 메소드를 사용가능

```swift
let alphabet = ["A", "B", "C", "D", "E"]

if alphabet.contains("A") {
  print("contains A")
}

if alphabet.contains(where: { str -> Bool in
  // code...
  return str == "A"
}) {
  print("contains A")
}

if let index = alphabet.firstIndex(of: "Q") { //옵셔널. firstIndex에 Q라는 값이 있다면 거기의 인덱스를 반환
  print("index of D: \(index)")
} else {
  print("No index")
}
```

### Add a new element(배열 추가)

```swift
var alphabetArray = ["A"]
alphabetArray.append("B") // 맨 뒤에 추가
alphabetArray += ["C"] // 그 뒤에 추가
alphabetArray

var alphabetArray2 = ["Q", "W", "E"]
alphabetArray + alphabetArray2 // [A, B, C, Q, W, E]

//alphabetArray.append(5.0) // 5.0이 Double이라 error
//alphabetArray + 1 // 배열에 정수를 더해서 error

alphabetArray.insert("S", at: 0) // 0번째 index에 삽입
alphabetArray.insert("F", at: 3) // 3번째 index에 삽입
alphabetArray
```

### Change an existing element

```swift
alphabetArray = ["A", "B", "C"]
alphabetArray[0] = "Z"
alphabetArray // [Z, B, C]

1...5
1..<5
1...

alphabetArray = ["A", "B", "C", "D", "E", "F"]
alphabetArray[2...] // [C, D, E, F]
alphabetArray[2...] = ["Q", "W", "E"]
alphabetArray // [A, B, Q, W, E] F는 없어짐
```

### 요소 제거

```swift
alphabetArray = ["A", "B", "C", "D", "E"]

let removed = alphabetArray.remove(at: 0)
alphabetArray // [B, C, D, E]
alphabetArray.removeAll()


// index 찾아 지우기
alphabetArray = ["A", "B", "C", "D", "E"]
if let indexC = alphabetArray.firstIndex(of: "C") {
  alphabetArray.remove(at: indexC)
}
alphabetArray // [A, B, D, E]
```

### Sorting

```swift
alphabetArray = ["A", "B", "C", "D", "E"]
alphabetArray.shuffle() // [E, D, C, A, B]

alphabetArray.sort() // [A, B, C, D, E]. 위에서 shuffle해놓은거를 다시 sort
alphabetArray // [A, B, C, D, E]

// shuffle vs shuffled
// shuffle은 반환 타입이 void, shuffeld는 String
// shuffle은 자기 자신이 변경되어 반환되지만 shuffled는 자신은 그대로 있고 변경된 내용을 String에 담아서 반환
// sorted vs sort
// shuffle vs shuffled와 같은 개념

//func sorted() -> [Element]
//mutating func sort()

alphabetArray.shuffle()

var sortedArray = alphabetArray.sorted()
sortedArray
alphabetArray


// sort by closure syntax

sortedArray = alphabetArray.sorted { $0 > $1 }
alphabetArray.sorted(by: >= )
sortedArray
```

### Enumerating an array(배열의 인덱스와 내용을 함께 알고 싶을 때)

```swift
let array = ["Apple", "Orange", "Melon"]

for value in array {
  if let index = array.firstIndex(of: value) {
    print("\(index) - \(value)")
  }
}

for tuple in array.enumerated() {
  print("\(tuple.0) - \(tuple.1)")
//  print("\(tuple.offset) - \(tuple.element)")
} // 이렇게 잘 안쓰고 아래와 같이 decomposition해서 쓴다.

for (index, element) in array.enumerated() {
  print("\(index) - \(element)")
}

for (index, element) in array.reversed().enumerated() {
  print("\(index) - \(element)")
} // 위에꺼에서 element와 index를 바꿔서
```

## Dictionary

* Unique Key + Value (값은 중복이 가능하지만, 키는 유일해야한다.)

* _**key 끼리 같은 타입, value 끼리 같은 타입 이어야 한다.**_

  ```swift
  //var dictFromLiteral = ["key 1": "value 1", "key 2": "value 2"]
  //var dictFromLiteral = [1: "value 1", 2: "value 2"]
  //var dictFromLiteral = ["1": 1, "2": 2]
  //dictFromLiteral = [:]
  
  // 오류
  //var dictFromLiteral = [:]
  ```

### Dictionary type:  타입 추론 가능하다.

```swift
let words1: Dictionary<String, String> = ["A": "Apple", "B": "Banana", "C": "City"]
let words2: [String: String] = ["A": "Apple", "B": "Banana", "C": "City"]
let words3 = ["A": "Apple", "B": "Banana", "C": "City"]
```

### 요소 갯수

```swift
var words = ["A": "Apple", "B": "Banana", "C": "City"]
let countOfWords = words.count // key와 value는 하나의 쌍이기 때문에 3이 반환됨

if !words.isEmpty {
print("\(countOfWords) element(s)")
} else {
print("empty dictionary")
}
```

### 요소 접근

key로 접근한다.

```swift
words["A"]
words["Q"]

if let aValue = words["A"] {
  print(aValue)
} else {
  print("Not found")
}

if let zValue = words["Z"] {
  print(zValue)
} else {
  print("Not found")
}

print(words.keys) //key 만을 배열로 뽑음
print(words.values) // value 만을 배열로 뽑음

let keys = Array(words.keys)
let values = Array(words.values)

print(keys)
print(values)
```

### Enumerating an dictionary

* 배열의 그것에서 확장된 느낌?

```swift
let dict = ["A": "Apple", "B": "Banana", "C": "City"]

for (key, value) in dict {
  print("\(key): \(value)")
}

for (key, _) in dict {
  print("Key :", key)
}

for (_, value) in dict {
  print("Value :", value)
}

for value in dict.values {
  print("Value :", value)
}
```

### Searching

```swift
//var words = ["A": "Apple", "B": "Banana", "C": "City"]

for (key, _) in words {
  if key == "A" {
    print("contains A key.")
  }
}

if words.contains(where: { (key, value) -> Bool in
    return key == "A" // 키가 A인걸 찾으면 true
  }) {
  print("contains A key.")
}
```

### 새 요소 추가

```swift
words = ["A": "A"]

words["A"]    // Key -> Unique

words["A"] = "Apple"
words

words["A"] = "Melon"
words // 같은 키에 새로운 값을 할당하면 덮어쓰기 되고

words["B"] = "Banana"
words // 새로운 키에 값을 할당하면 추가됨

words["C"] = "City"
words
```

### 존 요소 변경

```swift
words = [:]
words["A"] = "Application"
words // ["A": "Application"]

words["A"] = "App"
words // ["A": "App"]


// 키가 없으면 데이터 추가 후 nil 반환,
// 키가 이미 있으면 데이터 업데이트 후 oldValue 반환

if let oldValue = words.updateValue("Apple", forKey: "A") {
  print("\(oldValue) => \(words["A"]!)")
} else {
  print("Insert \(words["A"]!)")
}
words // ["A": "Apple"]

```

### 요소 제거

```swift
words = ["A": "Apple", "I": "IPhone", "S": "Steve Jobs", "T": "Timothy Cook"]
words["S"] = nil
words["Z"] = nil // 존재하지 않는 요소에 접근하려해도 error 발생하지 않는다.
words // ["A": "Apple", "I": "IPhone", "T": "Timothy Cook"]

// 지우려는 키가 존재하면 데이터를 지운 후 지운 데이터 반환, 없으면 nil
if let removedValue = words.removeValue(forKey: "T") {
  print("\(removedValue) removed!")
}
words // ["A": "Apple", "I": "Iphone"]

words.removeAll()

```

### Nested

Dictionary의 중첩?

```swift
var dict1 = [String: [String]]() // key는 (문자열) 타입, value는 (String 배열) 타입
//dict1["arr"] = "A"

dict1["arr1"] = ["A", "B", "C"]
dict1["arr2"] = ["D", "E", "F"]
dict1

var dict2 = [String: [String: String]]() // key는 문자열 타입, value는 dictionary 타입
dict2["user1"] = [
  "name": "나개발",
  "job": "iOS 개발자",
  "hobby": "코딩",
]
dict2["user2"] = [
  "name": "나코딩",
  "job": "Android 개발자",
  "hobby": "코딩",
]
dict2

// 예
[
  "name": "나개발",
  "job": "iOS 개발자",
  "age": 20,
  "hobby": "코딩",
  "apps": [
    "이런 앱",
    "저런 앱",
    "잘된 앱",
    "망한 앱",
  ],
  "teamMember": [
    "designer": "김철수",
    "marketer": "홍길동",
  ]
] as [String: Any]
```

## Set

* 순서 없는 콜렉션

  ```swift
  let fruitsSet: Set<String> = ["Apple", "Orange", "Melon"]
  let numbers: Set = [1, 2, 3, 3, 3]
  let emptySet = Set<String>()
  ```

  

* 유니크 값

* 배열과 딕셔너리에 비해 덜 중요함

* Number of element

  ```swift
  fruitsSet.count // 3
  
  if !fruitsSet.isEmpty {
    print("\(fruitsSet.count) element(s)")
  } else {
    print("empty set")
  }
  ```

* Searching

  ```swift
  let x: Set = [1, 2, 3, 4, 5]
  let y: Set = [1, 2, 3, 4, 5]
  x
  y
  
  //fruitsSet[0]    // error 발생. 왜냐, 순서 X -> Index X
  x.first   // set에서 요소를 접근하는 방법
  
  
  if fruitsSet.contains("Apple") {
    print("Contains Apple") // Contains Apple 출력
  }
  
  
  let productSet: Set = ["iPhone", "iPad", "Mac Pro", "iPad Pro", "Macbook Pro"]
  
  for element in productSet {
    if element.hasPrefix("i") { //i로 시작하는 요소 검색
      print(element) // 해당 요소 출력. 
    }
  }
  ```

* Add a new element

  ```swift
  // [1,2,3].append(1) // append는 뒤로 추가
  // [1,2,3].insert(2, at: 0) //insert는 앞으로 추가
  // set은 순서가 없으므로 insert로만 쓴다
  var stringSet: Set<String> = []
  stringSet.insert("Apple")
  stringSet
  
  stringSet.insert("Orange")
  stringSet
  
  stringSet.insert("Orange")
  stringSet // 중복되는 값을 추가하는 것은 무시됨. 따라서 {"Orange", "Apple"} 
            // 만약 배열이었으면 ["Orange", "Orange", "Apple"]
  
  
  ```

* Remove element

  ```swift
  stringSet = ["Apple", "Orange", "Melon"]
  
  stringSet.remove("Apple")
  stringSet // {"Orange", "Melon"}
  
  if let removed = stringSet.remove("Orange") {  // index가 없으므로 이렇게 지운다
    print("\(removed) has been removed!")
  }
  
  stringSet // {"Melon"}
  
  stringSet.removeAll()
  ```

* Compare two sets

  ```swift
  var favoriteFruits = Set(["Apple", "Orange", "Melon"])
  var tropicalFruits = Set(["Banana", "Papaya", "Kiwi", "Pineapple"])
  
  if favoriteFruits == tropicalFruits {
    print("favoriteFruits == tropicalFruits")
  } else {
    print("favoriteFruits != tropicalFruits")
  }
  
  var favoriteFruits1 = Set(["Orange", "Melon", "Apple"])
  var favoriteFruits2 = Set(["Apple", "Melon", "Orange"])
  
  favoriteFruits1 == favoriteFruits2 // set은 순서가 없다. 따라서 내용이 같으므로 true.
  favoriteFruits1.elementsEqual(favoriteFruits2) // 굳이 순서까지 따지려면 이렇게 하면 된다.
  
  
  // 포함 관계 여부. 상위/하위 집합.
  // group1 ⊇ group2
  let group1: Set = ["A", "B", "C"]
  let group2: Set = ["A", "B"]
  
  // superset
  group1.isSuperset(of: group2)
  group1.isStrictSuperset(of: group2)
  
  // strictSuperset - 자기 요소 외 추가 요소가 최소 하나 이상 존재해야 함
  group1.isSuperset(of: group1)
  group1.isStrictSuperset(of: group1)
  
  // subset도 같은 방식
  group2.isSubset(of: group1)
  group2.isStrictSubset(of: group1)
  group2.isStrictSubset(of: group2)
  ```

* Intersection (교집합)

  ```swift
  favoriteFruits = Set(["Apple", "Orange", "Melon", "Kiwi"])
  tropicalFruits = Set(["Banana", "Papaya", "Kiwi", "Pineapple"])
  
  // 교집합에 해당하는 요소를 반환
  let commonSet = favoriteFruits.intersection(tropicalFruits) // intersection은 새로운 set을 반환. 반환 타입을 잘 확인해야함.
  commonSet // {"Kiwi"}
  
  // 교집합에 해당하는 요소만 남기고 나머지 제거
  tropicalFruits.formIntersection(favoriteFruits) // formIntersection은 set 자기 자신을 변경
  tropicalFruits // {"Kiwi"}
  ```

* SymmentricDifference (교집합의 여집합)

  ```swift
  favoriteFruits = Set(["Apple", "Orange", "Melon", "Kiwi"])
  tropicalFruits = Set(["Banana", "Papaya", "Kiwi", "Pineapple"])
  
  // 교집합의 여집합 요소들을 반환
  let exclusiveSet = favoriteFruits.symmetricDifference(tropicalFruits)
  exclusiveSet // {"Melon", "Banana", "Orange", "Pineapple", "Papaya", "Apple"}
  ```

* Union (합집합)

  ```swift
  favoriteFruits = Set(["Apple", "Orange", "Melon", "Kiwi"])
  tropicalFruits = Set(["Banana", "Papaya", "Kiwi", "Pineapple"])
  
  // 합집합 반환
  var unionSet = favoriteFruits.union(tropicalFruits)
  unionSet // {"Orange", "Pineapple", "Papaya", "Apple", "Kiwi", "Banana", "Melon"}
  ```

* Subtracting

  ```swift
  favoriteFruits = Set(["Apple", "Orange", "Melon", "Kiwi"])
  tropicalFruits = Set(["Banana", "Papaya", "Kiwi", "Pineapple"])
  
  // 차집합 반환
  let uncommonSet = favoriteFruits.subtracting(tropicalFruits)
  uncommonSet // {"Orange", "Apple", "Melon"}
  ```

  

