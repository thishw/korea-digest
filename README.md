# Korea Digest

Synapse 지식 운영 시스템이 자동 발행하는 한국 이슈·인사이트 다이제스트.

## 개요

- **발행 시스템**: [Synapse](https://github.com/This-HW/synapse) — 지능형 지식 운영 시스템
- **공통 테마**: [`github.com/thishw/synapse-hugo-shared`](https://github.com/thishw/synapse-hugo-shared) Hugo Module 사용
- **배포**: GitHub Pages (GitHub Actions 자동 배포)

## 구조

```
korea-digest/
├── content/
│   ├── _index.md          # 홈 페이지
│   └── posts/
│       ├── _index.md      # 글 목록
│       └── *.md           # 개별 글 (Synapse 자동 생성)
├── hugo.toml              # Hugo 설정
├── go.mod                 # Hugo Module 의존성
└── .github/workflows/
    └── hugo.yaml          # GitHub Actions 배포 파이프라인
```

## 로컬 빌드

```bash
hugo --gc --minify
hugo server
```

## 주의사항

- `go.mod`의 `replace` 디렉티브는 로컬 빌드 검증용입니다. push 전 제거하거나 상위 컨트롤러가 정리합니다.
- 콘텐츠는 Synapse 발행 파이프라인이 자동 생성합니다. 직접 편집하지 마세요.
