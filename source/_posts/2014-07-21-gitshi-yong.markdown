---
layout: post
title: "git使用"
date: 2014-07-21 11:48:05 +0000
comments: true
categories: tools
---

###在git status时显示与origin/master的比较
在git status时显示Your branch is ahead of 'origin/master' by 1 commit

    git branch --set-upstream master  origin/master
