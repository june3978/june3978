# june3978

## UE 5.7 솔로개발 오버워치 스타일 FPS 제작 플랜

좋아. **"오캐이 진짜 시작"** 기준으로, 이제 설명 줄이고 바로 실행한다.
이 문서는 **오늘 당장 첫 플레이 가능한 빌드**를 만드는 체크리스트다.

---

## 0) 목표 재확인 (절대 안 바꾸기)
- 영웅 2명(딜러 1 / 서포터 1)
- 맵 1개(점령전)
- 8주 안에 플레이테스트 가능한 빌드

성공 기준:
1. 총알 맞으면 서버 기준으로 체력 감소
2. 점령 퍼센트 오르고 승패 판정
3. 스킬 1개 이상 실전에서 사용 가능

---

## 1) 오늘 바로 시작 플랜 (4시간)

### 0:00 ~ 0:30
- [ ] UE 5.7 C++ 프로젝트 생성 (First Person)
- [ ] 플러그인 On: GAS / Gameplay Tags / Enhanced Input / Common UI
- [ ] 기본 맵 저장: `Maps/MP_Control_01`

### 0:30 ~ 1:30
- [ ] 입력 구성: 이동, 점프, 발사, 재장전, Shift/E/Q
- [ ] 히트스캔 발사 구현
- [ ] 서버 판정 데미지 반영

### 1:30 ~ 2:30
- [ ] HUD에 체력/탄약 표시
- [ ] 점령 구역 Actor 배치
- [ ] 점령 퍼센트 UI 연결

### 2:30 ~ 4:00
- [ ] 라운드 종료 조건(100%)
- [ ] 2클라 테스트(호스트 + 클라)
- [ ] 발견 버그 5개까지 정리(치명도 포함)

---

## 2) 프로젝트 세팅 (최소 고정)

### 엔진/플랫폼
- Unreal Engine **5.7**
- Win64
- C++ 프로젝트

### 플러그인
- Gameplay Ability System
- Gameplay Tags
- Enhanced Input
- Common UI

### 폴더
```text
Source/ProjectName/
  Core/
  Characters/
  Weapons/
  Abilities/
  GameModes/
  UI/

Content/
  BP/
  Characters/
  Weapons/
  Abilities/
  Maps/
  UI/
  DataAssets/
```

규칙:
- 수치 하드코딩 금지
- 접두어 통일: `BP_`, `WBP_`, `DA_`

---

## 3) 8주 로드맵 (진짜 필요한 것만)

### Week 1
- 이동/발사/재장전
- 히트스캔 서버 데미지
- HUD 체력/탄약

### Week 2
- 점령전 룰 완성(퍼센트, 승패)
- 리스폰 처리

### Week 3
- GAS 연동
- 공통 Ability 슬롯(Shift/E/Q)

### Week 4
- 딜러 영웅 완성

### Week 5
- 서포터 영웅 완성

### Week 6
- 타격감(사운드, 카메라, 피격 피드백)

### Week 7
- 밸런스 1차 조정

### Week 8
- 버그픽스 + 플레이테스트 빌드

---

## 4) GAS 최소 규칙
- 스킬: `GameplayAbility`
- 효과: `GameplayEffect`
- 상태: `GameplayTag`
- 시작 Attribute: `Health`, `MaxHealth`, `Ammo`, `UltCharge`, `MoveSpeed`

원칙:
- 공통 코드가 2회 이상 반복될 때만 추상화
- 처음엔 동작 우선, 그다음 구조화

---

## 5) 첫 빌드 DoD (오늘 종료 기준)
- [ ] 발사/피격/체력 감소 동작
- [ ] 점령 퍼센트 + 라운드 종료 동작
- [ ] HUD 3요소(체력/탄약/점령도) 표시
- [ ] 2클라에서 동일 결과 확인

---

## 6) 막히면 이 순서로 해결
1. 입력 안 먹힘 → Enhanced Input MappingContext 확인
2. 데미지 미적용 → 서버 RPC 호출 경로 확인
3. UI 미갱신 → GameState 복제 변수 바인딩 확인
4. 동기화 튐 → CharacterMovement 보간값부터 고정

---

## 7) 지금 실행 선언
오늘 목표는 단 하나:
**"총 쏘고, 맞고, 점령해서, 승패가 난다"**

이거 완성 전에는 새 기능 추가 금지.
