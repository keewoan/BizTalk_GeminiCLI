# GEMINI.md — 프로젝트 지침 및 컨텍스트

이 파일은 **업무 말투 변환기(BizTalk)** 프로젝트의 구조, 기술 스택, 개발 원칙 및 실행 방법을 정의합니다. Gemini CLI는 이 지침을 기반으로 모든 개발 작업을 수행합니다.

---

## 1. 프로젝트 개요
- **목적**: 사용자가 입력한 메시지를 수신 대상(상사, 동료, 고객 등)에 맞는 적절한 업무 언어로 변환하는 서비스.
- **핵심 기술**:
    - **Back-end**: Python 3.11+, FastAPI, LangChain
    - **AI Model**: Upstage Solar-Pro3 (`langchain-upstage` 활용)
    - **Front-end**: Vanilla HTML, CSS, JavaScript
    - **Deployment**: Vercel

---

## 2. 개발 및 실행 지침

### 2-1. 환경 설정 및 실행
- **사전 요구사항**: Python 3.11 이상, Upstage API Key (`.env` 관리)
- **의존성 설치**:
    ```bash
    pip install fastapi uvicorn langchain python-dotenv langchain-upstage
    ```
- **서버 실행**:
    ```bash
    cd backend
    uvicorn main:app --reload --port 8000
    ```

### 2-2. 디렉토리 구조
프로젝트는 다음과 같은 표준 구조를 유지해야 합니다.
```
biztalk_gemini-cli/
├── backend/
│   ├── main.py                 # FastAPI 앱 설정
│   ├── routers/                # API 라우터 (convert.py)
│   ├── services/               # LLM 연동 로직 (tone_converter.py)
│   ├── prompts/                # 대상별 프롬프트 (templates.py)
│   ├── models/                 # Pydantic 모델 (schemas.py)
│   └── requirements.txt
├── frontend/
│   ├── index.html
│   ├── css/ (style.css)
│   └── js/ (app.js)
├── .env                        # API 키 (UPSTAGE_API_KEY)
└── PRD_업무말투변환기.md
```

---

## 3. 바이브 코딩(Vibe Coding) 3원칙
모든 작업 시 다음 원칙을 엄격히 준수합니다.

1.  **완료 기준 정의**: 작업을 시작하기 전 "무엇을 구현하면 끝인지" 체크리스트를 명확히 합니다.
2.  **조사 먼저, 구현 나중**: 새로운 라이브러리나 API 연동 시 문서를 먼저 확인하고 방법을 확정한 뒤 코드를 작성합니다.
3.  **버그 분석 우선**: 에러 발생 시 즉각적인 수정 대신 원인 분석(Root Cause Analysis)을 먼저 수행하고 보고합니다.

---

## 4. 주요 API 명세

### `POST /api/convert`
- **Request**:
    ```json
    { "text": "원문", "target_audience": "boss | colleague | client | team" }
    ```
- **Response**:
    ```json
    { "converted_text": "변환문", "target_audience": "...", "original_text": "..." }
    ```

---

## 5. 단계별 구현 로드맵
- **STEP 1**: 환경 준비 (디렉토리 생성, API 키 설정, 의존성 명시)
- **STEP 2**: 백엔드 구현 (데이터 모델 -> 프롬프트 -> LLM 연동 -> 라우터 -> 메인 앱)
- **STEP 3**: 프론트엔드 구현 (UI 레이아웃 -> 스타일링 -> JS 연동)
- **STEP 4**: 배포 (GitHub 푸시 -> Vercel 배포)

---

> **Gemini CLI 참고**: 작업 진행 중 모호한 사항은 `PRD_업무말투변환기.md`를 최우선으로 참고하십시오.

### @PRD_업무말투변환기.md 문서와 GEMINI.md 문서 항상 최신화 하기
* 모든 변경사항이 발생하면 (예를 들어 Source Code가 변경 되거나 라이브러리 버전이 변경되면) md 문서도 반드시 업데이트 합니다. 
* 구현이 완료된 사항들은 `2. 완료 체크리스트`에 모두 체크표시를 해서 완료 되었음을 반드시 표시하세요.
* `8. 단계별 구현 순서` 에서도 STEP별로 구현이 완료되면 체크표시를 해서 완료 되었음을 반드시 표시하세요.

