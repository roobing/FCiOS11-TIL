# Container View Controller

## Navigation Controller

네비게이션 컨트롤러: 화면이 present되는게 아니라 나와서 덮는것



프레젠트는

루트뷰를 기점으로 계속 띄우는 것



네비게이션 컨트롤러

루트뷰로 네비게이션컨트롤러를 할당하고 네비게이션컨트롤러

탑뷰는 네이게이션 스택(차일드뷰의 적층)의 가장위

비저블뷰는 가장 최종적으로 보이는 뷰(프레젠트된 뷰)

타이틀

프롬프트: 타이틀위에 작게 나오는

프리퍼 라지 타이틀

* 전환방법
  * Embed In-> 2nd View 추가 및 Storyboard ID 설정-> Root View에 navigation item 추가 -> navigation item에 bar button 추가 ->  bar button을 Root View에 IBAction 연결 -> instantiateViewController로 2nd View 인스턴스 생성-> pushViewController로 전환

## Tabbar Controller

탭바

탭바 아이템마다 각각의 뷰컨이 연결됨

최대 5개까지만 표시됨. 그 이상은 ... 으로 표시됨.

아이템을 5개 초과해서 쓸 일은 거의 없음

