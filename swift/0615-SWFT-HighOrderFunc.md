# High Order Function

# 1. 고차함수

## 1) 정의

하나 이상의 함수를 인자로 갖는 함수

함수를 결과로 반환하는 함수

## 2) 조건

고차함수가 되려면 '1급객체'이어야 한다.

1급객체의 조건

변수나 데이터에 할당 가능해야함

객체의 인자로 넘길 수 있어야 함

객체의 리턴값으로 리턴 가능해야함

예시)

```swift
func firstClassCitizen() {
  print("function call")
}

func function(_ parameter: @escaping ()->()) -> (()->()) {
  return parameter
}

let returnValue = function(firstClassCitizen)
returnValue
returnValue()

// 출력결과
// function call
```

# 2. 고차함수 in Swift

Collection Type(Array, Dictionary, Set)에 사용된다.

## 1) forEach

컬렉션의 각 요소(Element)에 동일 연산을 적용하며, 반환값이 없는 형태

![High%20Order%20Function%20839230f3b7574d1490de9346217d7c7d/_2020-06-15__1.29.05.png](High%20Order%20Function%20839230f3b7574d1490de9346217d7c7d/_2020-06-15__1.29.05.png)

throws: 함수가 실행중에 오류가 발생할 수 있으므로 그거에 대한 처리 코드를 작성하라는 표시

```swift
let immutableArray = [1, 2, 3, 4]

immutableArray.forEach { num in
  print(num, terminator: " ")
}
print()
```

※for vs forEach

반복문에서 쓸 수 있는 제어이전문(continue, break)을 forEach에서는 사용 불가. return 사용 가능.

## 2. map

컬렉션의 각 요소(Element)에 동일 연산을 적용하여, 변형된 새 컬렉션 반환

![High%20Order%20Function%20839230f3b7574d1490de9346217d7c7d/_2020-06-15__1.46.44.png](High%20Order%20Function%20839230f3b7574d1490de9346217d7c7d/_2020-06-15__1.46.44.png)

(Int)는 컬렉션 타입에 따라 바뀜. intArr가 ["a", "b"]이면 (String)으로 나옴. 반환값은 어떤 타입도 될 수 있어서 T.

```swift
let names = ["Chris", "Alex", "Bob", "Barry"]
print(names.map { $0 + "'s name" })

// 출력
//Chris's name
//Alex's name
//Bob's name
//Barry's name
```

## 3. filter

컬렉션의 각 요소를 평가하여 조건을 만족하는 요소만을 새로운 컬렉션으로 반환

```swift
let names = ["Chris", "Alex", "Bob", "Barry"]

let containBNames = names
  .filter { (name) -> Bool in
    return name.contains("B")
  }
print(containBNames)

// 출력
//["Bob", "Barry"]
```

## 4. reduce

컬렉션의 각 요소들을 결합하여 단일 타입으로 반환. e.g. Int, String

```swift
let sum1to100 = (1...100).reduce(0) { (sum: Int, next: Int) in
  return sum + next
}
print(sum1to100)

// 출력
// 5050
```

## 5. flatmap

중첩된 컬렉션을 하나의 컬렉션으로 병합

```swift
let nestedArr: [[Int]] = [[1, 2, 3], [9, 8, 7], [-1, 0, 1]]
print(nestedArr)
print(nestedArr.flatMap { $0 })

// 출력
//[1, 2, 3, 9, 8, 7, -1, 0, 1]
```

## 6. compactMap

컬렉션의 요소중 옵셔널이 있을 경우 제거

```swift
let optionalStringArr = ["A", nil, "B", nil, "C"]
print(optionalStringArr) // [Optional("A"), nil, Optional("B"), nil, Optional("C")]
print(optionalStringArr.compactMap { $0 })

// 출력
//["A", "B", "C"]

let numbers = [-2, -1, 0, 1, 2]
let positiveNumbers = numbers.compactMap { $0 >= 0 ? $0 : nil }
print(positiveNumbers)

// 출력
[0, 1, 2]
```

map vs compactMap

```swift
let array = ["1j", "2d", "3", "4"]

let m1 = array.map({ Int($0) })
let f1 = array.compactMap({ Int($0) })
let m2 = array.map({ Int($0) })[0]
let f2 = array.compactMap({ Int($0) })[0]

// 출력
// m1 = [nil, nil, Optional(3), Optional(4)]
// f1 = [3, 4]
// m2 = nil
// f2 = 3
```

# 3. 고차 함수 내부

map, filter도 내부적으로는 for, while과 같은 반복문으로 만들어져있다.

- map

    ![High%20Order%20Function%20839230f3b7574d1490de9346217d7c7d/_2020-06-15__4.07.29.png](High%20Order%20Function%20839230f3b7574d1490de9346217d7c7d/_2020-06-15__4.07.29.png)

- filter

    ![High%20Order%20Function%20839230f3b7574d1490de9346217d7c7d/_2020-06-15__4.07.54.png](High%20Order%20Function%20839230f3b7574d1490de9346217d7c7d/_2020-06-15__4.07.54.png)