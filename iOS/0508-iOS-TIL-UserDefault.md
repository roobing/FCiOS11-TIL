# UserDefault

메모리와 파일로 저장하는 형태의 차이점은?

메모리는 앱이 종료되면 업서지고 파일은 아니다



유저디폴트는 보통 싱글톤으로 만들어쓴다.



싱글톤: 인스턴스 한개만 만들어서 모두가 공유해서 쓰는것



키 자체가 있는지 확인해보려면

.contain 이런걸로 확인해야한다.



모든 타입을 그대로 다 집어넣을수있는건 아니다

기본제공타입들(bool, int, double 등등)은 바로 저장 가능하다.
