# 김실장 가이드라인 📘

**권장 사항 + 템플릿.** 따르면 좋은 best practices와 코드 예시. 강제 룰은 [GUARDRAILS.md](./GUARDRAILS.md) 참고.

---

## URL은 짧게 ⭐

**모든 URL은 가능한 짧게.** 사용자가 입력/공유하기 편하고, 카드 표시도 깔끔.

| ❌ 길다 | ✅ 짧다 |
|---------|---------|
| `kimsiljang.com/cost-calculator/` | `kimsiljang.com/cal/` |
| `kimsiljang.com/restaurant-cost-calculator/` | `kimsiljang.com/cal/` |
| `cost-calculator.kimsiljang.com` | `cal.kimsiljang.com` |
| `automation-wifi-setup.kimsiljang.com` | `wifi.kimsiljang.com` |

**원칙:**
- 풀네임은 **레포/프로젝트명**에서 (예: `cost-calculator` 레포)
- URL은 **3-5글자** 약자 (예: `cal`, `wifi`, `pos`, `crm`)
- 약자가 헷갈리면 풀네임 사용 OK

**기존 긴 URL 정리:**
- 새 짧은 URL 만들고 → 옛 URL은 301 영구 리다이렉트 (SEO 보존)
- 예시: `home/vercel.json`의 `/cost-calculator/* → /cal/*` 리다이렉트

---

## 새 앱 만드는 추천 순서

1. **로컬 폴더 생성**: `YYMM-앱이름/`
2. **기획**: 자료조사/템플릿 만들기 (로컬에만, 푸시 ❌)
3. **디자인**: 김실장 브랜드 가드레일 따름 (`shared/design-system.css` 예정)
4. **개발**: PWA 3종 세트 + 앱 본체
5. **GitHub 레포 생성**: `gh repo create im-kimsiljang/앱이름 --public --source=. --push`
6. **Vercel 프로젝트 생성**: `im-kimsiljang/앱이름` 연결
7. **`Home/vercel.json` rewrites 추가** (필요시):
   ```json
   { "source": "/앱이름/:path*", "destination": "https://앱-xxx.vercel.app/앱이름/:path*" }
   ```
8. **`Home/apps.json` + `Home/index.html`** 카드 추가 ⚠️ (가드레일)
9. **라이브 검증** (kimsiljang.com/앱이름/)

---

## PWA 구현 템플릿

### `manifest.json`

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

self.addEventListener('install', e =>
  e.waitUntil(caches.open(CACHE).then(c => c.addAll(ASSETS)))
);

self.addEventListener('activate', e =>
  e.waitUntil(
    caches.keys().then(keys =>
      Promise.all(keys.filter(k => k !== CACHE).map(k => caches.delete(k)))
    )
  )
);

self.addEventListener('fetch', e =>
  e.respondWith(
    caches.match(e.request).then(r => r || fetch(e.request))
  )
);
```

### Service Worker 등록 (HTML에 추가)

```html
<script>
  if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('/앱이름/sw.js');
  }
</script>
```

---

## 디자인 시스템 (권장)

- **컬러**: `--primary: #3b5fff` (김실장 브랜드 블루)
- **폰트**: Pretendard (cdn.jsdelivr.net)
- **이모지 아이콘**: `/icons/emoji/*.svg` 활용
- **캐릭터**: `/icons/character-*.png` 활용
- **상세**: 추후 `Home/shared/design-system.css` 참고 (TODO)

---

## 사용자 안내 카피 (마케팅)

기술 용어 대신 사장님이 이해하는 표현:

| 기술 표현 ❌ | 사장님 친화 ✅ |
|------------|----------------|
| PWA로 제작 | **홈화면에 앱처럼 추가** |
| Service Worker로 캐싱 | **인터넷 끊겨도 일부 동작** |
| HTTPS / SSL | (언급 안 함, 당연한 거) |
| OAuth / Firebase Auth | **구글 계정으로 로그인** |
| Serverless Function | **빠른 처리** |

---

## 코드 스타일 (권장)

- **인라인 CSS**: 작은 앱은 `<style>` 인라인이 빠름 (HTTP 요청 절약)
- **외부 라이브러리**: CDN 우선 (npm 말고) — 빌드 단순화
- **JS 모듈**: `<script type="module">` 활용
- **타입스크립트**: 필요시만 (가벼운 앱은 plain JS)

---

## API 사용 (`kimsiljang.com/api/*`)

- 모든 앱이 공통 API 사용 가능
- 엔드포인트 목록: [im-kimsiljang/api](https://github.com/im-kimsiljang/api) README 참고
- 새 엔드포인트 필요시 → api 레포에 PR

---

## TODO 항목

다음 작업들은 향후 진행:

- [ ] `Home/shared/design-system.css` 생성 (현재 cost-calculator에 있는 디자인시스템 이전)
- [ ] `Home/shared/brand.js` (로고/컬러/폰트 공통화)
- [ ] 앱 템플릿 (`templates/monthly-app/`) 만들어서 새 앱 시작 1분 컷
- [ ] CI: 새 앱 푸시 시 PWA 체크 자동화 (manifest, sw.js 존재 검증)
