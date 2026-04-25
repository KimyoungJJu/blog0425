---
title: "GitHub Pages 블로그 만들기 1: 저장소 만들고 로컬에 클론하기"
layout: default
date: 2026-04-25 09:00:00 +0900
nav_order: 1
---

# GitHub Pages 블로그 만들기 1: 저장소 만들고 로컬에 클론하기

GitHub Pages 블로그를 만들 때 가장 먼저 할 일은 블로그가 될 저장소를 준비하는 것이다. GitHub Pages는 GitHub 저장소에 있는 파일을 기준으로 정적 사이트를 만들어 준다. 그래서 블로그도 결국 하나의 Git 저장소에서 시작한다.

이번 글에서는 GitHub에서 새 저장소를 만들고, 내 컴퓨터의 작업 폴더에 연결하는 과정까지 정리한다.

## 새 저장소 만들기

GitHub에 로그인한 뒤 새 repository를 만든다. 저장소 이름은 블로그 주소에 들어가므로 너무 길거나 복잡하지 않게 정하는 것이 좋다.

예를 들어 저장소 이름을 `blog0425`로 만들면, 나중에 GitHub Pages 주소는 보통 다음과 같은 형태가 된다.

```text
https://사용자명.github.io/blog0425/
```

저장소를 만들 때 README, `.gitignore`, license를 미리 추가할 수도 있지만, 처음 구조를 직접 잡아볼 계획이라면 빈 저장소로 만들어도 괜찮다.

## 로컬 폴더에 클론하기

저장소를 만들었다면 내 컴퓨터의 작업 폴더로 이동해서 clone한다.

```bash
git clone https://github.com/사용자명/저장소명.git .
```

마지막의 `.`은 현재 폴더에 저장소 내용을 바로 가져오겠다는 뜻이다. 이미 작업 폴더를 만들어 두었고 그 안에서 바로 작업하고 싶을 때 사용한다.

빈 저장소를 클론하면 이런 경고가 보일 수 있다.

```text
warning: You appear to have cloned an empty repository.
```

이건 문제가 아니다. GitHub 저장소에 아직 파일이 없다는 뜻일 뿐이다.

## 연결 상태 확인하기

클론이 끝나면 Git 상태를 확인한다.

```bash
git status
```

아직 커밋이 없다면 대략 이런 상태가 보인다.

```text
No commits yet on main
```

이제 이 폴더는 GitHub 저장소와 연결된 로컬 작업 공간이다. 다음 글에서는 여기에 Just the Docs 테마를 적용할 기본 파일들을 추가한다.

## 이번 글 요약

- GitHub Pages 블로그는 GitHub 저장소에서 시작한다.
- 저장소 이름은 나중에 블로그 주소에 포함된다.
- `git clone 저장소주소 .` 명령으로 현재 폴더에 클론할 수 있다.
- 빈 저장소 경고는 정상이다.

