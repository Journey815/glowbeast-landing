# Glowbeast Landing — Handoff Doc

*Updated: 2026-05-13 · Version: **v1.4 (Figma 3089:2 전체 재sync)***

이 문서를 처음 보시나요? 5분만 읽으시면 즉시 작업 시작 가능합니다.

---

## 1. 프로젝트 개요

| 항목 | 값 |
|---|---|
| 클라이언트 | **Glowbeast** — 땀 흘리는 사람들을 위한 액티브 뷰티 브랜드 |
| 산출물 | 모바일 웹 랜딩 사이트 (2 페이지) |
| 기반 디자인 | Figma `h0efHuovfV7mG9ZQINaalO` node `3089:2` |
| 기술 스택 | Vanilla HTML + CSS + JS (빌드 도구 없음) |
| 의존성 | **없음** — npm install 등 불필요 |
| 베이스 뷰포트 | 모바일 402px → 데스크톱은 max-width 480px 중앙 정렬 |

---

## 2. 1분 시작하기

### 그냥 보기만 할 때
1. 압축 해제
2. `landing/index.html` 더블클릭 → 기본 브라우저에서 열림 — 끝.

### 로컬 서버로 보고 싶을 때 (권장 — 폰트·CORS 안정)
```bash
cd landing/
python3 -m http.server 8000
# 또는 Node.js 사용 시: npx serve .
```
브라우저에서 `http://localhost:8000/` 접속.

---

## 3. 폴더 구조

```
landing/
├── index.html         # 메인 랜딩 페이지 (8 섹션)
├── lineup.html        # 라인업 페이지 (스켈레톤 — 본문 구현 대기)
├── styles.css         # 전체 스타일 (디자인 토큰 + 컴포넌트)
├── HANDOFF.md         # 이 문서
├── motion-plan.md     # 인터랙션/애니메이션 기획안 (보류 중, 참고용)
├── assets/            # 이미지 21개 + README.md (Figma 노드 매핑)
│   ├── hero-bg.png
│   ├── logo.svg
│   ├── category-1.png ~ category-6.png
│   ├── meet-1.png ~ meet-4.png
│   └── ... (총 20개)
└── fonts/             # 자체 호스팅 폰트 3종 + README.md
    ├── Juana-RegularIt.otf            (Juana Regular Italic)
    ├── HelveticaNeue-Condensed.ttf    (Regular 400)
    └── HelveticaNeue-BlackCond.otf    (Black 900)
```

---

## 4. 페이지 구조 — 6 섹션 (`index.html`)

| # | 섹션 | 클래스 | 배경 | 비고 |
|---|---|---|---|---|
| 1 | Hero | `.hero` | 풀블리드 이미지 | iPhone status bar + 워드마크 |
| 2 | After the sweat: | `.after-sweat` | 검정 | sticky stacking 4카드 |
| 3 | That's where we start: | `.mission` | 흰색 | 미션 메시지 |
| 4 | The Beast Solution: | `.solution` | 검정 | 가로 스와이프 6카드 + `INSIDE THE LINEUP` CTA |
| 5 | Hey Beast there, | `.signup-section` | 검정 | 이메일 입력 + `GET FIRST ACCESS` 노란 CTA |
| 6 | Footer | `.site-footer` | 검정 | 로고 + 이메일 / 메뉴·주소 / 카피·소셜 (폼 없음) |

`lineup.html`은 동일 헤더·푸터 + 빈 본문(coming-soon placeholder).

> v1.2 변경 요약: "Until now..." CTA 블록은 4번 솔루션 CTA로 통합, "Meet The Beasts" 섹션 제거, "Hey Beast there"는 푸터 폼에서 분리되어 풀섹션으로 승격, 푸터 단순화 (폼 제거 → 단일 메일 링크).

---

## 5. 디자이너 작업 가이드

### 이미지 교체
- **위치**: `landing/assets/`
- **파일명 ↔ Figma 노드 매핑**: `assets/README.md` 참조
- **규칙**:
  - 파일명을 README의 표와 정확히 일치시켜 저장 (대소문자·확장자 포함)
  - PNG로 내보내도 OK (HTML이 .png 참조 중)
  - 2x 배율 권장 (Retina 대응)
- **실수 방지**: Figma의 원본 프레임 이름이 `categoty-` 오타일 수 있음 → 저장 시 `category-`로 수정

### 폰트 교체/추가
- **위치**: `landing/fonts/`
- **방법**: `fonts/README.md` 참조 — `@font-face` 패턴 동일
- **현재 임베딩**: Juana Regular Italic, Helvetica Neue Condensed Regular/Black

### 색상·간격 토큰
`styles.css` 최상단 `:root` 블록:
```css
--accent-yellow: #ffff33;
--bg-black: #000;
--bg-white: #fff;
--max-w: 480px;        /* 데스크톱 최대 너비 */
--gutter: 16px;        /* 좌우 패딩 */
--section-pad-y: 120px;
```

### Figma 원본
https://www.figma.com/design/h0efHuovfV7mG9ZQINaalO/glowbeast?node-id=3089-2

---

## 6. 개발자 작업 가이드

### 주요 코드 영역
| 위치 | 내용 |
|---|---|
| `styles.css:6-27` | `@font-face` (Juana + Helvetica Neue 2종) |
| `styles.css:36-50` | 디자인 토큰 (`:root`) |
| `index.html:25-50` | iPhone status bar 모크업 (인라인 SVG) |
| `index.html` 최하단 `<script>` | 가로 드래그 스크롤 (vanilla JS, 약 22줄) |

### 반응형 정책
- 모바일 우선 (Figma 402px 베이스)
- 데스크톱: `body { max-width: 480px; margin: 0 auto; }` — 중앙 정렬, 주변 검정 배경
- 가로 카드 레일 (`.scroll-x`): 우측 풀블리드 (`margin-right: -16px` 트릭)

### 페이지간 연결
- `index → lineup`: 3 곳 (상단 nav `LINEUP`, 솔루션 CTA `INSIDE THE LINEUP`, footer `LINEUP`)
- `lineup → index`: 4 곳 (로고, HOME, back-link, footer `ABOUT`)

### 가로 드래그 스크롤 동작
- 클래스 `.scroll-x` → 자동 적용
- 마우스 클릭+드래그 / 터치 스와이프 / 트랙패드 모두 지원
- 카드 단위 snap (`scroll-snap-type: x mandatory`)

---

## 7. 다음 작업 후보 (우선순위 순)

### 🔥 즉시 가능
1. **`lineup.html` 본문 구현** — 현재 스켈레톤만, Figma에서 라인업 페이지 디자인 확정 후
2. **`hero-bg.png` 최적화** — 현재 4.8 MB, 1 MB 이하로 압축 (TinyPNG/Squoosh)
3. **Signup 섹션 액션 연결** — `index.html`의 폼 `action="#"`과 `GET FIRST ACCESS` `href="#"`를 실제 엔드포인트로 (Mailchimp/Stibee 등)
4. **신규 카테고리·푸터 이미지 export** — `assets/README.md` 체크리스트 참조 (hero-bg / category-1~6 / footer-logo / 4 stack-card 이미지)
5. **소셜 링크** — 푸터 Instagram/Amazon `href="#"` → 실제 URL

### 🎨 디자인 작업
- Page 2 (라인업) 와이어프레임/디자인 확정
- 이메일 폼 성공/실패 상태 디자인

### 💻 개발 작업
- 메타 태그 보강 (OG 이미지, Twitter Card)
- 실제 도메인 배포 (GitHub Pages / Netlify / Vercel — 정적이라 어디든 OK)
- analytics 삽입 (GA4 등)

### ✋ 보류 중 (재논의 필요)
- **모션/애니메이션 레이어** — v1.1에서 GSAP+Lenis로 시도 후 사용성 저하로 롤백.
  기획안은 `motion-plan.md`에 보존. 다음에 시도 시 더 절제된 마이크로 인터랙션 위주 권장 (Lenis 관성 스크롤 X, 카드 호버만 등)

---

## 8. 알려진 이슈

| 이슈 | 비고 |
|---|---|
| `wordmark-glow.png`, `mission-wordmark.png` 미참조 | Hero·Mission 워드마크가 `logo.svg`로 대체됨. 삭제해도 무방 |
| iPhone status bar 모크업 | 실제 모바일에서 OS status bar와 겹쳐 보일 수 있음 → 실배포 전 검토 |
| Figma 원본 프레임명 오타 | "categoty-*" → 코드는 "category-*"로 정정됨. 다시 export 시 주의 |
| 데스크톱 가로 드래그 후 클릭 충돌 방지 | 이미 처리됨 (`moved` 플래그 + click 차단) |

---

## 9. 외부 링크 & 자료

- **Figma 원본**: https://www.figma.com/design/h0efHuovfV7mG9ZQINaalO/glowbeast?node-id=3089-2
- **모션 레퍼런스 (보류 중)**: https://bennettandclive.com/
- **GSAP Docs (모션 재시도 시)**: https://gsap.com/docs/
- **Lenis Docs**: https://lenis.darkroom.engineering/

---

## 10. 변경 이력

| 날짜 | 버전 | 내용 |
|---|---|---|
| 2026-04-23 | v1.0 베이스 | 8 섹션 정적 사이트 완성, 폰트 임베딩, 가로 드래그 |
| 2026-04-24 | v1.1 모션 시도→롤백 | GSAP+Lenis 적용 후 사용성 저하로 v1.0으로 복귀 |
| 2026-04-27 | 핸드오프 | HANDOFF.md 작성 + zip 패키징 |
| 2026-04-29 | After-the-sweat 리뉴얼 | 가로 스크롤 2카드 → sticky stacking 4카드 (Figma 3089:49) |
| 2026-05-08 | v1.2 Figma 3089:2 sync | Meet 섹션 제거, Hey Beast there 별도 섹션 분리, 푸터 단순화(폼 제거), 스택 카드 카피 갱신 |
| 2026-05-11 | v1.3 마이크로 폴리싱 | Hero drop 스냅 튜닝(1.0s/1.4s, easeIn), 워드마크 yellow 보정 + BEAST z:4(모델 앞), chevron bob, btn hover wipe, nav 밑줄 sweep, input focus 강화, 소셜 hover, 푸터 로고 IO 진입 |
| 2026-05-13 | v1.4 Figma 재sync | Nav `ABOUT` 추가, Hero drop 1st 작아짐(262×61, 우측), Status/Nav backdrop-blur, After-the-sweat 32px·#777/#999 톤, Solution 카드 한글 부제목(Pretendard) |

상세 진행 로그는 부모 폴더의 `clients/glowbeast/progress.md` 참조.

---

질문 있으시면 전지연 (wuswus2@gmail.com)으로 연락주세요.
