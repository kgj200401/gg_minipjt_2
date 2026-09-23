# Frontend 기획 (React) — 착수 전 참고 문서

착수 전 상태에서 방향만 정리해둔 문서. 실제 구현은 아래 "착수 전 선행 작업"을 먼저 끝낸 뒤 시작한다.

## 디자인 참고 이미지

`docs/design/`에 보관.

- `screen_01_select_type.png` — 첫 화면. 관람객 유형 선택 (어린이 / 일반인 / 전문가)
- `screen_02_upload.png` — 유물 사진 촬영/업로드 화면
- `mascot_minsok.png` — 마스코트 원본 이미지 (배경 투명 PNG)

## 화면 구성 (이미지 기준)

### 1. 유형 선택 화면 (`screen_01_select_type.png`)

- 상단 배지: "국립중앙박물관 AI 도슨트"
- 마스코트(민석이) + 말풍선 인사 문구
- 타이틀: "관람객 유형을 선택해주세요"
- 카드 3개: 어린이 / 일반인 / 전문가, 각 카드에 "선택하기 →" 버튼
  - 선택된 카드는 강조 테두리(전문가 카드 예시에서 확인)
- 하단: 카피라이트, 우하단 도움말(?) 버튼

### 2. 유물 업로드 화면 (`screen_02_upload.png`)

- 상단바: 뒤로가기, "민석 AI 도슨트" 타이틀, 우측에 선택한 모드 표시(예: "일반인 모드")
- 업로드 박스: 카메라 아이콘 + "유물 사진을 업로드하세요" + "클릭하거나 파일을 드래그하세요"
- 하단 안내 카드: 마스코트 썸네일 + 사용 안내 문구

### 3. 마스코트 (`mascot_minsok.png`)

- 이름: 민석이
- 한복 차림, 손에 파란 구슬(빛나는 구체) 들고 있는 캐릭터
- 아이콘/로고, 채팅 아바타, 로딩 애니메이션 등에 재사용 예정

## 예상 라우트 / 컴포넌트 구조 (초안, 착수 시 조정 가능)

```text
frontend/src/
├─ pages/
│  ├─ SelectTypePage.tsx      # 화면 1
│  └─ UploadPage.tsx          # 화면 2 (이후 결과 화면 추가 예정)
├─ components/
│  ├─ Mascot.tsx              # mascot_minsok.png 사용
│  ├─ VisitorTypeCard.tsx     # 어린이/일반인/전문가 카드
│  └─ ImageUploadBox.tsx      # 업로드 박스
├─ assets/
│  └─ mascot_minsok.png       # docs/design에서 복사해 사용
└─ App.tsx
```

관람객 유형(어린이/일반인/전문가)은 이후 LLM 프롬프트/설명 난이도 분기에 쓰일 값이므로,
프론트에서 백엔드로 넘길 때 상태값 이름을 미리 통일해둘 필요가 있다 (예: `visitor_type: "child" | "general" | "expert"`).

## 착수 전 선행 작업

1. `docs/api_spec.md` 작성 — 최소한 아래 엔드포인트의 요청/응답 스키마 확정
   - 유물 이미지 업로드 → 분류 결과 (`artifact_id`, `confidence` 등)
   - 분류 결과 + `visitor_type` → 도슨트 설명 텍스트
2. 위 스키마를 기준으로 `frontend/`, `backend/` 뼈대를 함께 생성 (mock 응답도 이 스키마를 따르게)
3. `ai/vision/` 학습은 `data/processed/`에 train/val/test 이미지가 준비된 뒤 진행 (`docs/TODO.md` 참고)
