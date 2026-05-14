# Glowbeast Landing — 이미지 배치 가이드

아래 파일을 **정확한 이름**으로 이 `assets/` 폴더에 넣어주세요.
Figma에서 각 노드를 Export로 내보내면 됩니다.

**Figma 파일**: `h0efHuovfV7mG9ZQINaalO` (Glowbeast)
**기준 노드**: `3089:2` (모바일 402px 랜딩, v1.2)

---

## ✅ 재export 권장 (디자인 갱신 가능성)

v1.2에서 Figma `3089:*` 노드로 리뉴얼되면서 아래 이미지는 새로 export하시는 걸 권장합니다.

| 파일명 | 디자인 변경 가능성 |
|---|---|
| `hero-bg.png` | 중간 |
| `category-1.png ~ category-6.png` (6장) | 중간 |
| `footer-logo.png` | 높음 |
| `heat-spikes.gif`, `sweat-drains.png`, `bacteria.png`, `elasticity.png` | 높음 |

---

## Hero 섹션 (최상단)

| 파일명 | Figma 노드 | 포맷 | 권장 크기 | 설명 |
|---|---|---|---|---|
| `hero-bg.png` | `3089:3` | PNG/JPG | 800×1754 (2x) | Hero 풀블리드 배경 사진 (모델 이미지) |
| `logo.svg` | (재사용) | SVG | — | 좌상단 내비 + Hero 워드마크 + Mission 워드마크(filter invert) |
| `chevron-down.svg` | (재사용) | SVG | — | Hero 맨 아래 ∨ 아이콘 |

## "After the sweat:" 섹션 (검은 배경, sticky stacking 4 카드)

| 파일명 | Figma 노드 | 포맷 | 권장 크기 | 설명 |
|---|---|---|---|---|
| `heat-spikes.gif` | `3089:53` (image 15) | GIF/PNG | 800×800 (2x) | 01 HEAT SPIKES 카드 이미지 |
| `sweat-drains.png` | `3089:194` (image 15 nested) | PNG | 800×800 (2x) | 02 SWEAT DRAINS 카드 이미지 |
| `bacteria.png` | `3089:201` (image 15 nested) | PNG | 800×800 (2x) | 03 BACTERIA PARTY 카드 이미지 |
| `elasticity.png` | `3089:209` (image 16) | PNG | 800×800 (2x) | 04 ELASTICITY DROPS 카드 이미지 |

> Figma에서 이 4 카드는 image 15/16/17/18 등 여러 이미지가 z-stack으로 겹쳐 보이게 처리됨. 각 카드의 **최상단 이미지**를 export 하시면 됩니다.

## "That's where we start:" 섹션 (흰 배경)

`logo.svg` 1장만 사용 (CSS `filter: invert(1)`로 검정 워드마크 표시). 별도 export 불필요.

## "The Beast Solution:" 섹션 (검은 배경, 가로 스크롤 6 카드)

| 파일명 | Figma 노드 | 포맷 | 권장 크기 | 설명 |
|---|---|---|---|---|
| `category-1.png` | `3089:90` 자식 frame | PNG | 616×612 (2x) | SOOTHING |
| `category-2.png` | `3089:95` 자식 frame | PNG | 616×612 (2x) | HYDRATION |
| `category-3.png` | `3089:100` 자식 frame | PNG | 616×612 (2x) | ELASTICITY & ENERGY |
| `category-4.png` | `3089:105` 자식 frame | PNG | 616×612 (2x) | RECOVERY & BARRIER |
| `category-5.png` | `3089:110` 자식 frame | PNG | 616×612 (2x) | PURIFYING |
| `category-6.png` | `3089:115` 자식 frame | PNG | 616×612 (2x) | ANTIOXIDANT |

## "Hey Beast there," 섹션 (검은 배경, signup)

이미지 없음 — 텍스트 + 입력 + 노란 CTA 버튼만.

## Footer (검은 배경)

| 파일명 | Figma 노드 | 포맷 | 권장 크기 | 설명 |
|---|---|---|---|---|
| `footer-logo.png` | `3255:73` (footer-logo) | PNG (투명배경) | 708×488 (2x) | 하단 대형 브랜드 락업 |
| `icon-instagram.svg` | `3255:95` | SVG | 24×24 | Instagram 아이콘 |
| `icon-amazon.svg` | `3255:106` | SVG | 24×24 | Amazon 아이콘 |

---

## ⚠️ v1.1까지 사용했지만 v1.2에서 미사용 (삭제 가능)

| 파일명 | 미사용 사유 |
|---|---|
| `meet-1.png ~ meet-4.png` | "Meet The Beasts" 섹션 제거 (v1.2) |
| `mission-wordmark.png`, `wordmark-glow.png`, `beast-ilo.png` | 이미 `logo.svg`로 대체됨 (v1.0) |
| `heat-spikes.png` | `.gif` 버전이 사용 중 |

폴더에 남겨두셔도 무관하지만 (참조하지 않음), 정리 시 제거 권장.

---

## Figma Export 팁

1. Figma에서 위 노드 ID 선택
2. 우측 **Design 패널 하단 Export** 섹션 펼치기
3. **포맷 선택** + **2x** 배율 (Retina 대응)
4. **`Export [노드명]`** 클릭 → 이 `assets/` 폴더로 저장
5. **파일명을 위 표와 정확히 일치**시켜 저장 (`hero-bg.png` 등)

## 확인 방법

모든 파일을 넣은 후 `landing/index.html`을 브라우저에서 열면 바로 반영됩니다.
누락된 파일이 있으면 그 위치에 깨진 이미지 아이콘이 표시됩니다.
레이아웃·텍스트·버튼 네비게이션은 이미지 없이도 정상 작동합니다.
