# 다정 앱 — 에이전트 핸드오프 (HANDOFF)

> **이 파일을 읽는 에이전트:** 새 세션/다른 Mac에서 작업을 이어갈 때 **가장 먼저** 이 문서를 읽고, 구현 기준은 `docs/PRODUCT_SPEC.md`를 따른다.  
> **사람(범석):** 매 작업 마무리 시 아래 「세션 로그」에 남기고 「현재 상태」「다음에 할 일」을 고친다.

---

## 0. 다른 에이전트에게 바로 주는 지시 (복붙용)

```
다정(재가요양/방문케어) Expo 앱 작업을 이어서 해 줘.
1) care/docs/HANDOFF.md 와 care/docs/PRODUCT_SPEC.md 를 먼저 읽어.
2) 코딩 원칙: 관련 없는 코드 수정 금지, 추측 금지(TBD는 임의 확정 금지), 여러 방법이면 선택지 제시, 최소 변경.
3) 앱은 레포 2개: Desktop/care-consumer (소비자), Desktop/care-caregiver (사회복지사). 문서는 Desktop/care.
4) 이미 동작하는 화면은 요청 없이 수정하지 마.
5) 다음 작업은 HANDOFF의 「다음에 할 일」을 보고 진행해. 불확실하면 질문해.
```

---

## 1. 프로젝트 위치

| 경로 | 역할 | 원격 |
|------|------|------|
| `Desktop/care` | 명세·핸드오프 | `origin` → `https://github.com/1008wldnjs-crypto/care.git` (**push 완료**) |
| `Desktop/care-consumer` | 소비자 Expo 앱 | `origin` → `https://github.com/1008wldnjs-crypto/care-consumer.git` (**push 완료**, `4f15123`) |
| `Desktop/care-caregiver` | 사회복지사 Expo 앱 | `origin` → `https://github.com/1008wldnjs-crypto/care-caregiver.git` (**push 완료**, `9132301`) |

- 모노레포 아님. Mac 전환 시 **세 폴더** 모두 동기화.
- 구현 기준: `care/docs/PRODUCT_SPEC.md`

```bash
cd ~/Desktop/care-consumer && npm start
cd ~/Desktop/care-caregiver && npm start
```

---

## 2. 확정 정책 요약

- 앱명 **다정** · 로그인: 카카오/네이버/전화 (Google·Apple·이메일 제외)
- 결제: 서비스 종료 후 · 플랫폼 수수료 **15%**
- 취소: 24h **20%** / 12h **30%** / 노쇼 **100%**
- 매칭: 1명 자동 · 다수 별점순 직접선택 · 0명 시 24h 재요청 버튼 · 36h 자동 재요청 · 이후 앱 안 고객센터
- 지원자 카드: 이름·경력·후기미리보기·후기개수
- 상주: 접수 안내만 (앱에서 전화 안 검)
- 고객센터: 앱 안 문의 폼
- 단가·제공자 자격: TBD (임의 확정 금지)
- 백엔드 예정: Firebase (미연동)

---

## 3. 현재 구현 상태

### care-consumer — 목 UI 플로우 완료

신청 → 매칭(지원자) → 확정 → 채팅 → 결제 → 리뷰 + 상주 + Support  
첫 화면은 가입 전 한 장(카카오·네이버·휴대폰, 로그인, 둘러보기). 홈은 2열 3행. 하단은 예약하기·예약 내역·실시간·대화하기. 내 정보는 오른쪽 위 버튼이고 탭은 아니다.  
예약은 일정 다음 케어 항목. 상주만 전화 상담. 실시간·대화하기·예약 카드·내 정보 하위 메뉴는 정해진 문구·제목만 있다.  
주요: `src/data/catalog.ts`, `src/services/applicants.ts`, `src/services/sessionStore.ts`, `src/constants/app.ts`

### care-caregiver — 목 UI 완료

피드 → 지원 → 일정 → 채팅 → 완료 → 정산(15%)  
주요: `src/services/jobStore.ts` (지원 후 데모로 즉시 matched)

### 미구현

Firebase, 실로그인, 실PG, 두 앱 연동, 조건 조정 제안 UI

---

## 4. 다음에 할 일

**확정 (2026-10-05):** 로그인 실연동을 먼저 한다. Firebase 의뢰 연동은 그 다음.

로그인 방식: 별도 가입 폼 없이 카카오 연동 / 네이버 연동 / 휴대폰 인증번호로 계정 생성.

바로 이어서: 소비자 앱은 iPhone에 설치됨. 첫 실행 전에 설정에서 개발자 인증서를 신뢰해야 함.

진행 (2026-10-09):
- Xcode 27 설치됨. 라이선스 동의 완료. iPhone(UDID `00008140-001E50201A90801C`, iPhone17,1) paired.
- Node `~/.local/node` (v24.21.0). CocoaPods는 mise Ruby 3.3의 `pod` 1.17.0.
- 원본 경로 `Downloads/🇨🇦/...` 는 CocoaPods가 UTF-8 경로에서 죽음. 빌드 복사본: `~/src/care-consumer`. 심볼릭 링크 `~/care-consumer`는 같은 이유로 불가.
- `npx expo run:ios --device` 가 `No code signing certificates are available to use` 로 중단. Xcode에 Apple ID(Team) 추가가 필요.
- prebuild가 care-consumer의 `app.json`(bundleIdentifier `com.anonymous.dajeong-consumer`)과 `package.json` ios/android 스크립트를 바꿈. `ios/`는 생성됨.

진행 추가 (2026-10-10):
- 빌드 성공. `com.anonymous.dajeong-consumer`가 iPhone에 설치됨.
- 첫 실행은 거절됨. 아이폰이 개인 개발자 서명을 아직 신뢰하지 않음. 설정 → 일반 → VPN 및 기기 관리에서 신뢰하면 열림.
- 빌드 복사본은 `~/src/care-consumer`. 디버그 실행은 맥의 Metro(8081)가 켜져 있어야 함.

막힌 것:
- 카카오·네이버 개발자 키, 문자 발송 설정은 아직 없음. 이번 설치는 현재 목 UI.
- 2026-10-10 네 화면 빌드 후 설치함. `expo run:ios` 설치는 잠금 때문에 한 번 실패했고, 이어서 `devicectl`로 설치했다. 아이콘은 네이티브라 Metro만으로는 안 바뀐다.

세 레포 `main`은 2026-10-13에 push됨. 다른 Mac은 각 폴더에서 `git pull` 한 뒤 앱은 `npm install`.

---

## 5. 세션 마무리 체크리스트

1. [ ] §3·§4·§6 갱신  
2. [ ] PRODUCT_SPEC과 결정 동기화  
3. [ ] 기능별 commit (+ remote 있으면 push)  
4. [ ] 다른 Mac: pull → `npm install`

---

## 6. 세션 로그

### 2026-10-05 (로그인 우선순위·실기기)

- **결정:** 다음 구현은 로그인 실연동. 회원가입 폼 없이 카카오 / 네이버 / 휴대폰 인증번호로 계정을 만든다.
- **바로 하고 싶은 것:** 소비자 앱을 실제 기기에 설치해서 보기.
- **확인:** 이 Mac에 Node.js 없음. 카카오·네이버·문자 키 없음. Expo SDK 57이라 스토어 Expo Go와 버전이 맞아야 열린다.
- **다음:** 폰은 iPhone, USB 연결 확인. Xcode.app 없음 → 앱스토어에서 Xcode 설치 필요. 설치 후 소비자 앱을 기기에 빌드.

### 2026-10-09 (아이폰 설치)

- Xcode 27 설치·라이선스 동의. 아이폰 paired.
- CocoaPods는 폴더명의 🇨🇦 때문에 원본 경로에서 실패. 빌드는 `~/src/care-consumer` 복사본.
- 빌드는 코드 서명에서 멈춤. Xcode에 Apple ID를 넣었다고 해도 프로젝트 Team·개발 인증서는 아직 없음.
- 2026-10-10: 빌드·설치 성공. 첫 실행은 Untrusted Developer. 설정에서 개발자 신뢰 후 다시 열면 됨.
- 시작 화면: 돌봄 소개 그림 후 가입. 가입하면 바로 홈.
- 홈 순서: 운동·케어·음식·청소·배움·상주. 청소 세부는 바닥 정리, 옷·침구 정리, 화장실 주변. 공부는 배움.
- 상주 상담 시간대 「24시간」 문구는 「입주 간병」.
- 2026-10-10에 붙여 넣은 별도 명세(방문 시작 청구, 네이버페이, 알바·인지활동 등)는 **무시**. 구현 기준은 `PRODUCT_SPEC.md`.
- 색만 별도로 확정: 주황 `#E8913A` / 버튼 `#C2570C` / 초록 `#2F6B4F` / 배경 `#FFFBF6`. 소비자·사회복지사 `colors.ts`에 반영.
- 홈 하단 4칸: 예약하기 · 예약 내역 · 라이브 보기 · 대화하기. 프로필은 오른쪽 위. 주소는 예약하기에서 프로필 왼쪽. 라이브는 준비 중 문구만.
- 앱 아이콘 그림은 `assets/icon.png`와 iOS AppIcon에 주황 하트로 들어감. 홈 화면 반영을 위해 재설치함. 실행은 다시 개발자 신뢰가 필요함(설정 → 일반 → VPN 및 기기 관리).
- 홈은 그림의 큰 분류. 운동·케어, 청소·음식, 배움, 상주. 안부·정서 지원·약 복용은 케어로 합침. 정서 지원 작은 글씨는 외로움, 노인우울증.
- 임시 단가가 있는 것: 식사 준비·식사 도움 25,000원, 산책 등 운동 25,000원, 인지재활 27,000원, 병원 동행 32,000원. 나머지는 금액 미정.
- 로고는 집 안에 하트. 가운데 점선은 넣지 않는다. 앱 아이콘 배경은 `#E8913A`, 집과 하트는 `#FFFFFF`. 앱 안 심볼은 `#E8913A`.
- 음식 세부는 식사 도움(기존 임시 단가 시간당 25,000원), 반찬 구독(금액 미정, 신청으로 넘어가지 않음).
- 운동은 세부 선택에서 금액을 숨긴다. 금액은 일정 다음 예상 가격 화면에서 보여 준다.
- 주요 버튼 배경은 `#D9691F`, 눌렀을 때는 `#C2570C`. 글자는 흰색 18px 굵게. 같은 날 네 화면 시안으로 이 색은 대체됨.

### 2026-10-10 (고객용 화면 네 장)

- **결정:** 시안 네 장(첫 화면, 홈, 예약 내역, 내 정보)이 이전 홈 배치와 버튼 색을 대체한다. 주황 버튼 글자는 `#2B2420`. 버튼 배경 `#FF8A1F`, 눌렀을 때 `#E67300`. 로고·아이콘 배경은 `#E8913A`.
- 말은 「내 정보」, 「인지 활동」, 탭 「실시간」. 글꼴 Pretendard. 아이콘은 선 아이콘.
- 홈 칸 문구는 시안 그대로. 안에서 고르는 항목과 임시 단가는 그대로다. 청소 칸 설명은 「정리정돈·빨래」이고, 선택 항목은 청소 및 정리정돈·용변 처리다.
- 당일 긴급은 일정에서 오늘이 선택된 상태다. 10분·20분 매칭은 만들지 않았다.
- 일러스트, 실시간·대화 화면, 예약 카드 디자인, 결제수단·공지·환경설정·사회복지사 지원 화면은 비워 두었다.
- 빌드 후 iPhone에 설치함. Metro(8081)가 켜져 있어야 디버그 앱이 열린다.

### 2026-10-13 (다른 Mac으로 넘김)

- care, care-consumer(`4f15123`), care-caregiver(`9132301`)의 `main`을 GitHub에 push함.
- 다른 Mac은 세 저장소를 pull한 뒤 앱 폴더에서 `npm install`.
- 소비자 빌드는 경로의 🇨🇦 때문에 `~/src/care-consumer` 복사본에서 한다.

### 2026-10-02 (핸드오프·커밋·푸시)

- **한 일**
  - `HANDOFF.md`·`handoff.mdc` 갱신/추가
  - **care** 커밋·push: `docs: 다른 Mac·에이전트용 핸드오프…`
  - **care-consumer** 기능 커밋 3개 (remote는 URL만 연결, GitHub 레포 미생성으로 push 실패)
    - `feat(consumer): 공유 UI·단가표·앱 상수 기반을 추가한다`
    - `feat(consumer): 신청·매칭·상주 상담 플로우 UI를 추가한다`
    - `feat(consumer): 채팅·결제·리뷰·고객센터 목 플로우를 추가한다`
  - **care-caregiver** 기능 커밋 3개 (동일하게 push 실패)
    - `feat(caregiver): 공유 UI·단가·앱 상수 기반을 추가한다`
    - `feat(caregiver): 의뢰 피드·상세·지원 플로우를 추가한다`
    - `feat(caregiver): 내 일정·채팅·도움완료·정산 화면을 추가한다`
- **사용자 할 일:** GitHub에서 `care-consumer`, `care-caregiver` 빈 레포 생성 후 각 폴더에서 `git push -u origin main`
- **다음 구현:** Firebase 또는 실로그인

### 2026-10-02 (이전 작업 — 구현)

- 소비자: 지원자 UI(B), 채팅·결제·리뷰(A), 가입 수단(C), 상주(D), 앱 안 고객센터(E)
- 제공자: 피드·지원·일정·채팅·완료·정산 목 UI
- PRODUCT_SPEC 동기화

---

## 7. 관련 문서

| 파일 | 용도 |
|------|------|
| `docs/HANDOFF.md` | 이어하기 (본 파일) |
| `docs/PRODUCT_SPEC.md` | 구현 기준 |
| `.cursor/rules/coding-principles.mdc` | 코딩 원칙 |
| `.cursor/rules/handoff.mdc` | 세션 종료 시 HANDOFF 갱신 |
