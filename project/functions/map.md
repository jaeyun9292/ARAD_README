## 🔍 Google Map
사용자가 앱에서 AR 광고판이 설치된 위치를 조회할 수 있도록, 구글 맵 기능을 적용하였습니다. <br>
 여러 지도 API 중 Google Map을 선택한 이유는 다음과 같습니다. <br>

**다양한 API 및 개발 자료 제공**
- 카메라 이동 및 회전
- 마커 및 지도 위 오브젝트 표시  
- TimeZone 등 다양한 기능 제공
  
**전 세계 데이터 커버리지**
- ARCore Geospatial API 기반 좌표 데이터를 활용해 여러 국가에서 활용 가능
  
**풍부한 지도 커스터마이징 기능**
- 지도 스타일, 마커 디자인 등 다양한 시각적 요소를 자유롭게 설정 가능

<br>

## 📝 체크 리스트

- [x] 구글 Map API 연동  
- [x] 지도 테마 커스터마이징  
- [x] POI(위·경도) 기반 마커 활성화  
- [x] 마커 Bitmap 커스터마이징  
- [x] 가까운 지하철 및 AR 마커 거리 측정

<br>

## 📷 실행 화면
  
|   구글 맵1   |   구글 맵2   |  구글 맵3 |
| :-------------: | :-------------: | :-------------: |
| <img src="https://github.com/user-attachments/assets/f609b5d1-ec34-418e-816e-e49bc551d838" width="200" height="450"/> | <img src="https://github.com/user-attachments/assets/7358795b-8745-4d72-a65a-087ed350da86" width="200" height="450"/> | <img src="https://github.com/user-attachments/assets/138a3398-a4ca-43ca-afa1-870970bd3733" width="200" height="450"/> |

<br>

## 📮 세부 내용

### 구글 맵 스타일 커스텀

`GoogleMap.setMapStyle()`을 사용해 스타일을 커스터마이징했으며, [스타일 마법사](https://mapstyle.withgoogle.com/)와 [Subtle Grayscale](https://snazzymaps.com/style/15/subtle-grayscale) 디자인을 참고했습니다.

```kotlin
mGoogleMap.setMapStyle(MapStyleOptions.loadRawResourceStyle(mContext, R.raw.style2_json))
```

<br>

### 마커 커스텀

커스텀 이미지와 타이틀을 포함한 마커를 생성합니다.

```kotlin
mGoogleMap.addMarker(
    MarkerOptions()
        .position(ttukseomStarbucks)
        .title("뚝섬 스타벅스")
        .icon(BitmapDescriptorFactory.fromBitmap(bitmapStarBucks!!))
)
```

```kotlin
bitmapStarBucks = createUserBitmap(
    BitmapFactory.decodeResource(
        mActivity.resources, R.drawable.ic_profile_ex2
    )
)
```

<br>

### POI 위·경도 기반 카메라 이동 및 마킹

MapView `onCreate()` 이후 서버로부터 위경도 데이터를 수신하고 콜백을 활용해 `setLastLocation()`을 실행합니다.

```kotlin
override fun setLastLocation(location: Location) {
    val myLocation = LatLng(location.latitude, location.longitude)

    val marker = MarkerOptions()
        .position(myLocation)
        .title("i am here")

    val cameraOptions = CameraPosition.Builder()
        .target(myLocation)
        .zoom(18.0f)
        .tilt(45.0f)
        .bearing(90f)
        .build()

    val camera = CameraUpdateFactory.newCameraPosition(cameraOptions)

    MyLocation = myLocation
    MyCameraOptions = cameraOptions
    MyCamera = CameraUpdateFactory.newCameraPosition(MyCameraOptions)
}
```
