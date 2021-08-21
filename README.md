# RunningCourse
### 산책하자! - 산책 루트 지도
---
### 1.  개발 목표
+ Google maps api를 사용한다.
+ CLLocationManager를 사용하여 현재 위치를 받아온다.


### 2.  개발 언어
+ Swift 

### 3.  개발 툴
+ Xcode 
+ Google maps Api

### 4.  최적화 디바이스
+ iPhone XR/XS Max/11/11 Pro Max  


### 5.  주요 기능
+ 위치가 달라짐에 따라 지도에 루트 표시 - Google maps api, CLLocationManager이용
+ 걸음 수 계산 - CMPedometer

### 6.  스크린샷
<img src="https://user-images.githubusercontent.com/63438947/130318561-25258217-c90c-460f-962a-58c9dfc650a4.png" width="30%"> 



### 7.  주요 개발 사항
+ 현재 위치를 받아온다.
```Swift
        mapView = GMSMapView()
        mapView.isMyLocationEnabled = true
      
        move(at: locationManager.location?.coordinate)
        mapView.settings.myLocationButton = true
        mapView.settings.zoomGestures = true
        mapView.settings.scrollGestures = true
        
        //3초에 delay후 현재 위치 마커 생성
        let time = DispatchTime.now() + .seconds(3)
        DispatchQueue.main.asyncAfter(deadline: time) {
            self.makeMarker(at: self.locationManager.location!.coordinate)
        }
        
        self.view = mapView
```
+ 루트 표시
  1. 3초마다 CLLocationCoordinate2D 배열에 현 위치를 담아 바로 path를 생성한다.
  2. polyline을 업데이트한다.
```Swift
  timer = Timer.scheduledTimer(timeInterval: 3, target: self, selector: #selector(makePoint(_:)), userInfo: nil, repeats: true)
  // 3초마다 아래 구문 실행
   @objc func makePointMarker(at coordinate: CLLocationCoordinate2D){
        let latitude = coordinate.latitude
        let longtitude = coordinate.longitude
        arr.append(CLLocationCoordinate2D(latitude: latitude, longitude: longtitude))
        print("cllocationcoordidate2D array\(i) = \(arr[i])")
        path.addLatitude(arr[i].latitude, longitude: arr[i].longitude)
        i = i + 1
        let polyline = GMSPolyline(path: path)
        polyline.strokeWidth = 5.0
        polyline.strokeColor = UIColor.colorWithRGBHex(hex: 0x00A89C)
        polyline.map = mapView
    }
  
```
+ 걸음 수
  CMPedometer.isStepCountingAvailable가 true면 step수를 update한다.
```Swift
  if CMPedometer.isStepCountingAvailable(){
            self.pedometer.startUpdates(from: Date()){ (data, error) in
                if error == nil{
                    if let response = data {
                        DispatchQueue.main.async {
                            print("Step Counter : \(response.numberOfSteps)")
                            self.longtitudeLabel.text = "\(response.numberOfSteps)"
                        }
                    }
                }
            }
        }
```
