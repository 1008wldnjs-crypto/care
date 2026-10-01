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
| `Desktop/care` | 명세·핸드오프 | `origin` → `https://github.com/1008wldnjs-crypto/care.git` |
| `Desktop/care-consumer` | 소비자 Expo 앱 | **remote 미설정** (푸시 전 `gh repo create` 등 필요) |
| `Desktop/care-caregiver` | 사회복지사 Expo 앱 | **remote 미설정** |

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
주요: `src/data/catalog.ts`, `src/services/applicants.ts`, `src/services/sessionStore.ts`, `src/constants/app.ts`

### care-caregiver — 목 UI 완료

피드 → 지원 → 일정 → 채팅 → 완료 → 정산(15%)  
주요: `src/services/jobStore.ts` (지원 후 데모로 즉시 matched)

### 미구현

Firebase, 실로그인, 실PG, 두 앱 연동, 조건 조정 제안 UI

---

## 4. 다음에 할 일

| 순위 | 작업 |
|------|------|
| **추천** | Firebase로 의뢰·지원·매칭 연동 |
| 대안 | 카카오/네이버/전화 실로그인 |
| 선행 가능 | consumer·caregiver GitHub remote 생성 후 push (다른 Mac 동기화) |

사용자 미선택 시 Firebase vs 로그인 장단지를 제시하고 기다린다.

---

## 5. 세션 마무리 체크리스트

1. [ ] §3·§4·§6 갱신  
2. [ ] PRODUCT_SPEC과 결정 동기화  
3. [ ] 기능별 commit (+ remote 있으면 push)  
4. [ ] 다른 Mac: pull → `npm install`

---

## 6. 세션 로그

### 2026-10-02 (핸드오프·커밋)

- **한 일:** HANDOFF/규칙 정리, 기능별 커밋·push 시도(문서 레포). 이전 세션에서 소비자 A플로우·제공자 목 UI·매칭/고객센터 정책 반영 완료 상태 기록.
- **다음:** Firebase 또는 실로그인 · 앱 2개 remote 설정
- **주의:** DEV에서 매칭 대기 타이머 단축 · caregiver 지원 후 즉시 매칭 데모 · 메모리 스토어는 재시작 시 초기화

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
