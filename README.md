# McEZR

마인크래프트 도시 개발을 돕는 **게임 외부 정보 처리 도구**입니다.
현실적인 도시를 만들려는 플레이어가, 도로·필지·건물·지형 데이터를
게임 밖에서 설계하고 관리할 수 있게 합니다.

> 한 줄 요약: GIMP처럼 그리되, 내부는 객체·속성·공간 정보를 유지하는 도시 정보 설계 플랫폼.

---

## 지금 바로 써보기

GitHub Pages: **(여기에 Pages 주소를 넣으세요)**

또는 저장소를 받아 `index.html`을 브라우저로 엽니다.

---

## 모듈

McEZR는 네 개의 모듈로 이루어집니다. 지금은 **McRAD**를 개발 중입니다.

| 모듈 | 이름 | 역할 | 상태 |
|------|------|------|------|
| **McRAD** | Road & Area Data | 도로 설계 | 개발 중 (v0.2.0) |
| McBIS | Block & Infrastructure | 블록·필지·건물 | 예정 |
| McMIS | Monument Information | 분석·규제 | 예정 |
| McHIS | Height & Infrastructure | 지형·높이 | 예정 |

---

## 문서 안내

처음 오셨다면 이 순서로 읽으세요.

1. **[spec_McEZR.md](spec_McEZR.md)** — 프로젝트 전체의 철학과 공통 구조. **가장 먼저 읽으세요.**
2. **[spec_McRAD.md](spec_McRAD.md)** — McRAD(도로 모듈) 상세 명세.
3. **[tasks_McRAD.md](tasks_McRAD.md)** — McRAD 할 일 목록과 진행 상황.
4. **[done_McRAD.md](done_McRAD.md)** — 완료된 작업 기록.

---

## 역할

| 역할 | 담당 | 하는 일 | 적는 곳 |
|------|------|---------|---------|
| 개발자 | (본인) | 코드 작성, 설계 결정 | `tasks_*`, `done_*` |
| 검증자 | (동료) | 테스트, 버그 보고, 사용성 피드백 | `feedback_McRAD.md` |

검증자는 [feedback_McRAD.md](feedback_McRAD.md)에 양식대로 적어주시면 됩니다.

---

## 파일 구조

```
README.md            이 파일
index.html           허브 페이지 (Pages 연결)
index_McRAD.html     McRAD 에디터
index_McBIS.html     McBIS 에디터 (준비 중)

spec_McEZR.md        공통 철학·구조 (먼저 읽기)
spec_McRAD.md        McRAD 명세
spec_McBIS.md        McBIS 명세 (뼈대)

tasks_McRAD.md       McRAD 할 일
tasks_McBIS.md       McBIS 할 일 (뼈대)
done_McRAD.md        McRAD 완료 기록
feedback_McRAD.md    검증자 피드백
```

---

## 원칙 (자세한 내용은 spec_McEZR.md)

- 화면은 GIMP처럼, 내부는 객체 기반.
- 시각 정보 없이도 데이터를 편집할 수 있어야 한다.
- 좌표는 내부에서 실수로 저장, 출력할 때만 마인크래프트 블록 정수로 변환.
- 되돌리기 비싼 것은 미리 정의하고, 싼 것은 만들며 정한다.
