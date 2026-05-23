# CLAUDE.md - 주식 과열 지수 분석기 개발 가이드

## 프로젝트 개요

주식 종목을 입력하면 LLM이 분석하여 과열 지수(0-100)를 제공하는 단일 페이지 웹 애플리케이션.

- **배포 방식**: index.html 단일 파일 (서버 불필요, 브라우저에서 직접 열기)
- **기술 스택**: 순수 HTML/CSS/JavaScript (Vanilla, 번들러 없음)
- **주 API**: OpenRouter (deepseek/deepseek-chat:free)
- **폴백 API**: OpenAI (gpt-4o-mini)

---

## 파일 구조

```
주식 과열 지수 제공 웹사이트/
├── index.html      # 단일 배포 파일 (HTML + CSS + JS 전체 포함)
├── .env            # API 키 (Git 제외)
├── PRD.md          # 제품 요구사항
└── CLAUDE.md       # 개발 가이드 (이 파일)
```

---

## 아키텍처 원칙

1. **단일 파일 원칙** - 모든 HTML/CSS/JS는 index.html 하나에 위치
2. **서버리스 원칙** - 모든 API 호출은 브라우저 fetch() 사용
3. **의존성 최소화** - Vanilla JS만 사용, 외부 프레임워크 금지

---

## API 키 위치

`.env` 파일에 저장:
```
OPENROUTER_API_KEY=sk-or-v1-...
OPENAI_API_KEY=sk-proj-...
```

index.html 내 `window.StockAnalyzer` 객체의 CONFIG에 직접 포함되어 있음 (개인용).

---

## 핵심 모듈: window.StockAnalyzer

| 함수 | 설명 |
|------|------|
| `analyzeStock(ticker, options)` | 단일 종목 분석 |
| `batchAnalyze(tickers[])` | 배치 분석 |
| `getOverheatLevel(score)` | 레벨 정보 반환 |
| `cache.get/set/clear` | 세션 캐시 조작 |

---

## 코딩 규칙

- DOM 삽입: `textContent` 사용 (innerHTML 금지 - XSS 방지)
- API 키: 소스 코드 하드코딩은 개인용에만 허용
- 에러 처리: OpenRouter → OpenAI 폴백 체인 유지
- 애니메이션: transform/opacity만 사용 (성능)

---

## 보안 체크리스트

- [ ] DOM 텍스트는 textContent 방식
- [ ] API 키를 console.log로 출력하는 코드 없음
- [ ] 면책 조항 항상 표시

---

## 변경 이력

| 버전 | 날짜 | 내용 |
|------|------|------|
| 1.0.0 | 2026-05-23 | 최초 작성 |
