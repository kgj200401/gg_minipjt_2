# Git 협업 규칙

## 1. 브랜치 전략

우리 팀은 아래 구조로 브랜치를 관리한다.

```text
main
└── develop
    ├── feature/vision-efficientnet
    ├── feature/rag-search
    ├── feature/llm-quantization
    ├── feature/backend-api
    └── feature/frontend-ui
```

### `main`

- 발표 또는 배포 가능한 안정 버전만 유지한다.
- 직접 작업하지 않는다.
- `develop`에서 검증된 내용만 병합한다.
- 직접 `push`하지 않는다.

### `develop`

- 팀 통합 브랜치이다.
- 각 기능 브랜치는 최종적으로 `develop`에 PR을 통해 병합한다.
- 직접 `push`하지 않는다.
- 기능 통합 및 테스트 기준 브랜치로 사용한다.

### `feature/*`

각자 실제 작업하는 브랜치이다.

예시:

```text
feature/vision-efficientnet
feature/rag-search
feature/llm-finetuning
feature/llm-quantization
feature/backend-api
feature/frontend-ui
```

기능 하나당 브랜치 하나를 원칙으로 한다.

---

# 2. 기본 작업 순서

## 작업 시작 전

항상 최신 `develop`을 기준으로 작업을 시작한다.

```bash
git switch develop
git pull origin develop
```

그다음 본인이 작업할 브랜치를 생성한다.

```bash
git switch -c feature/vision-efficientnet
```

이미 생성된 브랜치라면:

```bash
git switch feature/vision-efficientnet
```

---

# 3. 작업 후 Commit

작업이 어느 정도 완료되면 변경사항을 확인한다.

```bash
git status
```

변경된 파일을 추가한다.

```bash
git add .
```

Commit을 생성한다.

```bash
git commit -m "feat: EfficientNet 유물 분류 모델 추가"
```

원격 저장소에 업로드한다.

```bash
git push origin feature/vision-efficientnet
```

---

# 4. Commit Message 규칙

Commit 메시지는 아래 Prefix를 사용한다.

| Prefix | 의미 |
|---|---|
| `feat` | 새로운 기능 추가 |
| `fix` | 버그 수정 |
| `refactor` | 기능 변화 없는 코드 구조 개선 |
| `docs` | 문서 작성 및 수정 |
| `test` | 테스트 코드 및 테스트 관련 수정 |
| `chore` | 설정, 패키지, 환경 관련 작업 |
| `style` | 코드 포맷팅 및 스타일 수정 |

예시:

```text
feat: EfficientNet 유물 분류 모델 추가
feat: RAG 검색 기능 구현
feat: Qwen 4bit inference 추가
fix: 이미지 업로드 오류 수정
refactor: Vision inference 모듈 분리
docs: README 실행 방법 추가
chore: pyproject.toml 패키지 업데이트
```

Commit 메시지는 가능한 한 `무엇을 했는지`가 바로 보이도록 작성한다.

좋지 않은 예:

```text
수정
코드 변경
작업
123
```

---

# 5. Pull Request 규칙

작업이 완료되면 본인 브랜치에서 `develop` 브랜치로 PR을 생성한다.

```text
feature/*
    ↓
Pull Request
    ↓
develop
```

직접 `develop`에 merge하지 않는다.

최소 1명의 팀원이 코드를 확인한 뒤 merge한다.

## PR 제목 규칙

담당 영역을 앞에 표시한다.

```text
[Vision] EfficientNet baseline 추가
[RAG] FAISS retrieval 구현
[LLM] Qwen 4bit inference 추가
[BE] 유물 인식 API 구현
[FE] 유물 인식 결과 화면 구현
```

## PR 내용

PR에는 최소한 아래 내용을 작성한다.

```md
## 구현 내용

- EfficientNet-B0 기반 유물 분류 모델 구현
- 5개 클래스 inference 기능 추가

## 변경 파일

- ai/vision/train.py
- ai/vision/inference.py

## 테스트

- sample 이미지 5장 추론 테스트 완료
- artifact_id 반환 확인

## 확인 필요

- Backend에서 반환 JSON 형식 확인 필요
```

---

# 6. Merge 규칙

아래 조건을 만족한 경우에만 merge한다.

- 코드 실행에 문제가 없는지 확인
- 기존 기능이 깨지지 않는지 확인
- 최소 1명 이상 리뷰
- 공용 인터페이스 변경 여부 확인
- 충돌 해결 완료

본인이 만든 PR을 바로 본인이 merge하지 않는 것을 원칙으로 한다.

---

# 7. 충돌 Conflict 규칙

충돌이 발생했을 때 상대방의 코드를 임의로 삭제하지 않는다.

충돌이 발생하면 먼저 어떤 파일에서 충돌했는지 확인한다.

```bash
git status
```

다른 팀원이 담당한 코드와 충돌했다면 반드시 해당 담당자와 확인 후 수정한다.

특히 아래 파일은 단독 판단으로 수정하지 않는다.

```text
backend/main.py
pyproject.toml
package.json
공용 schema 파일
공용 API 파일
공용 config 파일
```

---

# 8. 작업 시작 전 최신 develop 반영

오래 작업한 브랜치는 PR 전에 최신 `develop` 내용을 반영한다.

```bash
git switch develop
git pull origin develop

git switch feature/본인브랜치
git merge develop
```

충돌이 발생하면 해결한 뒤 Commit한다.

```bash
git add .
git commit -m "chore: develop 브랜치 변경사항 반영"
```

---

# 9. 프로젝트 디렉터리 담당

기본 프로젝트 구조는 아래처럼 관리한다.

```text
museum-ai-docent/
│
├── frontend/
│   └── React
│
├── backend/
│   ├── main.py
│   ├── routers/
│   ├── schemas/
│   └── services/
│
├── ai/
│   ├── vision/
│   │   ├── train.py
│   │   └── inference.py
│   │
│   ├── rag/
│   │   ├── embedding.py
│   │   └── retrieval.py
│   │
│   └── llm/
│       ├── inference.py
│       ├── finetuning.py
│       └── prompts.py
│
├── data/
│   ├── raw/
│   └── processed/
│
├── docs/
│
├── .gitignore
├── README.md
└── pyproject.toml
```

각 담당자는 본인 영역을 중심으로 작업한다.

다른 파트의 파일을 수정해야 한다면 해당 담당자에게 먼저 공유한다.

---

# 10. 역할별 작업 영역

| 역할 | 주요 작업 디렉터리 |
|---|---|
| Vision AI | `ai/vision/` |
| RAG | `ai/rag/` |
| Local LLM | `ai/llm/` |
| Backend | `backend/` |
| Frontend | `frontend/` |
| 공통 데이터 | `data/` |
| 문서 | `docs/` |

---

# 11. 모델 및 데이터 파일 관리

대용량 모델과 데이터셋은 GitHub에 직접 업로드하지 않는다.

업로드 금지 예시:

```text
.pt
.pth
.bin
.gguf
.ckpt

대용량 이미지 데이터셋
3D 모델 원본
LLM 모델 파일
학습 checkpoint
```

`.gitignore` 예시:

```gitignore
# Python
.venv/
__pycache__/
*.pyc

# Environment
.env

# Dataset
data/raw/

# Models
models/
weights/
checkpoints/

*.pt
*.pth
*.bin
*.gguf
*.ckpt

# Frontend
node_modules/

# IDE
.vscode/
.idea/
```

모델과 데이터는 README에 다운로드 방법과 저장 경로를 작성한다.

예시:

```text
data/raw/artifacts/
models/vision/
models/llm/
```

---

# 12. `.env` 파일 관리

API Key, Token, 비밀번호는 절대 GitHub에 올리지 않는다.

예시:

```text
HUGGINGFACE_TOKEN=
MODEL_PATH=
VECTOR_DB_PATH=
```

실제 값이 들어간 `.env` 파일은 Git에서 제외한다.

대신 `.env.example` 파일을 만든다.

```env
HUGGINGFACE_TOKEN=
MODEL_PATH=
VECTOR_DB_PATH=
```

---

# 13. 공용 Interface 규칙

각 파트가 연결되는 데이터 형식은 마음대로 변경하지 않는다.

예를 들어 Vision 모델의 반환 형식을 아래처럼 결정했다면:

```json
{
  "artifact_id": "NMK_001",
  "artifact_name": "금동 반가사유상",
  "confidence": 0.94
}
```

Vision 담당자가 임의로 아래처럼 변경하지 않는다.

```json
{
  "label": "반가사유상",
  "score": 94
}
```

공용 데이터 구조를 수정해야 한다면 팀원과 먼저 협의한다.

---

# 14. 공용 Schema 관리

공용 데이터 형식은 문서로 관리한다.

예시:

```text
docs/
├── git_rules.md
├── api_spec.md
├── interface_spec.md
└── architecture.md
```

예를 들어 Vision 결과:

```json
{
  "artifact_id": "NMK_001",
  "artifact_name": "금동 반가사유상",
  "confidence": 0.94
}
```

RAG 결과:

```json
{
  "artifact_id": "NMK_001",
  "contexts": [
    {
      "text": "반가사유상은 ...",
      "source": "국립중앙박물관"
    }
  ]
}
```

LLM 결과:

```json
{
  "artifact_id": "NMK_001",
  "answer": "지금 보고 계신 유물은 ...",
  "sources": [
    "국립중앙박물관"
  ]
}
```

이런 데이터 구조를 먼저 합의한다.

---

# 15. 패키지 설치 규칙

새로운 Python 패키지를 설치했다면 반드시 프로젝트 의존성에도 반영한다.

`uv`를 사용하는 경우:

```bash
uv add 패키지명
```

예시:

```bash
uv add fastapi
uv add torch torchvision
uv add transformers
uv add sentence-transformers
```

임의로 `pip install`만 하고 끝내지 않는다.

Frontend도 동일하다.

```bash
npm install axios
```

를 했다면 `package.json` 변경사항도 Commit한다.

---

# 16. 하루 작업 시작 시

매일 작업을 시작할 때:

```bash
git switch develop
git pull origin develop
```

본인 브랜치에 최신 내용을 반영한다.

```bash
git switch feature/본인브랜치
git merge develop
```

그 이후 작업을 시작한다.

---

# 17. 하루 작업 종료 시

작업 종료 전 최소한 현재 코드를 Commit하고 Push한다.

```bash
git status

git add .

git commit -m "feat: 작업 내용"

git push origin feature/본인브랜치
```

작업 중간 상태라도 다른 팀원이 알아볼 수 있는 Commit 메시지를 사용한다.

---

# 18. 절대 하지 않는 것

아래 작업은 팀원 협의 없이 하지 않는다.

```text
main 직접 push

develop 직접 push

force push

git push --force

다른 사람 브랜치 삭제

다른 사람 코드 임의 삭제

공용 API Response 구조 변경

공용 DB Schema 임의 변경

대용량 모델 파일 GitHub 업로드

.env 업로드

검증하지 않은 코드 develop merge
```

특히 아래 명령어는 사용 전 반드시 팀원과 확인한다.

```bash
git push --force

git reset --hard

git clean -fd
```

---

# 19. 우리 팀 Git 작업 흐름 요약

```text
develop 최신화

        ↓

feature 브랜치 생성

        ↓

개별 기능 개발

        ↓

Commit

        ↓

Push

        ↓

Pull Request

        ↓

팀원 Review

        ↓

develop Merge

        ↓

통합 테스트

        ↓

안정 버전

        ↓

main Merge
```

---

# 20. 핵심 규칙 요약

> `main`은 배포용, `develop`은 통합용으로 사용한다.

> 모든 개발은 `feature/*` 브랜치에서 진행한다.

> `main`, `develop`에는 직접 push하지 않는다.

> 기능 개발 후 PR을 통해 `develop`에 병합한다.

> 최소 1명이 코드 확인 후 merge한다.

> 충돌 발생 시 상대방 코드를 임의로 삭제하지 않는다.

> 모델 파일과 대용량 데이터는 GitHub에 업로드하지 않는다.

> API, JSON Schema 등 공용 Interface는 합의 없이 변경하지 않는다.

> 작업 시작 전 최신 `develop`을 반영한다.

> 작업 종료 전 Commit과 Push를 진행한다.
