# xcode

* swift, object-c IDE (기타 언어도 사용은 가능하지만 비추천)
* 잘 만들어진 IDE는 아님
* new project
  * 오거나이제이션 네임: 저작권 표시에 사용되는 이름
  * 오거나이제이션 아이디: 관습적으로 url을 거꾸로 쓴다
  * 번들 아이디: 오거나이제이션 아이디 + 프로덕트 네임
* 최신버전에 맞춘 코드를 하위호환 시키려면 최신버전코드에 @available(iOS 최신버전넘버, *)을 선언해줘야함
* 현업에서는 최신에서부터 2버전 하위 혹은 3버전 하위까지 대응
* 앱델리게이트에서 상태변화메서드들을 관리한다. 12버전까지
* 씬델리게이트에서 상태변화메서드들을 관리한다. 13버전부터
* willterminate는 앱델리게이트에서 관리

# App Life Cycle

## The structure of an App 

모델: 앱에서 표현해야하는 모든 데이터

ex) 펫앱: 판매상품 가격, 강아지 종류, ...

view 컨트롤러: 화면상에서 보여지는 모든 작업들을 조절

모델에 있는 데이터를 언제 어떻게 뷰에 올릴건지 컨트롤러(앱델리게이트)가 제어

뷰에서 일어나는 작업을 언제 어떻게 모델에 전달할지 컨트롤러(앱델리게이트)가 제어

이벤트 루프: 

## The main run loop

컨트롤러의 이벤트 루프

입력을 이벤트큐에 쌓으면서 하나 씩 메인루프에 올려서 돌린다.

이벤트 종류: 터치, 흔들기, 가속도센서, 위치, ...

## state changes in an iOS app

앱이 실행이 되면 포어그라운드(사용자가 보는 영역)에서 인엑티브에서 엑티브로

인엑티브에서 백그라운드로(내가 아는 그 백그라운드)

백그라운드에서 작업이 없으면 서스팬디드(메모리에만 올려져있고 동작 안함)

메모리를 빼야하면 서스펜디드에서 낫러닝

## Execution states for apps

상태들을 옮겨다닐 때 호출되는 메서드(함수)들

초기설정

```swift
   func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        // Override point for customization after application launch.
        return true
    }
```

포어그라운드

```swift
    func applicationWillEnterForeground(_ application: UIApplication) {
        // foreground 되었을 때
        print("appWillEnterForeground")
    }
```

등등

## Launch Time



시스템이 돌려주는 코드

main에서 시작(objective-c)

first initialization에서 나의 코드(앱델리게이트에 속하는 코드들)를 시작 시켜줌

리스토어 유아이 스테이트와 나의 메서드들이 통신

파이널 이니셜라이제이션이 

## Main function

* 옵젝티브씨는 main.m에서 시작됨

* 스위프트에서는 메인파일이 없고 @UIAppicationMain 라고 앤트리포인트를 잡아준다

  ```swift
  @UIApplicationMain
  class AppDelegate: UIResponder, UIApplicationDelegate {
    // code...
  }
  ```

## Into the foreground

이벤트 해들을 이밴트 루프로 넘겨서 뭔가 실행되고 결과를 이벤트 핸들로 넘기고

into the background

앱 스위쳐해가지고 다른 앱으로 넘어가면 기존앱은 백그라운드로

앱이 백그라운드로 실행해도되는지 예스 오어 노

예스면 모니터링 이벤트하면서 이벤트 핸들을 계속 사용(이벤트 처리)

이벤트 처리 없으면 슬립들어감(서스펜디드)

## Into the background



## Handling alert-based interruptions

## background to foreground

## background transition cycle



## 호환성

13버전 이상은 씬델리게이트에서 이하는 앱델리게이트에서. 이중으로 코드를 작성해야한다.

그게 싫다하면. 그냥 앱델리게이트 하나만 작성한다. 그리고 아래와 같이 코드를 작성하고 설정을 수정한다.

1. 신델리게이트 삭제
2. 앱댈리게이트에서 신세션 라이프사이클부분(신세션컨피크, 씬세션) 삭제
3. 앱댈리에 var window: UIwindow 프로퍼티를 만든다
4. info.plist(앱이 돌아갈때 필요한 정보를 저장한 리스트)에서 어플 씬 메니페스트 삭제

필드에서는 사실상 신델리 안쓴다.

하지만 패캠 다니는 동안에는 그냥 디폴트 상태로 쓰면된다.



## 스토리보드

### 뷰

스토리보드의 View Controller는 ViewController.swift 파일과 연결된다.

![0423-SWFT-TIL-storyboard-Image01](/Users/woobincheon/Documents/fcios11/03-TIL/swift/SWFT-TIL-Images/0423-SWFT-TIL-storyboard-Image01.png)

View: 디바이스의 화면 전체

Safe Area: 노치를 고려한 View 영역(View에 포함되는 영역. 아이폰 X 부터 생김)

```swift
class ViewController: UIViewController { // UIViewController는 ViewController의 부모 클래스.

    override func viewDidLoad() {
        super.viewDidLoad()
        print("vidwDidLoad is executed")
        // Do any additional setup after loading the view.
      	view.backgroundColor = .systemBlue // 부모 클래스(UIViewController에 이미 정의돼있는 view라는 객체를 사용한다.
                                           // 컨트롤+커맨드+클릭으로 view의 정의에 대해 자세히 볼 수 있다.
    }
}
```

스토리보드랑 ViewController.swift랑 연결하는법(= 스토리보드에서 만든 요소에대해 ViewController.swift에서 수정할 수 있게)

* @IBOutlet을 사용한다.

* 방법 2가지

  1. 코드를 작성하고 스토리보드에서 드래그하여 연결

     Connection / Name / Type / Storage 등을 나타내는 코드를 작성한 후에 스토리보드의 요소와 드래그 연결

  2. 스토리보드에서 요소를 생성하고 코드영역에 드래그해서 연결

     드래그 연결 시, Connection / Name / Type / Storage 등을 설정하면 코드가 자동 생성된다.

```swift
class ViewController: UIViewController {

    @IBOutlet weak var view1: UIView!
    @IBOutlet weak var view2: UIView!
    @IBOutlet weak var view3: UIView!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        view1.backgroundColor = .yellow // 스토리보드에서 설정한 배경색 대신 yellow 적용됨
        view2.backgroundColor = .blue // 스토리보드에서 설정한 배경식 대신 blue 적용됨
        view3.backgroundColor = .green // 스토리보드에서 설정한 배경식 대신 green 적용됨
    }
}
```

연결해제

* 한번이라도 연결을 시켰으면 코드를 지우더라도 정보가 남기 때문에, 스토리보드에서도 지워줘야 에러 안남.

스토리 보드와 상관없이 코드 작성으로만 UIView 만들기

* 스토리 보드상에는 나타나지 않지만 빌드 후 실행 시키면 시뮬레이터에 적용된다.
* viewDidLoad 함수 안에 작성한다.

```swift
import UIKit

class ViewController: UIViewController {
    
    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        let myView = UIView()
        myView.frame = CGRect(x: 0, y: 0, width: 200, height: 200) // x/y로 위치를, width/heigth로 크기를 설정한다.
        myView.center = view.center // 내가 생성한 요소의 중심을 view의 중심으로 설정하겠다.
        myView.backgroundColor = .yellow
        view.addSubview(myView) // 맨 밑에 깔린 View 위로 내가 생성한 view를 올린다.
    }
}
```

* 응용:  세가지 view 만들어보기

```swift
        let myView1 = UIView()
        myView1.frame = CGRect(x: 0, y: 0, width: 100, height: 100)
        myView1.center = view.center
        myView1.backgroundColor = .black
        view.addSubview(myView1)
        
        let myView2 = UIView()
        myView2.frame = CGRect(x: 0, y: 0, width: 100, height: 100)
        myView2.center = view.center
        myView2.backgroundColor = .red
        view.addSubview(myView2)
        
        let myView3 = UIView()
        myView3.frame = CGRect(x: 0, y: 0, width: 100, height: 100)
        myView3.center = view.center
        myView3.backgroundColor = .yellow
        view.addSubview(myView3) // 가장 마지막에 코딩된 myView3가 가장 위에 올라와있음
				//view.insertSubview(myView3, at: 3) // view들의 z-축 순서를 정해줄 수 있음
```

스토리보드에 나오는 화살표(이니셜 뷰컨트롤 옵션)

스토리보드를 사용할때 어떤걸 제일 먼저 실행할건지 지정해줌![0423-SWFT-TIL-storyboard-Image02](/Users/woobincheon/Documents/fcios11/03-TIL/swift/SWFT-TIL-Images/0423-SWFT-TIL-storyboard-Image02.png)

아래와 같이 되면 빈바탕이 먼저 나온다

![0423-SWFT-TIL-storyboard-Image03](/Users/woobincheon/Documents/fcios11/03-TIL/swift/SWFT-TIL-Images/0423-SWFT-TIL-storyboard-Image03.png)

따라서 이걸로 에트리포이트 설정



### 버튼

눌렀을때(버튼에서 이벤트 발생) 창 뜨게 하는법: @IBAction 으로

1. 코딩 먼저하고 스토리보드랑 연결(연결하면 라인넘버 옆에 동그라미에 체크된다)
2. 스토리보드에서 만들고 코드에다가 드래그
   1. 커낵션 액션, 타입은 버튼으로설정, 이벤트: 해당 요소 박스 크기 안에서 터치가 떨어질때(touch up indside) 등등
   2. 샌더는 뭐냐 이벤트를 발생시킨 애가 샌더(여기서는 버튼)

```swift
    @IBAction func buttonDidTap(_ sender: Any) {
        print("눌렸따")
        print(sender) // 버튼에 대한 정보 뜬다. 그러나 타입이 Any 이므로 sender. 해서 정보 바꿀수없음
    }
    @IBAction func button1(_ sender: UIButton) {
        print("버튼@@@")
        print(sender) // 샌더 타입이 UIbutton으로 설정되어있기 때문에 sender. 으로 정보 바꿀 수 있따.
    }
```



3. 코드로만 만들기

   ```swift
   let myButton = UIButton(type: <#T##UIButton.ButtonType#>) 에서 타입이 뭐냐
   스토리보드에서 어트리뷰트 에서 타입부분에 해당하는 내용.기본적으로 .system 쓰면됨
   
   //완성
           let myButton = UIButton(type: .system)
           myButton.frame = CGRect(x: 100, y: 200, width: 200, height: 50)
           myButton.setTitle("마이버튼", for: .normal) // 평상시에(=for: .noraml) "마이버튼"을 title로 설정.
           myButton.setTitle("누르는중", for: .highlighted) // 누르고 있을때(=for: .highlighted) "누를중"을 title로 설정.
           view.addSubview(myButton)// 이걸로 view에 추가
   
   //진짜 완성
           myButton.addTarget(self, action: #selector(buttonDidTap(_:)), for: .touchUpInside)//이벤트사실을 전달하는 함수 target: 누구한테 전달?기본은 self, action: 동작할 내용. 함수전달 for 어떨떄 이벤트 발생시킬건가. 예를 들어 .touchUpInside
       }
   @objc    func buttonDidTap(_ sender: Any) {
           print("눌렸따")
       }//  버튼 이벤트랑 연결하려면, @objc 를 앞에 붙여야한다.
   ```





라벨

1. 코드로 추가하기

   뷰디드로드에서 만든거랑 상위에서 만든거(클래스 프로퍼티로 선언)랑 차이

   뷰디드로드 안에서 만든거: 그 안에서만 사용(수정) 가능. 예시) 버튼을 로디드안에 만들었을때, 터치시 텍스트나 버튼을 바꾸고 싶지만 바꿀 수 없음) 

   

해보기

버튼을 눌럿을떄 마다 카운트(UILabel에 나타나게)가 1씩 증가