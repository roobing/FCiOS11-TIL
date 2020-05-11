# Memory Management

<br></br>

## ARC

* 이전 방식
  1. GC(Garbage Collector)
     * 단점: 런타임에 돈다(일시적 성능저하 가능)
  2. MRR(Manual Retain Releaser/MRC(Manual Referece Counting)
     * objective-c에서 사용
     * RC(Reference Counting)을 통해 메모리를 수동으로 관리
     * retain / release / autorelease 등의 메모리 관리 코드를 직접 호출
     * 개발자가 명시적으로 rc를 증가시키고 감소시키는 작업수행
     * 손이 많이 간다

#

예시)

```objective-c
int main(int argc, const char * argvp[]) {
  Person *man = [[Person alloc]init]; // count 1
  [man doSomething]; // count 1

  [man retain]; // 메모리 추가 count 2
  [man doSomething]; // count 2
  
  [man release]; // count 1
  [man release]; // count 0
}
```

<br>

Memory Lack / Dangling Pointer

카운트 할당(alloc, retain)과 해제(release)가 균형이 맞아야한다.

할당이 많으면 => memory lack 메모리 누수

해제가 많으면 => dangling pointer 허상(비어있거나 이상한 값이 들어있음) 발생

메모리 카운트가 0이 되어야 메모리에서 삭제

***

메모리관리의 어려움

xcode static analyzer 같은 툴이 있었음(objective-c)

#

두둥등장 ARC

RC 자동 관리 방식

컴파일러가 자동으로 retain / release를 코드에 삽입

런타임이 아닌 컴파일 단에서 처리(Heap 스캔 불필요/성능 저하 없음)

#

ARC(Automatic Referece Counting). 

클래스 인스턴스에만 적용(즉 참조타입에만 적용됨)

참조타입 종류

1. strong: default. 참조될 때마다 참조카운트 1증가

weak, unowned: 참조 카운트 증가시키지 않음

강한 순환참조 주의 필요



```swift
class Point {
  //var refCount: Int
  var x, y: Double
  func draw() {
    //...
  }
}
```



```swift
stack
point1:
point2:
```



실제 런타임에서 stack 공간과 heap 공간이 할당된다.



```swift
presetn(secondVC) // count 2. presentedViewController에 참조됨
} // count 1

SecondVC{
  //...
  dismiss() // count 0
}
```

<br></br>

### ARC in Struct

stack에 바로 올라간다.

<br></br>

### Strong Reference Cycle

객체 연결이 모두 끊어졌지만 순환참조에 의해 카운트가 살아있어서 메모리 누수 발생=>성능저하

<br></br>

두개의 참조가 되는 객체에 걸려있는 참조 중 하나를 weak로

<br></br>

### ARC basic

면접에서 100% 물어본다

playground 참고

<br></br>

### Scope

pg

<br></br>

### Strong Reference Cycles

pg

<br></br>

### Strong, Weak, Unowned Reference Cycles Relastionship

pg

