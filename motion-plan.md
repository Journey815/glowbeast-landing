# Glowbeast Landing — Interaction & Animation Plan v1

*작성: 2026-04-24 · 기준: Figma node `2120:2` + Bennett&Clive 참조*

## 0. 레퍼런스 분석 (bennettandclive.com)

Awwwards 7.71/10, CSS Design Awards WOTY 2024 후보. 뷰티·패션·럭셔리 특화 스튜디오라 Glowbeast(액티브 뷰티)와 장르 매칭.

**관찰된 테크 스택** (Nuxt 번들 분석):
- GSAP 3 (핵심 애니메이션 엔진)
- ScrollTrigger (스크롤 연동)
- ScrollSmoother
- Lenis (부드러운 관성 스크롤)

**핵심 컨셉**: "앰퍼샌드(&) 기호가 페이지 전체를 관통하며 지나가는 요소들과 상호작용" — 하나의 브랜드 시그니처가 모션의 leitmotif(회귀 모티프) 역할.

## 1. Glowbeast 적용 설계 원칙

### 시그니처 컨셉: "The Beast Thread"
Bennett&Clive의 `&` 모티프를 Glowbeast의 **`BEAST` 워드마크**로 치환. 페이지를 관통하며 4곳에서 등장 (Hero → Mission → Until now CTA → Footer) — 각 등장마다 크기·색·맥락이 달라지며 **동일 모션 서명**(scale-in + pulse)으로 연결성 부여.

### 3원칙
1. **모바일 우선** — 터치·perf·`prefers-reduced-motion` 준수
2. **브랜드 결**: 땀 흘리는 액티브 · 강한 에너지 → **스냅 있는 타이밍** (200–500ms 주류, 800ms는 특별 순간만)
3. **페이지 = 스크롤 타임라인** — 사용자가 스크롤할 때마다 "다음 장면"이 트리거 (영화적 리듬)

## 2. 섹션별 인터랙션

### §0 페이지 로드 & 글로벌
| 요소 | 동작 |
|---|---|
| 스크롤 | Lenis 관성 스크롤 (duration 1.2s, easeOut cubic) |
| 초기 fadeIn | body opacity 0→1 (400ms) |
| 로딩 | 폰트 swap 완료 후 본문 표시 (FOIT 방지) |
| Reduced motion | 모든 스크롤 애니메이션 비활성, fadeIn만 100ms로 |

### §1 Hero
| 요소 | Trigger | 동작 |
|---|---|---|
| 배경 이미지 | 로드 직후 | `scale: 1.08 → 1.0`, 1200ms cubic-bezier(.2,.8,.2,1) |
| 상단 status bar | 로드 50ms | opacity 0→1 (300ms) |
| 내비 바 | 로드 200ms | translateY(-10px)→0 + opacity 0→1 (400ms) |
| "glow like a beast" 워드마크 | 로드 600ms | scale 0.85→1.0 + opacity 0→1, easeOutBack, 900ms |
| ∨ chevron | 반복 | translateY ±5px, 2.4s 사이클 (스크롤 유도) |
| Hero bg on scroll | 스크롤 | parallax 0.82x (배경이 느리게 움직임) |

### §2 "After the sweat:" (white)
| 요소 | Trigger | 동작 |
|---|---|---|
| 제목 `After the sweat:` | 뷰포트 30% 진입 | SplitType 글자 단위 stagger, 아래에서 올라오며 blur 6px→0, 글자당 30ms, 전체 800ms |
| 카드 | 뷰포트 40% 진입 | 우측에서 translateX(60px)→0 + opacity 0→1, 600ms, 카드간 150ms stagger |
| 카드 이미지 | 카드와 함께 | clip-path mask reveal (위→아래), 900ms |
| **가로 드래그 중** | 사용자 드래그 | 이미 구현된 drag-to-scroll + snap. 추가: **드래그 시 다음 카드 살짝 더 peek** (-30px offset during grabbing) |

### §3 "That's where we start:" (white)
| 요소 | Trigger | 동작 |
|---|---|---|
| 제목 | 30% 진입 | SplitType (같은 스타일) |
| **Mission 워드마크** (logo.svg 검정) | 40% 진입 | scale 0→1 + easeOutBack, 1000ms — **Beast Thread 1회차** |
| 미션 문구 `is active beauty…` | 워드마크 완료 직후 | 단어 단위 stagger, blur 4px→0, 단어당 80ms |
| 하단 신념문 3줄 | 추가 스크롤 | 줄 단위 stagger, 위로 20px + fadeIn, 줄당 120ms |

### §4 "The Beast Solution:" (black)
| 요소 | Trigger | 동작 |
|---|---|---|
| 제목 | 30% 진입 | SplitType (흰색) |
| 6카드 레일 | 레일이 뷰포트 진입 | 카드들이 우측에서 순차 슬라이드 인, 카드당 80ms stagger, 개별 500ms |
| **카드 스냅 시** | 스냅 완료 이벤트 | 스냅된 카드 제목 scale 1.0 → 1.03 → 1.0 (300ms) — "착륙" 피드백 |
| Desktop hover | 마우스 호버 | 배경 이미지 scale 1.05, 제목 순간 yellow flash (180ms) |
| 드래그 parallax | 드래그 중 | 성분 텍스트(하단)가 제목(중앙)보다 10% 느리게 따라옴 — 레이어드 깊이감 |

### §5 "Until now..." → INSIDE THE LINEUP (black, CTA)
**핵심 클라이맥스 — ScrollTrigger pin 적용**

섹션을 스크롤이 들어오면 1스크린 만큼 고정(pin)하고, 그 시간 동안 내부 타임라인 재생:

| t | 요소 | 동작 |
|---|---|---|
| 0.0s | `Until now, there was no beauty for Beasts` | 줄 단위 fadeUp (각 300ms, 200ms stagger) |
| 0.6s | 노란 `glow like a` | 위에서 드롭 + bounce settle, 500ms |
| 0.8s | **BEAST 일러스트** | scale 0.6→1.0 + 노란 glow filter (drop-shadow) 순간 폭발 → 안정, 700ms — **Beast Thread 2회차** |
| 1.3s | 노란 `INSIDE THE LINEUP` 버튼 | 배경 색 채우기(좌→우 wipe) + 텍스트 fadeIn, 600ms |
| — | 버튼 hover | 배경 yellow→black, 텍스트 black→yellow (200ms), 바깥 테두리 glow |
| — | 버튼 클릭 | **페이지 전환**: 노란색 원형 확장 트랜지션(400ms) → `lineup.html` fadeIn |

### §6 "Meet The Beasts:" (black)
| 요소 | Trigger | 동작 |
|---|---|---|
| 제목 | 30% 진입 | SplitType |
| **4타일 복합 그리드** | 그리드 진입 | 각 타일 다른 방향에서 진입 — t1 좌측, t2 하단, t3 상단, t4 우측 (에너지·놀이감) + 각 scale 0.9→1.0, 600ms, 타일당 100ms stagger |
| 타일 idle (진입 후) | 무한 루프 | 각자 다른 phase로 translateY ±3px, 4.5s cycle (숨 쉬는 듯) |
| 본문 3줄 | 타일 완료 후 | 줄 stagger (같은 패턴) |

### §7 "Hey Beast there," — 이메일 신청
| 요소 | Trigger | 동작 |
|---|---|---|
| 제목 | 30% 진입 | SplitType |
| 본문 리드 | 추가 스크롤 | fadeUp |
| 이메일 입력 focus | 포커스 | border white→yellow, caret yellow (이미 부분 구현) |
| `GET FIRST ACCESS` 버튼 | hover | §5와 동일 wipe 패턴 |
| Submit 성공 | 클릭 + 검증 통과 | ●●● 노란 닷 버스트 (Lottie 또는 canvas), 1s 페이드아웃 |

### §8 Footer
| 요소 | Trigger | 동작 |
|---|---|---|
| **Footer 대형 로고** | 푸터 진입 | scale 0.95→1.0 + fadeIn, 800ms — **Beast Thread 3회차** |
| 푸터 나머지 | 로고 다음 | 기본 fadeUp |
| 소셜 아이콘 hover | hover | rotate 8° + scale 1.15 (200ms) |

## 3. 글로벌 네비게이션 동작
- **Nav 자동 숨김**: 200px 이상 스크롤 다운 시 fadeOut (-10px), 스크롤 업 5px 이상 시 fadeIn
- **Nav 링크 hover**: `LINEUP` 아래 yellow 밑줄이 왼쪽→오른쪽으로 sweep (250ms, scaleX origin-left)
- **Status bar**: 고정 노출 (hero 한정)

## 4. 선택 옵션 (Premium Add-on)
| 아이템 | 설명 | 영향 |
|---|---|---|
| **커스텀 커서** | 데스크톱만, 작은 검정 닷 + hover 시 확장. `mix-blend-mode: difference`로 항상 가독 | 프리미엄 감 +, 모바일 영향 0 |
| **Lottie 마이크로** | 신청 성공 confetti, 버튼 wipe 세부 등 | 용량 +5–20KB |
| **페이지 전환** | index ↔ lineup 간 노란 원형 확장 | 브랜드감 +, JS 로직 필요 |
| **사운드 토글** | hover 시 subtle click sfx | 호불호 갈림, OFF 기본 |

## 5. 테크 스택 제안

### Core (필수)
- **GSAP 3** (무료 tier) — 타임라인·tween 엔진
- **ScrollTrigger** (GSAP 무료 플러그인) — 스크롤 연동
- **Lenis** (MIT) — 부드러운 관성 스크롤

### Optional
- **SplitType** (무료, GSAP SplitText의 대안) — 글자·단어 분할
- **Lottie Web** — confetti·마이크로 애니메이션

### 총 bundle 비용
- 필수 3종 gzipped: ~55KB
- Full stack (Lottie + SplitType 포함): ~85KB

### 기존 코드 영향
- `styles.css`: `prefers-reduced-motion` 쿼리 추가, 일부 초기 `opacity: 0` 스타일 추가
- `index.html`: `<script type="module">` 1개 추가 (GSAP 초기화)
- 기존 drag-to-scroll JS와 충돌 없음 (별도 이벤트 네임스페이스)

## 6. 구현 단계 (Phase 제안)

### Phase 1 — 필수 폴리싱 (1–2일)
- [ ] Lenis 부드러운 스크롤
- [ ] SplitType 6개 섹션 제목 reveal
- [ ] 본문 fadeUp 일괄 적용
- [ ] 버튼 hover state 전체
- [ ] Nav 스크롤 숨김/복귀
- [ ] Hero 로드 시퀀스 (bg zoom + 워드마크 scale-in)
- [ ] `prefers-reduced-motion` 준수

### Phase 2 — 브랜드 퍼스널리티 (2–3일)
- [ ] "Until now..." 핀-기반 타임라인 클라이맥스
- [ ] Meet The Beasts 4방향 진입 + idle 루프
- [ ] Beast Thread 3회차 pulse 동기화
- [ ] 카드 스냅 bump 피드백
- [ ] 6카드 드래그 parallax 레이어

### Phase 3 — 디라이트 (1–2일)
- [ ] 커스텀 커서 (데스크톱)
- [ ] Submit 성공 confetti
- [ ] 페이지 전환 (index ↔ lineup)
- [ ] 소셜 hover rotate

## 7. 접근성 & 퍼포먼스 기준
- **`prefers-reduced-motion`**: 모든 스크롤·변환 애니메이션 OFF, 100ms fadeIn만 유지
- **FPS**: 2020+ iPhone Safari에서 60fps 유지 (Chrome DevTools FPS meter)
- **Lighthouse mobile**: Performance ≥ 85
- **CLS (Cumulative Layout Shift)**: 0 (애니메이션이 레이아웃 점프 유발 금지)
- **TTI (Time to Interactive)**: 3s 이내 (3G 시뮬)
- **will-change**: 애니메이션 실행 중에만 적용, 완료 후 제거

## 8. 결정이 필요한 항목

구현 착수 전 확정 필요:
1. **타이밍 철학**: A) 스냅(200–400ms 주류) / B) 시네마틱(800–1200ms)
2. **커스텀 커서**: 적용 / 미적용
3. **페이지 전환 스타일**: 노란 원형 확장 / 단순 fade / 슬라이드
4. **Phase 1만 우선** / **Phase 1+2 동시** / **전체 한 번에**
5. **사운드**: 무음 / 선택적(토글)

## 9. 참고 자료

- [Bennett & Clive](https://bennettandclive.com/) — 레퍼런스 사이트
- [Bennett & Clive - Awwwards SOTD](https://www.awwwards.com/sites/bennett-clive)
- [CSS Design Awards - WOTY 2024 nominee](https://www.cssdesignawards.com/woty2024/sites/bennett-clive/)
- [GSAP ScrollTrigger docs](https://gsap.com/docs/v3/Plugins/ScrollTrigger/)
- [Lenis smooth scroll](https://lenis.darkroom.engineering/)

---

## Next step

위 8번의 5가지 결정 사항에 답해주시면 Phase 1 상세 구현 플랜을 Plan Mode로 정리해서 보여드리겠습니다. 바로 코드에 들어가지 않습니다.
