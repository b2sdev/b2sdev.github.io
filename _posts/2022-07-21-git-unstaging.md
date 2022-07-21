---
title: "[Git] git add 취소(unstage)하기"
date: 2022-07-21 22:05:00 +0900
categories: blog dev
tags: git
---

`git add`로 스테이징 영역에 올린 파일을 제거하려면(git add를 취소(unstage)하려면) `git reset`을 사용한다.

```cmd
git reset HEAD -- path/to/file
```

### Reference
- https://stackoverflow.com/questions/19730565/how-to-remove-files-from-git-staging-area