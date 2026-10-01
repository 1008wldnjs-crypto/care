# 레포 구성 (C)

> 결정: **레포 2개** (모노레포 아님)  
> 스택: React Native + Expo (TypeScript)

| 폴더 | 역할 | 경로 |
|------|------|------|
| `care` | 제품 명세·체크리스트 문서 | `/Users/yoojiwon/Desktop/care` |
| `care-consumer` | 소비자 앱 (노인·보호자) | `/Users/yoojiwon/Desktop/care-consumer` |
| `care-caregiver` | 사회복지사 앱 | `/Users/yoojiwon/Desktop/care-caregiver` |

각 앱 폴더는 **별도의 `.git`** 을 가집니다.

## 실행

```bash
# 소비자
cd ~/Desktop/care-consumer && npm start

# 사회복지사
cd ~/Desktop/care-caregiver && npm start
```

## 현재 상태

- 네비게이션 + 신청 플로우 UI
- 매칭: 지원자 목록·선택 / 1명 자동매칭 / 0명 재요청 (목 데이터, `src/services/applicants.ts`)
- 실제 로그인·결제·채팅 API는 아직 없음
- 구현 기준 문서: `care/docs/PRODUCT_SPEC.md`
