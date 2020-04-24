# UI Guide

## Resolution

* 1point = 1pixel 은 최초 아이폰 모델
* render at Xx: 제품에 따라 X 값이 달라진다. iPhone X => render at 3x
* potrait 세로 landscape 가로
* Ulitmate resolution guide: **https://goo.gl/vwbzQY**

## UIViewContentMod

### scaling

1. scaletofill: 비율이 빠게지더라도 프레임에 채운다.(윈도우즈 배경화면의 '늘이기' 같은거)

2. scaleaspectfit: 비율을 그대로 유지해서 프레임에 맞춘다.(프레임의 가로/세로 중 짧은쪽에 맞춘다)

3. scaleaspectfill: 비율을 그대로 유지해서 프레임에 채운다.(프레임의 가로/세로 중 긴쪽에 맞춘다)

   화면이 300x1000이고 이미지가 600x500이면 Assets폴더에서 해당이미지를 1x(1pixel/1point)로 하면 화면을 벗어난다.

   따라서 최소 x2(2pixel/1point, 가로 세로 각각 적용) 해상도로 설정해야 (600x500) / 2가 되어서 화면 안에 들어온다.

### positioning

```swift
// 사용법
blueView.contentMode = .bottom
```

1. center
2. top
3. bottom
4. left(top left, bottom left)
5. right(top right, bottom right)

### Cordination system orientation

* CGRect(x: , y: , ...)
* 왼쪽상단이 (0, 0)

### View Frame

* View Frame의 좌표는 상위뷰(super view)를 기준으로 설정

  subView(sub view)는 mainView(super view)를 기준으로.(mainView는 화면 자체)

  subOfSubView(sub view)는 subView(super view)를 기준으로.

### frame과 bounds

* frame은 super view를 기준으로 한다. like position: absolute
* bounds는 자기 자신을 기준으로 한다. like position: relative
  * 스크롤뷰에서 이용됨. ex) 지도앱

### addSubView()

* super view와 sub view의 관계를 잘 고려해서 써야한다.

  ```swift
  view.addSubview(blueView) // view가 super view, blueView가 sub view
  ```

  