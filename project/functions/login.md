## 🔍 소셜 로그인
회원가입 절차를 간소화하기 위해 소셜 로그인 기능을 구현하였습니다. <br>
Google과 Kakao에서 제공하는 OAuth API를 통해 Access Token을 발급받아 인증 절차를 진행하였고, <br>
이후 자체 서버로 인증 정보를 전달하여 사용자 식별 및 로그인 처리를 완료하였습니다.
<br><br>

## 📝 체크 리스트
  - [x] Google, Kakao 소셜 로그인
  - [x] 회원 가입 및 탈퇴 기능
  - [x] 로그인 토큰 수령 및 사용자 정보 수집
  - [x] UX 최적화
<br><br>

## 📷 실행 화면

|    소셜 로그인1    |   소셜 로그인2   |  소셜 로그인3 |
| :-------------: | :-------------: | :-------------: |
| <img src="https://github.com/user-attachments/assets/4e724765-1df1-4c0b-b905-6f19a8fc40e8" width="200" height="450"/> | <img src="https://github.com/user-attachments/assets/5d57208e-83a1-40c9-ae8e-55ac177da679" width="200" height="450"/> | <img src="https://github.com/user-attachments/assets/92da183d-680a-439f-8dab-e4edb66f2d72" width="200" height="450"/> |

<br>

## 📮 관련 이슈

### 로그인 클래스 분리 및 구조 개선
초기에는 로그인 기능이 특정 화면에 직접 구현되어 있어, 앱 전역에서 재사용하거나 테스트하기 어려운 구조였습니다. <br>

이를 해결하기 위해, 로그인 로직을 클래스 단위로 분리하고 Repository 패턴을 적용하여 도메인 계층으로 분리하였습니다. <br>
이후 클린 아키텍처 기반으로 ViewModel → UseCase → Repository 구조를 도입했습니다. <br>

```kotlin
/* LoginViewModel.kt */
@HiltViewModel
class LoginViewModel @Inject constructor(
	private val loginWithKakaoUseCase: LoginWithKakaoUseCase,
	private val loginWithGoogleUseCase: LoginWithGoogleUseCase,
	private val logoutUseCase: LogoutUseCase
) : ViewModel() {

	private val _loginResult = MutableStateFlow<Result<UserInfo>?>(null)
	val loginResult: StateFlow<Result<UserInfo>?> = _loginResult.asStateFlow()

	fun loginWithKakao() {
		viewModelScope.launch {
			val result = loginWithKakaoUseCase()
			_loginResult.value = result
		}
	}

	fun loginWithGoogle() {
		viewModelScope.launch {
			val result = loginWithGoogleUseCase()
			_loginResult.value = result
		}
	}

	fun logout() {
		viewModelScope.launch {
			logoutUseCase() 
			_loginResult.value = null
		}
	}
}
```
