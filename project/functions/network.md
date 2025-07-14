## 🔍 네트워크 통신

서버와 통신하기 위해 `HTTP` 기반의 통신 인터페이스를 `Retrofit2` 라이브러를 사용하여 구현하였습니다. <br>
통신 방식은 `REST API` 기반이며, 데이터는 `JSON` 형식으로 송/수신합니다. <br><br>

## 📡 API 목록

<img src="https://github.com/user-attachments/assets/ed0071e4-b097-444a-b4c0-5a128eb0da9b" width="50%" height="200%" />
<br><br>

## 📝 체크 리스트

- [x] API 문서 기반 인터페이스 구현 
- [x] Repository 패턴 적용하여 데이터 접근 로직을 추상화
- [x] 이미지 및 POI 데이터 서버 업로드 / 사용자 정보 수신  
- [x] 안정적인 흐름 제어를 위한 `Coroutine` 사용
<br>

## 📷 실행 화면

| 이미지 확인 | 업로드 요청 | 이미지 검토 |
| :-------------: | :-------------: | :-------------: |
| <img src="https://github.com/user-attachments/assets/65c34998-a20c-4a5c-a2e6-72ee25e27897" width="200" height="450"/> | <img src="https://github.com/user-attachments/assets/f28f5870-109b-4359-aec2-c41fa59f0b75" width="200" height="450"/> | <img src="https://github.com/user-attachments/assets/f44dc007-f98c-4808-952f-090278620c06" width="270" height="360"/> | 
 
|  업로드 정보 조회  |
|:-------------:|
| <img src="https://github.com/user-attachments/assets/8a8bbdff-558f-4925-8620-aa2d9484f21c" width="400" height="315"/> |

<br>

## 📮 관련 이슈

### Http 통신 로직 구현
`Retrofit` 인터페이스를 Hilt를 통해 의존성 주입하여 사용하였습니다.  
각 도메인별 `Repository`에 필요한 API만 주입받아 결합도를 낮추고 테스트 용이성을 확보했습니다.  
네트워크 흐름 중 발생할 수 있는 예외 상황에 대비하여, 에러 처리와 흐름 제어를 명확히 분리하였습니다. <br><br>

### AR 이미지 업로드 흐름 개선
기존에는 Unity에서 이미지를 촬영한 후, 서버 업로드까지 완료한 뒤에 Android 앱으로 전환되는 구조였습니다. <br>
하지만 간혹 업로드가 완료되기 전에 Unity 앱이 먼저 종료되거나 Android 앱으로 전환되면서 업로드가 실패하는 문제가 발생했습니다.  

Unity에서 Android의 업로드 메서드를 직접 호출할 수 있도록 인터페이스를 구성하고, 업로드는 Android에서 수행하도록 구조를 변경하였습니다. <br><br>

### 메인 홈 화면에서의 비동기 처리 수정
홈 화면에서는 소셜 로그인 정보와 POI 데이터를 서버에서 받아와 지도에 마커로 표시해야 하는 로직이 있습니다. <br>
기존에는 `Coroutine`으로 병렬 처리하던 중, 데이터가 준비되기 전에 `Google Map`이 먼저 실행되어 마커 렌더링에 실패하고 크래시가 발생하였습니다. <br>  

모든 비동기 작업을 하나의 `Coroutine`블록 내에서 순차적으로 (사용자 정보 → POI 데이터 → 지도 렌더링) 실행되도록 구조를 변경하였습니다. <br>
네트워크 통신에 실패하더라도 기본 마커를 먼저 표시하도록 예외 처리를 추가하여 앱이 종료되지 않도록 개선하였습니다. <br><br>
    
|    POI Marker    |
|:-------------:|
| <img src="https://github.com/user-attachments/assets/6beafa27-9e61-4f0e-bcd7-ceaed11354cb" width="200" height="450"/> |

