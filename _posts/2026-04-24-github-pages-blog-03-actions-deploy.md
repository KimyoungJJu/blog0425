---
title: "GitHub Pages 블로그 만들기 3: GitHub Actions로 자동 배포하기"
layout: default
date: 2026-04-24 11:00:00 +0900
nav_order: 3
---

# GitHub Pages 블로그 만들기 3: GitHub Actions로 자동 배포하기

테마 파일을 만들었다면 이제 GitHub Pages로 배포해야 한다. 수동으로 파일을 올리는 방식도 있지만, GitHub Actions를 사용하면 `main` 브랜치에 push할 때마다 자동으로 사이트를 빌드하고 배포할 수 있다.

이번 글에서는 GitHub Actions workflow를 만들어 Jekyll 사이트를 자동 배포하는 과정을 정리한다.

## workflow 파일 위치

GitHub Actions 설정 파일은 저장소의 `.github/workflows` 폴더 안에 둔다.

```text
.github/
  workflows/
    pages.yml
```

파일 이름은 꼭 `pages.yml`일 필요는 없지만, Pages 배포용이라는 의미가 잘 드러나서 관리하기 쉽다.

## pages.yml 작성하기

기본 workflow는 다음과 같이 작성할 수 있다.

```yaml
name: Deploy Jekyll site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v6

      - name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: "3.3"
          bundler-cache: true

      - name: Build with Jekyll
        run: bundle exec jekyll build --baseurl "/blog0425"
        env:
          JEKYLL_ENV: production

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v4
        with:
          path: ./_site

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

`build` job은 Jekyll 사이트를 `_site` 폴더로 빌드한다. `deploy` job은 그 결과물을 GitHub Pages에 배포한다.

## baseurl 확인하기

프로젝트 저장소를 Pages로 배포할 때는 baseurl을 꼭 확인해야 한다.

예를 들어 블로그 주소가 다음과 같다면,

```text
https://kimyoungjju.github.io/blog0425/
```

workflow의 build 명령에는 다음처럼 저장소 이름을 넣는다.

```yaml
run: bundle exec jekyll build --baseurl "/blog0425"
```

이 값이 맞지 않으면 CSS나 링크 경로가 어긋날 수 있다.

## GitHub Pages 설정하기

workflow 파일을 push한 뒤 GitHub 저장소 화면에서 Pages 설정을 해야 한다.

경로는 다음과 같다.

```text
Settings > Pages > Build and deployment
```

여기서 `Source`를 `GitHub Actions`로 선택한다. 별도의 저장 버튼이 보이지 않을 수도 있는데, GitHub 화면에서는 선택하는 즉시 저장되는 경우가 많다.

`Custom domain`은 개인 도메인을 연결할 때만 사용한다. GitHub 기본 주소를 쓸 예정이라면 비워두면 된다.

## 배포 확인하기

설정이 끝났다면 `Actions` 탭으로 이동한다. workflow가 실행 중이면 노란색, 실패하면 빨간색, 성공하면 초록색 체크가 표시된다.

성공 후 블로그 주소는 보통 다음과 같다.

```text
https://사용자명.github.io/저장소명/
```

## 이번 글 요약

- `.github/workflows/pages.yml`로 자동 배포를 설정한다.
- `main` 브랜치에 push하면 workflow가 실행된다.
- `build`는 Jekyll 사이트를 만들고, `deploy`는 GitHub Pages에 올린다.
- Pages 설정에서 `Source`를 `GitHub Actions`로 선택해야 한다.
- Custom domain은 개인 도메인이 있을 때만 사용한다.
