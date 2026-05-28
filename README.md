# 🛒 쇼핑 리스트 앱

로컬 브라우저에서 동작하는 단일 파일 쇼핑 리스트 웹 앱. 외부 라이브러리/프레임워크 없이 순수 HTML/CSS/Vanilla JS로 작성되어 있으며, 데이터는 브라우저 `localStorage` 에 자동 저장됩니다.

## 실행 방법

`shopping-list.html` 을 더블클릭해 기본 브라우저로 열면 됩니다. 별도 설치/빌드 단계 없음.

## 주요 기능

- 아이템 추가 (`추가` 버튼 / Enter 키)
- 아이템 체크 / 체크 해제 (취소선 + 완료 카운터)
- 단일 아이템 삭제 (`✕` 버튼)
- 완료 항목 일괄 삭제 (confirm 다이얼로그)
- 총 개수 / 완료 개수 실시간 표시
- 새로고침 후에도 상태 유지 (localStorage)

## 사용된 최신 패턴

- `crypto.randomUUID()` 로 충돌 없는 ID 생성
- `localStorage.setItem()` 에 `QuotaExceededError` 방어 (try/catch)
- `JSON.stringify` / `JSON.parse` 직렬화 (MDN 권장 패턴)

## Playwright MCP 자동 테스트 결과

`2026-05-28` 기준, Playwright MCP 로 실제 브라우저에서 전 기능 점검 — **전 항목 통과**.

| # | 항목 | 시나리오 | 결과 |
|---|---|---|---|
| 1 | 초기 로드 | 빈 상태, 카운터 0/0, 안내 문구 표시 | ✅ |
| 2 | 버튼으로 추가 | "사과" 입력 후 `추가` 클릭 | ✅ 리스트에 추가, 입력란 자동 초기화 |
| 3 | Enter 로 추가 | "바나나", "우유" 입력 후 Enter | ✅ 각각 추가, 총 3개 |
| 3a | 공백 가드 | 공백만 입력 후 Enter | ✅ 추가되지 않음 (trim 동작) |
| 4 | 체크 토글 | 사과 체크박스 클릭 → 다시 클릭 | ✅ `.checked` 클래스 토글, 카운터 동기화 |
| 4a | 다중 체크 | 사과 + 바나나 체크 | ✅ 완료 2개 |
| 5 | 단일 삭제 | "우유"의 `✕` 클릭 | ✅ 우유만 제거, 총 2개로 갱신 |
| 6 | 일괄 삭제 + 다이얼로그 | 미체크 "딸기" 추가 → `완료 항목 모두 삭제` 클릭 → confirm 수락 | ✅ 체크된 사과/바나나 제거, 딸기 잔존 |
| 7 | localStorage 직렬화 | `localStorage.getItem('shopping-list-items')` 검사 | ✅ UUID 형식 ID 저장 확인 |
| 7a | 새로고침 복원 | 페이지 reload 후 상태 확인 | ✅ "딸기" 복원 |

## 파일 구성

```
shopping-list.html       단일 파일 앱 (HTML + CSS + JS)
README.md                이 문서
```
