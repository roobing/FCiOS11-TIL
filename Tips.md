# Tips

1. 라벨.center = CGPoint(x: , y:)
   * 해당 라벨의 중앙 점을 정해주는 것
2. 라벨.origin = CGPoint(x:, y:)
   * 해당 라벨의 시작 점을 정하는 것
3. 라벨.frame.size = CGSize(width: , height:)

.center/.origin의 타입은 CGPoint 이고, .frame.size의 타입은 CGSize 이다

* storyboar로 만든 view controller를 소스코드에서 참조하려면 UIStoryboard 인스턴스를 생성해서 접근해야한다.
  * 자세한 내용은 도움말'UIStoryboard'와 재은기본 참고

4. 데이터 전달할 때 옵셔널 타입을 많이 활용하라

5. viewDidLoad 기준으로 위로는 IBOutlet, 아래로는 IBAction

6. 각 기능별로 extension 으로 나눠서 작성

   ```swift
   class ViewController: UIViewController {
   
       @IBOutlet var displayLabel: UILabel!
       
     //code...
     
       override func viewDidLoad() {
           super.viewDidLoad()
       }
     
       func setDisplay(by num: String) {
           displayLabel.text = num
       }
       
       @IBAction func numKeys(_ sender: UIButton) {
           newNum = newNum * 10 + (Int(sender.currentTitle ?? "0") ?? 0)
           setDisplay(by: String(newNum))
       }
     // code...
     
     /// MARK - UI
     extension ViewController {
       button.frame.
     }
       
       
   }
   ```
   
   