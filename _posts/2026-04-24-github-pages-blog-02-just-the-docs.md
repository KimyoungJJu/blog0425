---
title: "GitHub Pages 블로그 만들기 2: Just the Docs 테마 적용하기"
layout: default
date: 2026-04-24 10:00:00 +0900
nav_order: 2
---

# GitHub Pages 블로그 만들기 2: Just the Docs 테마 적용하기

저장소를 준비했다면 이제 블로그의 모양을 잡을 차례다. 이번에는 Jekyll 테마인 Just the Docs를 사용한다.

Just the Docs는 원래 문서 사이트에 잘 어울리는 테마지만, 글을 차곡차곡 정리하는 블로그에도 꽤 잘 맞는다. 왼쪽 내비게이션, 검색, 깔끔한 본문 폭이 기본으로 제공되기 때문에 기술 블로그나 학습 기록용 블로그를 만들기에 편하다.

## 필요한 파일 구조

가장 단순한 시작 구조는 다음과 같다.

```text
.github/
  workflows/
    pages.yml
_posts/
  2026-04-24-welcome.md
.gitignore
Gemfile
_config.yml
index.md
README.md
```

여기서 핵심은 `Gemfile`, `_config.yml`, `index.md`, `_posts` 폴더다.

## Gemfile 작성하기

Jekyll과 Just the Docs 테마를 Ruby gem으로 불러오기 위해 `Gemfile`을 만든다.

```ruby
source "https://rubygems.org"

gem "jekyll", "~> 4.4"
gem "just-the-docs", "~> 0.12"
```

GitHub Actions가 이 파일을 읽고 필요한 Ruby 패키지를 설치한다.

## _config.yml 작성하기

Jekyll 사이트 설정은 `_config.yml`에 넣는다.

```yaml
title: YoungJJu Blog
description: GitHub Pages blog powered by Just the Docs.
theme: just-the-docs

url: https://사용자명.github.io
baseurl: /저장소명

permalink: pretty

search_enabled: true
heading_anchors: true
color_scheme: light
```

여기서 `url`과 `baseurl`이 중요하다. 사용자 페이지가 아니라 프로젝트 저장소로 GitHub Pages를 운영하는 경우, `baseurl`에는 저장소 이름을 넣는다.

예를 들어 저장소가 `blog0425`라면 다음처럼 쓴다.

```yaml
url: https://kimyoungjju.github.io
baseurl: /blog0425
```

## 홈 화면 만들기

`index.md`는 블로그 첫 화면이다.

```markdown
---
title: Home
layout: home
nav_order: 1
---

# YoungJJu Blog

GitHub Pages and Just the Docs theme are ready.

## Latest Posts

{% raw %}{% assign posts = site.posts | sort: "date" | reverse %}
{% for post in posts %}
- [{{ post.title }}]({{ post.url | relative_url }}) - {{ post.date | date: "%Y-%m-%d" }}
{% endfor %}{% endraw %}
```

이렇게 작성하면 `_posts` 폴더에 있는 글 목록이 최신순으로 홈 화면에 표시된다.

## 첫 글 만들기

Jekyll의 글 파일은 `_posts` 폴더에 넣는다. 파일명은 날짜로 시작해야 한다.

```text
YYYY-MM-DD-title.md
```

예시는 다음과 같다.

```markdown
---
title: Welcome
layout: default
date: 2026-04-24
nav_order: 1
---

# Welcome

This is the first post for the blog.
```

이제 기본 테마 구조는 준비됐다. 다음 글에서는 이 파일들이 GitHub에 push될 때 자동으로 배포되도록 GitHub Actions를 설정한다.

## 이번 글 요약

- Just the Docs는 Jekyll 기반 테마다.
- `Gemfile`에서 Jekyll과 테마 gem을 지정한다.
- `_config.yml`에서 사이트 이름, 주소, 테마를 설정한다.
- 프로젝트 저장소 Pages에서는 `baseurl`에 저장소 이름을 넣는다.
- `_posts` 폴더에 글을 추가하면 블로그 글이 된다.
