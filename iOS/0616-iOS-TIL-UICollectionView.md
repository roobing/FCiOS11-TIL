# UICollectionView

# 1. 개념

UITableView랑 비슷한 컨셉

TableView는 위에서 아래 방향으로 하나의 행에 하나의 셀만 포함한다.

CollectionView는 가로/세로 방향으로 하나의 행에 여러가지 셀을 포함하는 등 다양한 방법으로 셀 생성한다.

## 1) 비교

Collection View

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation124.png" alt="0616-iOS-TIL-UIViewAnimation124" style="zoom:50%;" />

Table View

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation125.png" alt="0616-iOS-TIL-UIViewAnimation125" style="zoom:50%;" />

## 2) 유연성 및 자유도

CollectionView > TableView

# 2. Merging Content and Layout

자유도가 높은데신 레이아웃을 어떻게 잡을지 명확하게 정해야함

### 1) 필수요소

Layout, DataSource ⇒ Main!

DelegateFlowlayout ⇒ Option

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation101.png" alt="0616-iOS-TIL-UIViewAnimation101" style="zoom:50%;" />

### 2) 세가지 타입

(1) 셀

(2) 보조뷰

(3) 장식뷰

## 레이아웃

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation102.png" alt="0616-iOS-TIL-UIViewAnimation102" style="zoom:50%;" />

Flow Layout 아니면 모두 Custom Layout

### 1) Flow Layout

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation103.png" alt="0616-iOS-TIL-UIViewAnimation103" style="zoom:50%;" />

### 2) Custom Layout(section layout)

`itemSize`: 셀의 크기

`minimumInteritemSpacing`: 셀간 간격

`minimumLineSpacing`: 라인간의 간격

`sectionInset`: 외부여백

프로퍼티 속성으로 설정 가능하고 델리게이트로도 가능!

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation104.png" alt="0616-iOS-TIL-UIViewAnimation104" style="zoom:50%;" />

(1) itemSize

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation105.png" alt="0616-iOS-TIL-UIViewAnimation105" style="zoom:50%;" />

(2) sectionInset

상하좌우 여백. CSS의 padding느낌

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation119.png" alt="0616-iOS-TIL-UIViewAnimation119" style="zoom:50%;" />

(3) lineSpacing

minimumLineSpacing: 셀마다 크기가 다를때, 즉 셀간의 간격이 다를때. 최소한의 간격을 지정해주는 것

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation106.png" alt="0616-iOS-TIL-UIViewAnimation106" style="zoom:50%;" />

예시

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation107.png" alt="0616-iOS-TIL-UIViewAnimation107" style="zoom:50%;" />

(4) InteritemSpacing

lineSpacing과 같은 개념이지만 셀 사이의 가로간격

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation120.png" alt="0616-iOS-TIL-UIViewAnimation120" style="zoom:50%;" />

(5) Header / Footer Size

셀마다 다르게 할거 아니면 델리게이트 쓰지 않고 프로퍼티로 설정한다.

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation103.png" alt="0616-iOS-TIL-UIViewAnimation103" style="zoom:50%;" />

(6) Layout Matrics

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation122.png" alt="0616-iOS-TIL-UIViewAnimation122" style="zoom:50%;" />

(7) Cell Hierarchy

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation108.png" alt="0616-iOS-TIL-UIViewAnimation108" style="zoom:50%;" />

CustomView(ImageView, Label 등)은 ContentView 위에 add해야 한다.

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation113.png" alt="0616-iOS-TIL-UIViewAnimation113" style="zoom:50%;" />

(8) 셀의 상태

터치에 대해 셀이 가지는 상태

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation109.png" alt="0616-iOS-TIL-UIViewAnimation109" style="zoom:50%;" />

## 3. 스토리보드를 이용한 셀 설정

셀 추가

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation117.png" alt="0616-iOS-TIL-UIViewAnimation117" style="zoom:50%;" />

셀 아이디 정의

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation118.png" alt="0616-iOS-TIL-UIViewAnimation118" style="zoom:50%;" />

셀의 크기를 정확하게 바꾸러면

컬렉션뷰에서 셀 사이즈에서 바꿔줘야 제대로 적용된다. 컬렉션뷰 플로우 레이아웃에서도 가능.

![0616-iOS-TIL-UIViewAnimation110](/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation110.png)

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation111.png" alt="0616-iOS-TIL-UIViewAnimation111" style="zoom:50%;" />

셀 레이아웃 잡기

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation112.png" alt="0616-iOS-TIL-UIViewAnimation112" style="zoom:50%;" />

섹션 인셋 적용

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation116.png" alt="0616-iOS-TIL-UIViewAnimation116" style="zoom:50%;" />

버티컬 vs 호라이젠탈 정렬

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation114.png" alt="0616-iOS-TIL-UIViewAnimation114" style="zoom:50%;" />

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation115.png" alt="0616-iOS-TIL-UIViewAnimation115" style="zoom:50%;" />

## 4. 코드를 이용한 셀 설정

스택뷰

<img src="/Users/woobincheon/Documents/fcios11/03-TIL/iOS/iOS-TIL-Images/0616-iOS-TIL-UIViewAnimation123.png" alt="0616-iOS-TIL-UIViewAnimation123" style="zoom:50%;" />

### Collection View Configuration

필수요소

1. Layout, CollectionView 인스턴스 생성 및 addSubView
2. AutoLayout

```swift
func setupCollectionView() {
    
    // collection view layout 설정
    let layout = UICollectionViewFlowLayout() // CollectionView 생성에 필수!!
    layout.itemSize = CGSize(width: 60, height: 60)   // 셀 사이즈. 기본값 (50, 50)
    layout.minimumInteritemSpacing = 10  // 셀 간 간격. 기본값 10
    layout.minimumLineSpacing = 20   // 행 사이 간격. 기본값 10
    layout.sectionInset = UIEdgeInsets(top: 5, left: 5, bottom: 5, right: 5)  // 외부여백. 기본값 0
		layout.scrollDirection = .vertical // 스크롤 방향 설정(vertical or horizontal)
    
    // collection view configuration
    collectionView = UICollectionView(frame: .zero, collectionViewLayout: layout) // CollectionView 초기화 및 인스턴스 생성(layout 인자 필수)
    collectionView.register(UICollectionViewCell.self, forCellWithReuseIdentifier: "Cell") // 생성할 셀 등록
    collectionView.dataSource = self // 잊지 말기
    collectionView.backgroundColor = .systemBackground // 없으면 걍 까만화면
    view.addSubview(collectionView)
    
    // collection view 오토레이아웃
    collectionView.translatesAutoresizingMaskIntoConstraints.toggle()
    NSLayoutConstraint.activate([
      collectionView.topAnchor.constraint(equalTo: controllerStackView.bottomAnchor, constant: 10),
      collectionView.leadingAnchor.constraint(equalTo: view.leadingAnchor),
      collectionView.bottomAnchor.constraint(equalTo: view.bottomAnchor),
      collectionView.trailingAnchor.constraint(equalTo: view.trailingAnchor),
    ])
  }
```

### Collection View DataSource/Delegate

DataSource

1. `numberOfItemsInSection`
2. `cellForItemAt`

```swift
extension BasicCodeViewController: UICollectionViewDataSource {
	// 셀의 개수  
	func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
    return itemCount
  }
  
	// 셀 생성
  func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
    let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "Cell", for: indexPath)
    cell.backgroundColor = [.red, .green, .blue, .magenta].randomElement()
    return cell
  }
}
```

Delegate

1. `willDisplay`
2. `sizeForItemAt`

```swift
extension CustomCellViewController: UICollectionViewDelegateFlowLayout {
	// 서서히 생기는 애니메이션
  func collectionView(_ collectionView: UICollectionView, willDisplay cell: UICollectionViewCell, forItemAt indexPath: IndexPath) {
    cell.alpha = 0
    cell.transform = .init(scaleX: 0.3, y: 0.3)
    
    UIView.animate(withDuration: 0.3) {
      cell.alpha = 1
      cell.transform = .identity
    }
  }
    
	// 특정 셀의 크기를 2배로 만드는 기능
  func collectionView(
    _ collectionView: UICollectionView,
    layout collectionViewLayout: UICollectionViewLayout,
    sizeForItemAt indexPath: IndexPath
  ) -> CGSize {
    let flowLayout = collectionViewLayout as! UICollectionViewFlowLayout
    
    if indexPath.item % 5 == 2 {
      return flowLayout.itemSize.applying(CGAffineTransform(scaleX: 2, y: 2))
    } else {
      return flowLayout.itemSize
    }
  }
}
```