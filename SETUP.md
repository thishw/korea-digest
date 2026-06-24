# Korea Digest — Setup Report

생성일: 2026-06-23  
패턴 참고: `~/dev/web-site-repo/hw_insight/`

---

## 파일 트리 (git-tracked)

```
korea-digest/
├── .github/
│   └── workflows/
│       └── hugo.yaml          # GitHub Pages 배포 워크플로우 (hw_insight 동일)
├── .gitignore                 # public/, resources/_gen/, .hugo_build.lock
├── README.md                  # 사이트 역할·구조·주의사항
├── content/
│   ├── _index.md              # 홈 (title: Korea Digest)
│   └── posts/
│       ├── _index.md          # 글 목록 섹션
│       └── 2026-06-23-welcome.md  # 샘플 글 (표+blockquote+mermaid+categories:["소개"])
├── go.mod                     # module + require synapse-hugo-shared + replace 디렉티브
└── hugo.toml                  # baseURL, title, taxonomies, menu, goldmark, [module]
```

---

## 빌드 결과

```
hugo -s ~/dev/web-site-repo/korea-digest --gc --minify

                  │ KO
──────────────────┼────
 Pages            │ 16
 Paginator pages  │  0
 Non-page files   │  0
 Static files     │  0
 Processed images │  0
 Aliases          │  0
 Cleaned          │  0

Total in 85 ms
```

빌드 에러 없음. Pages 16개 생성(홈, 글 섹션, 샘플 포스트, 카테고리/태그 taxonomy 등).

---

## 검증 결과

### 메뉴 탭 (홈/글)

```
grep -o '홈\|글' public/index.html
→ 홈, 글, 글  ✓
```

HTML 확인: `<li class=menu-item><a href=/ class=menu-link>홈</a></li>` + `<li class=menu-item><a href=/posts/ class=menu-link>글</a></li>`

### 샘플 글 렌더 (표·blockquote·mermaid)

```
grep -o 'mermaid\|blockquote\|<table' public/posts/welcome-korea-digest/index.html
→  1 <table
   2 blockquote
   5 mermaid   ✓
```

### 카테고리 페이지

```
ls public/categories/소개/
→ index.html  index.xml  ✓
```

`categories: ["소개"]` front matter → `public/categories/소개/` 생성 확인.

---

## replace 디렉티브 위치

`go.mod` 3행:

```
replace github.com/thishw/synapse-hugo-shared => ../synapse-hugo-shared
```

로컬 빌드 검증용. 로컬에서는 `~/dev/web-site-repo/synapse-hugo-shared/`를 직접 참조.  
**push 전 상위 컨트롤러가 제거 또는 go.sum 기반 버전 고정으로 교체해야 함.**

---

## 초기 커밋

```
브랜치: main
커밋 해시: 87843bc
메시지: feat(korea-digest): initial site scaffold
```

**GitHub 레포 생성·push는 하지 않음.** 상위 컨트롤러(thishw 인증)가 수행 예정.

---

## 우려사항

1. **go.sum 부재**: `replace` 디렉티브로 로컬 경로를 직접 참조하므로 go.sum이 생성되지 않음.  
   CI에서 빌드할 때 `replace` 제거 후 공식 버전 pin + `go mod tidy`로 go.sum을 생성해야 함.  
   현재 사용 버전: `v0.0.0-20260623052829-2838d77953fb` (hw_insight go.sum 참조값).

2. **go 1.26 + `hugo mod get` 비호환**: 지시사항대로 `hugo mod get`을 사용하지 않고 go.mod 직접 작성으로 우회. CI는 `setup-go@v5` + `go-version-file: go.mod`로 처리.

3. **메뉴 임시 구성**: 현재 `홈(/)`·`글(/posts/)` 2개. 실제 주제 확정 후 hugo.toml `[[menu.main]]` 블록 교체 필요.
