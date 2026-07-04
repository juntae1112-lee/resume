# 책바퀴 - 동네 기반 책 교환 Android 앱

## 기간

진행 중

**Kotlin, Jetpack Compose, Coroutine, StateFlow, Hilt, Navigation Compose, Supabase, Ktor, Mapbox**

---

## 한 줄 소개

동네를 기반으로 책을 교환하고, 책이 어떤 사람과 동네를 지나왔는지 기록하는 Android 앱을 개발하고 있습니다.

## 주요 기여

### Android 앱 구조 설계

- Compose 기반 화면과 ViewModel을 MVI 구조로 구성
- 사용자 액션을 Intent로 받고, 화면 상태는 State, 일회성 이벤트는 Effect로 분리
- Feature UI, Presentation, Domain, Data 계층을 나누고 기능 간 의존은 공통 Domain 계약을 통해 연결
- AppNavHost에서 feature별 NavGraph를 조립하고, 각 feature가 자신의 화면 흐름을 소유하도록 구성

### 기술적 의사결정

- 화면별 Intent와 State를 하나의 Contract에 모아 사용자 액션과 화면 상태의 변경 지점을 명확히 관리
- 같은 feature 안에서도 상태와 처리 메서드가 흩어지면 변경 이력 추적과 영향 범위 파악이 어려워진다고 판단해, 화면 단위 상태 관리 구조를 적용
- 동네 기반 지도 Fit과 사용자 동네 표시를 위해 동네 정보를 별도 테이블로 분리하고, 프로필에는 `neighborhood_id`만 저장하도록 설계
- feature 간 직접 의존이 커지면 한쪽 feature의 수정이 다른 feature로 전파될 수 있다고 판단해, 공통 Domain 계약을 통해 필요한 기능만 연결

### 사용자·동네 데이터 모델링

- Supabase Auth UUID를 사용자 식별자로 사용하고, `profiles` 테이블과 연동
- 프로필의 동네 문자열을 `neighborhood_id` 참조 구조로 전환
- `neighborhoods` 테이블을 분리해 동네 표시명, 좌표, provider code를 관리
- 선택한 동네를 먼저 catalog에 upsert한 뒤 profile의 `neighborhood_id`를 갱신하는 흐름 설계

### 동네 기반 탐색 UX

- 내 동네가 없을 때 빈 지도와 동네 설정 CTA를 보여주는 초기 진입 흐름 설계
- 위치 권한이 있을 때 주변 동네 후보를 조회하고, 권한이 없거나 위치 매칭에 실패하면 직접 선택으로 폴백

### 안정성 및 예외 처리

- Supabase/PostgREST 오류를 도메인 예외로 변환해 UI가 외부 SDK 오류를 직접 알지 않도록 처리
- 세션 만료, 권한 거부, 네트워크 오류를 사용자 행동 기준 메시지로 분리
- 동네 저장 실패 시 진행 상태를 닫고 drawer를 다시 열어 재시도 가능한 흐름으로 복구
