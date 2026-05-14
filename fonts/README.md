# Landing 폰트 드롭인 가이드

`@font-face`로 자체 호스팅 중인 폰트 파일들이 놓이는 폴더.

## 현재 배치된 폰트

| 파일명 | 서체 | 용도 |
|---|---|---|
| `Juana-RegularIt.otf` | Juana Regular Italic | 48px 이탤릭 디스플레이 제목 (After the sweat / The Beast Solution 등) |
| `HelveticaNeue-Condensed.ttf` | Helvetica Neue Condensed Regular (400) | 일반 본문/보조용 (현재 페이지에서 직접 사용되는 곳 없음) |
| `HelveticaNeue-BlackCond.otf` | Helvetica Neue Black Condensed (900) | 카드 제목 "SOOTHING / HYDRATION …" (40px) + "Heat spikes" 등 (24px) ✅ Figma 원본과 정확히 일치 |

## 추가 폰트 드롭인 방법

### 1. 폰트 파일을 이 폴더에 넣기
예: `HelveticaNeue-CondensedBlack.otf`, `HelveticaNeue-Bold.woff2` 등

### 2. `styles.css` 최상단에 `@font-face` 블록 추가

```css
@font-face {
  font-family: "Helvetica Neue Condensed";
  src: url("fonts/HelveticaNeue-CondensedBlack.otf") format("opentype");
  font-weight: 900;
  font-style: normal;
  font-display: swap;
}
```

### 3. 사용할 CSS 변수를 앞에 추가
```css
:root {
  --font-heading: "Helvetica Neue Condensed", "Archivo Black", Impact, sans-serif;
}
```

## 지원 포맷

`@font-face` src 문법으로 브라우저가 자동 선택:
- `.woff2` (권장, 웹 최적화) — `format("woff2")`
- `.woff` — `format("woff")`
- `.otf` — `format("opentype")`
- `.ttf` — `format("truetype")`

## Figma 원본 폰트 목록 (우선순위 순)

1. **Juana Regular Italic** — 48px 제목 ✅ 임베딩 완료
2. Helvetica Neue Condensed Black — 카테고리 카드 제목 (SOOTHING 등, 40px) ⚠️ Regular 임베딩됨 (Black 필요)
3. Helvetica Neue Bold — 버튼·내비 (17px/14px) ⏳ 시스템 Helvetica 폴백
4. Helvetica Neue Regular — 본문 (14px) ⏳ 시스템 Helvetica 폴백
5. SF Pro Display Light / Regular — 푸터 법적 문구 (12px) ⏳ Apple 기기는 시스템 폰트, 외 브라우저는 system-ui 폴백

추가가 필요하면 폰트 파일을 이 폴더에 넣고 알려주세요.
