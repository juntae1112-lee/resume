# Growing Deep - 설계와 상태 관리에 대한 기술 기록

## 기간

진행 중

**Architecture, Clean Architecture, Kotlin, StateFlow, Concurrency, Technical Writing**

---

## 한 줄 소개

실무와 개인 프로젝트에서 마주친 설계 판단을 바탕으로, 책임 분리·의존성 방향·상태 관리·동시성에 대한 생각을 정리하는 기술 기록 저장소입니다.

## 주요 내용

### 아키텍처와 책임 분리

- 좋은 설계를 단순한 코드 정리가 아니라 변경 비용을 예측 가능하게 만드는 투자로 정리
- 공용 코드에 플랫폼별 예외와 정책이 쌓일 때 변경 영향 범위가 커지는 문제를 분석
- 하나의 Controller에 책임이 집중될 때 테스트 범위와 변경 영향도가 커지는 문제를 정리
- 같은 사용자 의도와 같은 변경 이유를 가진 흐름은 하나의 Feature Boundary 안에서 책임져야 한다는 관점 정리

### 의존성 방향과 인터페이스 소유권

- 다형성 자체보다 인터페이스를 누가 소유하는지가 더 중요하다는 관점 정리
- 비즈니스 흐름이 외부 구현의 모양을 따르기보다, 필요한 계약을 정의하고 구현체가 그 계약을 따르는 구조를 지향
- 외부 API, 지도 SDK, 데이터 저장 방식처럼 변경 가능성이 큰 세부사항이 Domain 규칙을 흔들지 않도록 경계를 나누는 방식 정리
- 개인 프로젝트 구조에 적용하며 Feature 간 직접 의존을 줄이고 공통 Domain 계약을 통해 연결하는 방향 검토

### StateFlow와 동시성 이해

- `StateFlow`와 immutable state를 사용할 때도 read-modify-write 경쟁 상태가 생길 수 있음을 예제로 정리
- `MutableStateFlow.update {}`가 CAS 기반 retry로 lost update를 방어하는 이유 학습
- 단순히 atomic 여부만 보는 것이 아니라, 실제 구조에서 state writer가 몇 개인지 확인해야 한다는 관점 정리
- single writer 구조가 상태 관리 복잡도를 줄이는 이유를 Kotlin 상태 관리 관점에서 정리

## 이력서 관점의 의미

- 실무 경험을 단순 나열하지 않고, 설계 판단과 회고를 기술 언어로 설명하는 연습
- Android/Kotlin 상태 관리, Feature Boundary, Domain 계약 설계에 대한 이해를 보완
- 코드 구현뿐 아니라 변경 비용, 책임, 의존성 방향을 설명할 수 있는 개발자로서의 강점 강화
