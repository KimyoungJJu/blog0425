---
title: "GitHub Pages 블로그 만들기 5: 글 작성하고 운영하기"
layout: default
date: 2026-04-24 13:00:00 +0900
nav_order: 5
---

# GitHub Pages 블로그 만들기 5: 글 작성하고 운영하기

블로그가 배포되었다면 이제 중요한 일은 글을 꾸준히 쓰는 것이다. GitHub Pages와 Jekyll을 사용하면 글도 코드처럼 파일로 관리한다. 글을 작성하고, 커밋하고, push하면 GitHub Actions가 자동으로 블로그를 다시 배포한다.

이번 글에서는 `_posts` 폴더에 글을 추가하고 운영하는 기본 흐름을 정리한다.

## 글 파일 위치

Jekyll 블로그 글은 `_posts` 폴더에 넣는다.

```text
_posts/
  2026-04-24-my-first-post.md
```

파일명은 날짜로 시작해야 한다.

```text
YYYY-MM-DD-title.md
```

날짜는 글의 발행일 역할을 한다. 뒤의 title 부분은 영어 소문자와 하이픈을 사용하면 URL 관리가 편하다.

## front matter 작성하기

Markdown 파일 맨 위에는 front matter를 적는다.

```markdown
---
title: "나의 첫 블로그 글"
layout: default
date: 2026-04-24 13:00:00 +0900
nav_order: 1
---
```

각 항목의 의미는 다음과 같다.

```text
title     글 제목
layout    사용할 레이아웃
date      글 발행 날짜
nav_order Just the Docs 내비게이션 정렬 순서
```

그 아래부터는 평소처럼 Markdown으로 본문을 쓰면 된다.

## 글 작성 예시

```markdown
---
title: "나의 첫 블로그 글"
layout: default
date: 2026-04-24 13:00:00 +0900
nav_order: 1
---

# 나의 첫 블로그 글

오늘 GitHub Pages 블로그를 만들었다.

## 배운 점

- 저장소를 만들었다.
- Just the Docs 테마를 적용했다.
- GitHub Actions로 자동 배포했다.
```

Markdown은 제목, 목록, 코드 블록을 간단하게 표현할 수 있어서 기술 기록에 잘 맞는다.

## 홈 화면 글 목록

홈 화면에서 글 목록을 보여주고 싶다면 `index.md`에 다음 Liquid 코드를 사용할 수 있다.

```liquid
{% raw %}{% assign posts = site.posts | sort: "date" | reverse %}
{% for post in posts %}
- [{{ post.title }}]({{ post.url | relative_url }}) - {{ post.date | date: "%Y-%m-%d" }}
{% endfor %}{% endraw %}
```

이 코드는 `_posts`에 있는 글을 날짜 기준 최신순으로 정렬해서 보여준다.

## 글 배포 흐름

새 글을 썼다면 Git에 반영한다.

```bash
git status
git add .
git commit -m "Add new blog post"
git push
```

push가 끝나면 GitHub Actions가 자동으로 실행된다. Actions가 성공하면 블로그 사이트에 새 글이 반영된다.

## 운영 팁

글을 쓰기 시작할 때는 완벽한 카테고리보다 꾸준한 파일 구조가 더 중요하다. 처음에는 `_posts` 안에 날짜순으로 글을 쌓고, 글이 많아졌을 때 주제별 모음이나 내비게이션을 정리해도 충분하다.

추천하는 운영 방식은 단순하다.

```text
1. 배운 내용을 바로 짧게 기록한다.
2. 막혔던 에러와 해결 방법을 남긴다.
3. 명령어와 설정 파일은 실제 사용한 형태로 적는다.
4. 나중에 다시 읽을 나를 독자로 생각한다.
```

블로그는 처음부터 완성된 문서함일 필요가 없다. 오늘의 시행착오가 내일의 검색 결과가 된다.

## 이번 글 요약

- Jekyll 글은 `_posts` 폴더에 작성한다.
- 파일명은 `YYYY-MM-DD-title.md` 형식을 따른다.
- front matter로 제목, 날짜, 레이아웃을 설정한다.
- 글을 커밋하고 push하면 GitHub Actions가 자동 배포한다.
- 운영 초기에는 단순한 구조로 꾸준히 기록하는 것이 좋다.
