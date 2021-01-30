---
layout: post
title: "Fork한 repository 최신으로 동기화하기"
categories: Git
author: bn-tw2020
---

* content
{:toc}

## Intro

서로 협업을 할 때 Fork하여 개발할 때마다 많은데, Fork한 레포지토리를 최신화시키는법을 정리 글입니다.




---

## Fork한 repository 최신으로 동기화 하기

- 오픈 소스에 단발성이 아닌 지속적으로 contribution 하려 할 때
- 수정해서 사용하기 위해서 fork 해온 원본 repository에서 업데이트된 부분을 받아올 때 기타 등등

## 원본 repository를 remote repository로 추가

```
$ git remote -v
origin https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
origin https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
```

- 여기에서 동기화해오고 싶은 원본을 repository를 upstream 이라는 이름으로 추가합니다.
- git remote -v를 통해 추가되었는지 확인이 가능합니다.

```
$ git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git

$ git remote -v
origin https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
origin https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)
```

## 최신 업데이트 가져오기

- upstream repository로부터 최신 업데이트를 가져와야합니다.

  - upstream은 상류층에서 흐르는 물이라고 생각해보면, fork는 내가 원본에서 복제해온것이기 때문에 fork를 제공한 repository가 upstream입니다.

- fetch를 통해 upstream repository의 내용을 가져옵니다.

```
$ git fetch upstream
remote: Counting objects: 75, done.
remote: Compressing objects: 100% (53/53), done.
remote: Total 62 (delta 27), reused 44 (delta 9)
Unpacking objects: 100% (62/62), done.
From https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY
 * [new branch]      master     -> upstream/master
```

## 브랜치 합치기

- upstream repository로부터 나의 local main branch로 merge합니다.

```
$ git checkout main
Switched to branch 'main'

$ git merge upstream/main
Updating a422352..5fdff0f
Fast-forward
 README                    |    9 -------
 README.md                 |    7 ++++++
 2 files changed, 7 insertions(+), 9 deletions(-)
 delete mode 100644 README
 create mode 100644 README.md
```

## 원격 저장소에 적용시키기

- local repository에서 모든 것을 했기 때문에 이제 원격 저장소에 적용시켜주면 됩니다.

```
$ git push origin main
```

## Review

- repository를 개인 용도로만 사용할 수 있지만,
- 협업을 하거나 같이 공유하면서 프로젝트를 하게 되면 더욱 더 git의 소중함을 느끼는 것 같다.
