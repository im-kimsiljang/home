# 김실장 월간 앱 가드레일

새 앱을 만들 때 따라야 할 규칙입니다.

## 네이밍 규칙

- **로컬 폴더**: `YYMM-앱이름` (예: `2604-cost-calculator`)
  - 시간순 정렬용 프리픽스
- **GitHub 레포**: `앱이름` (예: `cost-calculator`)
  - im-kimsiljang 조직 아래
- **URL**: `kimsiljang.com/앱이름/` (예: `kimsiljang.com/cost-calculator/`)

## 새 앱 만드는 절차

1. 로컬에 `YYMM-앱이름/` 폴더 생성
2. 코드 작성 (디자인시스템은 Home의 가드레일 따름)
3. GitHub에 `im-kimsiljang/앱이름` 레포 생성 후 푸시
4. Vercel 프로젝트 생성 → `kimsiljang.com/앱이름/`로 라우팅
5. **`Home/apps.json`에 한 줄 추가** (잊지 말 것)
6. **`Home/index.html`에 카드 1개 추가**

## 폴더 구조 (각 앱 레포)

```
앱이름/
├── index.html               # 앱 랜딩
├── (앱 코드)
├── icons/                   # PWA 아이콘
├── manifest.json            # PWA 설정
├── sw.js                    # Service Worker
├── sitemap.xml
├── favicon.ico
├── vercel.json              # Vercel 라우팅 설정
└── .gitignore
```

## 항상 .gitignore 처리

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
```

⚠️ `Icon?` 패턴은 `icons/` 폴더까지 매칭해서 제외시키니 쓰지 말 것 (macOS의 `Icon\r` 파일은 그냥 수동 삭제).

문서/리서치/브랜딩 자산은 로컬에만 두고 레포에는 코드만 올립니다.

## PWA 필수 (모든 월간 앱)

모든 앱은 **PWA(Progressive Web App)** 로 만듭니다. 사용자가 홈화면에 추가하면 진짜 앱처럼 작동.

### PWA 구성 요소 (3종 세트)

1. **`manifest.json`** — 앱 정보 (이름, 아이콘, 시작 URL, 색상)
2. **`sw.js`** — Service Worker (오프라인 캐싱, 백그라운드 동기화)
3. **`icons/`** — PWA 아이콘
   - `icon-192.png` (필수, 안드로이드)
   - `icon-512.png` (필수, 안드로이드/스플래시)
   - `apple-touch-icon.png` (필수, iOS)
   - `favicon-16.png`, `favicon-32.png` (선택, 브라우저 탭)

### `<head>` 필수 메타 태그

```html
<link rel="manifest" href="/manifest.json?v=1">
<meta name="theme-color" content="#3b5fff">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="apple-mobile-web-app-status-bar-style" content="default">
<meta name="apple-mobile-web-app-title" content="앱이름">
<link rel="apple-touch-icon" href="/icons/apple-touch-icon.png?v=1">
```

### `manifest.json` 템플릿

```json
{
  "name": "김실장 - 앱이름",
  "short_name": "앱이름",
  "description": "한 줄 설명",
  "start_url": "/앱이름/",
  "scope": "/앱이름/",
  "display": "standalone",
  "background_color": "#fdfdfd",
  "theme_color": "#3b5fff",
  "icons": [
    { "src": "/icons/icon-192.png", "sizes": "192x192", "type": "image/png" },
    { "src": "/icons/icon-512.png", "sizes": "512x512", "type": "image/png" }
  ]
}
```

### Service Worker (`sw.js`) 최소 구현

```javascript
const CACHE = '앱이름-v1';
const ASSETS = ['/앱이름/', '/앱이름/index.html', '/manifest.json'];

self.addEventListener('install', e => e.waitUntil(caches.open(CACHE).then(c => c.addAll(ASSETS))));
self.addEventListener('activate', e => e.waitUntil(caches.keys().then(keys => Promise.all(keys.filter(k => k !== CACHE).map(k => caches.delete(k))))));
self.addEventListener('fetch', e => e.respondWith(caches.match(e.request).then(r => r || fetch(e.request))));
```

### 사용자 안내 카피 (마케팅용)

기술 용어 "PWA" 대신:
- ✅ "**홈화면에 앱처럼 추가**할 수 있어요"
- ✅ "별도 설치 없이 **앱처럼 사용**"
- ❌ "PWA로 제작" (사장님 대상엔 어려움)

## 디자인시스템

- 모든 앱은 `김실장` 브랜드 가드레일을 따름
- 공통 자산은 추후 `Home/shared/`로 통합 예정
- (TODO) `DESIGN-SYSTEM-김실장.md`를 Home으로 이전
