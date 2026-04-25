---
title: "GitHub Pages 블로그 만들기 4: Actions 에러 해결하기"
layout: default
date: 2026-04-24 12:00:00 +0900
nav_order: 4
---

# GitHub Pages 블로그 만들기 4: Actions 에러 해결하기

GitHub Pages를 처음 설정하다 보면 workflow가 한 번에 성공하지 않을 수 있다. 특히 Jekyll 빌드와 Pages 배포가 같은 workflow 안에 있기 때문에, 어디에서 실패했는지 나누어 보는 것이 중요하다.

이번 글에서는 실제로 마주칠 수 있는 Actions 에러를 기준으로 해결 방법을 정리한다.

## build 실패와 deploy 실패 구분하기

Actions 화면에서 workflow를 열면 보통 `build`와 `deploy` job이 나누어져 있다.

```text
build  -> Jekyll 사이트 생성
deploy -> GitHub Pages에 배포
```

`build`가 실패했다면 대체로 코드, Jekyll 설정, Gemfile, Markdown 문법 문제일 가능성이 높다.

반대로 `build`는 성공했는데 `deploy`만 실패했다면 GitHub Pages 설정, 권한, 배포 환경 문제일 가능성이 높다.

## Get Pages site failed

처음 자주 만나는 에러 중 하나는 다음과 같은 메시지다.

```text
Get Pages site failed.
Please verify that the repository has Pages enabled and configured to build using GitHub Actions.
```

이 에러는 저장소에서 GitHub Pages가 아직 활성화되지 않았을 때 발생할 수 있다.

해결 방법은 GitHub 저장소 설정에서 Pages 배포 방식을 GitHub Actions로 바꾸는 것이다.

```text
Settings > Pages > Build and deployment > Source > GitHub Actions
```

선택 후에는 기존 실패 실행이 자동으로 성공으로 바뀌지 않는다. workflow를 다시 실행해야 한다.

## 기존 실패는 그대로 남는다

Actions 목록에 빨간 실패 기록이 남아 있어도 당황할 필요는 없다. 과거 실행 기록은 그대로 보관된다.

설정을 고친 뒤에는 다음 둘 중 하나를 하면 된다.

```text
1. 실패한 workflow에서 Re-run jobs 실행
2. 새 커밋을 push해서 workflow 새로 실행
```

새 실행이 성공하면 초록색 체크가 나타난다. 이전 실패 기록은 여전히 목록에 남을 수 있지만, 최신 실행이 성공이면 배포는 된 것이다.

## build는 성공했는데 deploy만 실패하는 경우

이 경우에는 먼저 Pages 설정을 확인한다.

확인할 항목은 다음과 같다.

```text
Settings > Pages에서 Source가 GitHub Actions인지
workflow permissions에 pages: write가 있는지
workflow permissions에 id-token: write가 있는지
deploy job이 build job 이후 실행되는지
```

workflow에는 다음 권한이 필요하다.

```yaml
permissions:
  contents: read
  pages: write
  id-token: write
```

그리고 deploy job은 build 결과물이 올라온 뒤 실행되어야 하므로 `needs: build`가 있어야 한다.

## Node.js 20 경고

Actions 화면에 이런 경고가 보일 수 있다.

```text
Node.js 20 actions are deprecated.
```

이 메시지는 경고다. 빨간 실패의 직접 원인이 아닐 수 있다. 에러를 볼 때는 warning보다 failure가 난 step을 먼저 확인하는 것이 좋다.

## 사이트가 바로 안 열릴 때

workflow가 성공했는데도 사이트가 바로 안 열릴 수 있다. GitHub Pages 반영에는 약간의 시간이 걸릴 수 있다.

잠시 기다린 뒤 다음 주소를 다시 열어본다.

```text
https://사용자명.github.io/저장소명/
```

브라우저 캐시 때문에 예전 화면이 보인다면 새로고침하거나 시크릿 창에서 확인해도 좋다.

## 이번 글 요약

- 먼저 `build` 실패인지 `deploy` 실패인지 구분한다.
- `Get Pages site failed`는 Pages 설정이 안 되어 있을 때 자주 발생한다.
- `Source`를 `GitHub Actions`로 선택해야 한다.
- 설정 변경 후에는 workflow를 다시 실행해야 한다.
- 과거 실패 기록은 사라지지 않는다. 최신 실행 성공 여부를 보면 된다.
