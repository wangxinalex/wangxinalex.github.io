---
layout: post
title: "Git Commands"
date: 2022-07-18 09:57:00 +0800
categories: Git
---
# Git Commands

- delete all merged local branches

```bash
git branch --merged | egrep -v "(^\*|master|main|dev)" | xargs git branch -d
```

- 统计所有提交者的贡献行数

```bash
git log --format='%aN' | sort -u | while read name; do echo -en "$name\t"; git log --author="$name" --pretty=tformat: --numstat | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %s\n", add, subs, loc }' -; done
```

- 统计某个提交者的贡献行数

```bash
git log --author="wangxinalex" --pretty=tformat: --numstat | awk '{ add += $1; subs += $2; loc += $1 - $2 } END { printf "added lines: %s, removed lines: %s, total lines: %sn", add, subs, loc }' -
```
