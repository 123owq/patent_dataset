# 협업 가이드

## 브랜치 전략

- `main` 직접 push 금지 — **PR 필수**
- 브랜치명: `feature/이슈번호-간단설명`
  - 예: `feature/12-oa-parser`, `feature/7-tool2-dataset`

---

## 작업 시작 전 — Issue 먼저

작업을 시작하거나 할 일이 생기면 반드시 **GitHub Issue를 먼저 등록**하고 시작합니다.

1. Issues 탭 → **New Issue**
2. 제목: 작업 내용 한 줄 요약
3. 본문: 배경, 목표, 참고사항 간단히 작성
4. 본인 **Assignee** 지정
5. 이슈 번호 확인 후 브랜치 생성

```bash
git checkout -b feature/이슈번호-간단설명
```

---

## PR 규칙

- 본문에 관련 이슈 연결: `Closes #이슈번호`
- 셀프 머지 금지 — 리뷰어 최소 1명 지정
- main 머지는 리뷰 승인 후 진행

---

## 환경 세팅

```bash
uv sync
cp .env.example .env
# .env에 OPENROUTER_API_KEY 입력
```
