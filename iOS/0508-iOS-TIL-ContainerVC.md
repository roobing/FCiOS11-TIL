# Container View

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



탭바

탭바 아이템마다 각각의 뷰컨이 연결됨

최대 5개까지만 표시됨. 그 이상은 ... 으로 표시됨.

아이템을 5개 초과해서 쓸 일은 거의 없음



# UserDefault

메모리와 파일로 저장하는 형태의 차이점은?

메모리는 앱이 종료되면 업서지고 파일은 아니다



유저디폴트는 보통 싱글톤으로 만들어쓴다

싱글톤: 인스턴스 한개만 만들어서 모두가 공유해서 쓰는것



키 자체가 있는지 확인해보려면

.contain 이런걸로 확인해야한다.



모든 타입을 그대로 다 집어넣을수있는건 아니다

기본제공타입들(bool, int, double 등등)은 바로 저장 가능하다.


