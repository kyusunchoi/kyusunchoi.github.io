# 개인 학술 홈페이지 (Quarto)

전부 무료로 운영되는 학술용 개인 사이트 스캐폴드입니다.
렌더링 검증 완료 (Quarto 1.6.43).

---

## 0. 비용

| 항목 | 비용 |
|---|---|
| Quarto CLI | 무료 (오픈소스) |
| VS Code + Quarto 확장 | 무료 |
| GitHub 계정 / 저장소 | 무료 |
| GitHub Pages 호스팅 | 무료 (public 저장소) |
| `USERNAME.github.io` 주소 | 무료 |
| 개인 도메인 (선택) | 연 1.5만~2만원 — **안 쓰셔도 됩니다** |

---

## 1. 설치 (1회)

1. **Quarto CLI** — <https://quarto.org/docs/get-started/> 에서 Windows 설치 파일
   내려받아 실행. 설치 후 터미널에서 확인:
   ```
   quarto --version
   ```
2. **VS Code 확장** — 확장 탭에서 `Quarto` 검색 후 설치 (게시자: Quarto).
   미리보기, 문법 강조, 렌더 단축키가 붙습니다.
3. **Git** — <https://git-scm.com/downloads>
4. **GitHub 계정** — <https://github.com/signup>

---

## 2. 로컬에서 띄우기

이 폴더를 VS Code로 열고 터미널에서:

```
quarto preview
```

브라우저가 열리고, 파일을 저장할 때마다 자동으로 갱신됩니다.
중지는 터미널에서 `Ctrl+C`.

---

## 3. 바꿔야 할 자리

전체 검색으로 `USERNAME` 을 GitHub 아이디로 일괄 치환한 뒤:

| 파일 | 내용 |
|---|---|
| `_quarto.yml` | 사이트 제목, `site-url`, 네비게이션, ORCID 주소 |
| `index.qmd` | 소개문, 이메일, Google Scholar 주소 |
| `research.qmd` | 연구 축 3개 서술 |
| `references.bib` | **여기가 핵심** — 논문 목록. 아래 4번 참조 |
| `publications.qmd` | 저서, 워킹페이퍼 |
| `images/profile.jpg` | 프로필 사진으로 교체 (정사각형 권장, 600px 이상) |
| `files/cv.pdf` | CV PDF 를 이 경로에 넣으면 링크가 살아납니다 |
| `theme.scss` | 색·서체. 손대지 않아도 됩니다 |

---

## 4. 논문 목록 자동화

`publications.qmd` 의 목록은 `references.bib` 에서 자동 생성됩니다.
논문이 추가되면 **bib 항목만 넣으면 끝**이고, HTML을 손댈 필요가 없습니다.

BibTeX 항목 얻는 법:
- Google Scholar → 논문 옆 인용 아이콘 → BibTeX
- DOI → <https://doi.org> 조회 → BibTeX 내보내기
- Zotero → 우클릭 → Export → BibTeX

APA 형식으로 바꾸려면 `apa.csl` 파일을 <https://www.zotero.org/styles>
에서 내려받아 폴더에 두고 `_quarto.yml` 에 한 줄 추가:

```yaml
csl: apa.csl
```

---

## 5. GitHub Pages 배포

### 5-1. 저장소 만들기

GitHub에서 **Public** 저장소를 새로 만듭니다.
이름을 `USERNAME.github.io` 로 하면 주소가
`https://USERNAME.github.io` 가 되어 가장 깔끔합니다.

### 5-2. 최초 1회 — 로컬에서 게시

터미널에서:

```
git init
git add .
git commit -m "Initial site"
git branch -M main
git remote add origin https://github.com/USERNAME/USERNAME.github.io.git
git push -u origin main

quarto publish gh-pages
```

`quarto publish gh-pages` 는 `gh-pages` 브랜치를 만들고, 렌더 결과를 그
브랜치에 올린 뒤 브라우저를 띄웁니다. **이 명령은 반드시 한 번은 로컬에서
실행해야 합니다** — 이후 GitHub Actions 자동화가 이 브랜치를 전제로 동작합니다.

### 5-3. Pages 설정 확인

저장소 → Settings → Pages → Source 가 `gh-pages` 브랜치 / `/ (root)` 로
잡혀 있는지 확인. 배포까지 1~2분 걸립니다.

### 5-4. 이후 — 자동 배포

`.github/workflows/publish.yml` 이 이미 들어 있습니다.
`main` 에 push 할 때마다 GitHub가 알아서 렌더하고 배포합니다:

```
git add .
git commit -m "Add paper"
git push
```

로컬에서 `quarto render` 를 돌릴 필요가 없습니다.

---

## 6. 새 글 쓰기

```
posts/2026-08-어떤제목/index.qmd
```

폴더를 만들고 `index.qmd` 상단에:

```yaml
---
title: "제목"
description: "한 줄 요약"
date: 2026-08-15
categories: [method, climate]
---
```

`blog.qmd` 목록에 자동으로 올라오고, RSS 피드(`blog.xml`)도 자동 생성됩니다.

---

## 7. 알아두실 점

- **R/Python 코드를 문서 안에서 실행**할 수 있습니다. 코드 청크를 넣으면
  렌더 시점에 실행되어 표·그림이 문서에 박힙니다. 재현가능성 자료를
  올리기에 적합합니다. 실행 결과 캐시는 `freeze: auto` 로 이미 켜져 있습니다.
- **한글 렌더링**은 웹폰트(Noto Sans/Serif KR)로 처리했습니다. 라틴 문자는
  Source Serif 4 / Inter 로, 한글은 Noto 로 자동 분기됩니다.
- **Google 색인**은 `sitemap.xml` 과 `robots.txt` 가 자동 생성되므로
  기본은 갖춰져 있습니다. Google Search Console 에 사이트를 등록하면
  색인이 빨라집니다.
