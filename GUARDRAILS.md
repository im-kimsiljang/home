# 김실장 가드레일 🛡️

**무조건 지켜야 하는 룰.** 어기면 라우팅 깨지거나, 보안 사고 나거나, 사용자가 앱 못 찾음.

---

## 1. 네이밍 (절대 변경 금지)

| 위치 | 형식 | 예시 |
|------|------|------|
| 로컬 폴더 | `YYMM-앱이름` | `2604-cost-calculator` |
| GitHub 레포 | `앱이름` (프리픽스 없이) | `cost-calculator` |
| GitHub 조직 | `im-kimsiljang` (고정) | — |
| URL | `kimsiljang.com/앱이름/` | `kimsiljang.com/cost-calculator/` |

**왜 강제?** 라우팅(`Home/vercel.json` rewrites)이 정확히 이 형식에 의존. 어기면 404.

---

## 2. .gitignore 필수 항목

모든 앱 레포의 `.gitignore`에 반드시 포함:

```
.vercel
.DS_Store
.claude/
*.jpeg
*.jpg
icons/preview/
00-자료조사/
01-템플릿/
02-GTM/
03-Branding/
04-Character/
DESIGN-SYSTEM-김실장.md
02-PRD-*.md
Icon
*.xlsx
.env
.env.local
```

**왜 강제?**
- `00-자료조사/` ~ `04-Character/`: 내부 작업 자료 (외부 노출 금지)
- `DESIGN-SYSTEM-*.md`, `02-PRD-*.md`: 미공개 문서
- `.env*`: API 키 유출 방지
- `.vercel`: 토큰 유출 방지

⚠️ **`Icon?` 패턴 절대 쓰지 말 것** — `icons/` 폴더까지 매칭해서 PWA 아이콘 다 제외시킴. macOS의 `Icon\r` 파일은 수동 삭제만.

---

## 3. PWA 필수 (모든 월간 앱)

모든 앱은 **반드시 PWA**. 사용자가 홈화면에 추가하면 앱처럼 작동.

### 3종 세트 필수

1. ✅ **`manifest.json`** — 앱 정보
2. ✅ **`sw.js`** — Service Worker (오프라인)
3. ✅ **`icons/`** 다음 3개 필수:
   - `icon-192.png` (안드로이드)
   - `icon-512.png` (안드로이드 스플래시)
   - `apple-touch-icon.png` (iOS)

### `<head>` 필수 메타 태그

```html
<link rel="manifest" href="/manifest.json?v=1">
<meta name="theme-color" content="#3b5fff">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="default">
<meta name="apple-mobile-web-app-title" content="앱이름">
<link rel="apple-touch-icon" href="/icons/apple-touch-icon.png?v=1">
```

**왜 강제?** 김실장 = 모바일 자영업자 도구. PWA 아니면 사용성 50% 손실.

(템플릿/코드 예시는 [GUIDELINES.md](./GUIDELINES.md) 참고)

---

## 4. Home에 등록 필수

새 앱 배포 후 **반드시 두 가지 추가**:

1. **`Home/apps.json`** — 한 줄 등록 (앱 레지스트리)
2. **`Home/index.html`** — 카드 1개 추가 (사용자 진입점)

**왜 강제?** 안 하면 사용자가 새 앱 존재 자체를 모름.

---

## 5. 폴더 구조 최소 요구사항

각 앱 레포는 최소 다음 파일/폴더 보유:

```
앱이름/
├── index.html          # 앱 랜딩 (필수)
├── manifest.json       # PWA (필수)
├── sw.js               # PWA (필수)
├── favicon.ico         # 필수
├── icons/              # PWA 아이콘 (필수)
│   ├── icon-192.png
│   ├── icon-512.png
│   └── apple-touch-icon.png
├── vercel.json         # Vercel 라우팅 (필수)
├── README.md           # 설명 (필수)
└── .gitignore          # 위 항목 포함 (필수)
```

---

## 6. 문서 vs 코드 분리

- 코드만 GitHub에 올림
- 작업 자료(자료조사, 템플릿, 디자인 원본, PRD)는 **로컬에만**
- `.gitignore`로 강제

**왜 강제?** 미공개 자료가 외부 노출되면 IP/전략 유출.
