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
Icon?
*.xlsx
```

문서/리서치/브랜딩 자산은 로컬에만 두고 레포에는 코드만 올립니다.

## 디자인시스템

- 모든 앱은 `김실장` 브랜드 가드레일을 따름
- 공통 자산은 추후 `Home/shared/`로 통합 예정
- (TODO) `DESIGN-SYSTEM-김실장.md`를 Home으로 이전
