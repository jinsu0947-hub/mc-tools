# mc-tools

🔹 0. 전체 구조 (완성형 워크플로우)

앞으로 너는 이 구조로만 개발한다:

[너] 기능 정의
   ↓
[ChatGPT] 기능 구조 정리 (명확화)
   ↓
[Gemini] 전체 코드 생성
   ↓
[VS Code] 코드 반영
   ↓
[브라우저 실행]
   ↓
에러 발생
   ↓
[ChatGPT] 원인 분석
   ↓
[Gemini] 수정 코드 생성
   ↓
[GitHub] 커밋
   ↓
반복

👉 핵심:

ChatGPT = “두뇌”
Gemini = “코드 생산기”
GitHub = “기록 + 복구 장치”
🔹 1. 환경 세팅 (최종 형태)
✔ 필수 구성
① Visual Studio Code
코드 작성 / Git 연동
② Git + GitHub
코드 저장
버전 관리
“되돌리기” 핵심
③ Gemini
코드 생성 전담
④ Google Chrome 또는 Edge
실행 + DevTools

👉 여기까지가 “완성형 최소 세트”

🔹 2. GitHub 운영 규칙 (이게 핵심이다)

이걸 안 하면 무조건 다시 무너진다.

✔ 커밋 규칙 (강제)

👉 기능 단위로만 커밋

예:

feat: add node creation
fix: click event bug
refactor: canvas render cleanup
✔ 절대 금지

❌ 여러 기능 한 번에 커밋
❌ “작업중” 상태 커밋
❌ 설명 없는 커밋

✔ 브랜치 (초기엔 단순화)

👉 처음엔 하나만 쓴다:

main

👉 나중에:

feature/*
🔹 3. 역할 분리 (중요)
🔸 너
기능 정의
실행
판단
🔸 ChatGPT
기능을 “명확한 요구사항”으로 변환
디버깅
🔸 Gemini
코드 생성
코드 수정

👉 이 구조 깨지면 효율 급락

🔹 4. 실제 작업 흐름 (한 사이클)
Step 1. 너 (기능 정의)

예:

클릭하면 Node 생성

Step 2. ChatGPT (정제)

출력 예:

기능:
- canvas 클릭 시 좌표 저장
- nodes 배열에 추가
- 화면에 원으로 표시

👉 이걸 Gemini로 넘김

Step 3. Gemini (코드 생성)

프롬프트:

canvas 기반 JS 코드가 있다.

기능:
- 클릭하면 원 생성
- nodes 배열 관리
- 화면에 렌더링

조건:
- 전체 코드로 작성
- 실행 가능하게
Step 4. VS Code
붙여넣기
저장
Step 5. 실행
브라우저 열기
F12 → Console 확인
Step 6. 문제 발생 시

👉 ChatGPT:

에러:
(내용)

코드:
(전체 코드)

원인 + 수정 방법만 설명

👉 Gemini:

수정된 전체 코드 출력
Step 7. GitHub 커밋
feat: node creation working

👉 이게 “한 사이클”

🔹 5. 자동화 수준 (현실 최대치)

현재 구조에서:

단계	자동화
코드 생성	AI
코드 수정	AI
실행	수동
커밋	수동

👉 체감 자동화: 70~80%

🔹 6. 품질 유지 장치 (이게 핵심)
✔ 규칙 1

👉 항상 “전체 코드” 기준

✔ 규칙 2

👉 기능 완성 → 커밋 → 다음 기능

✔ 규칙 3

👉 “동작 확인 전 절대 다음 단계 금지”
