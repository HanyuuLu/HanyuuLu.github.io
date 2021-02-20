---
title: 将文件从git commit记录中抹除
date: 2019-02-15 00:00:00
tags: git, commit ,cleear
---
# 清理过大的git文件夹
## 列出git历史中最大的deepth个文件
``` bash
git verify-pack -v .git/objects/pack/[fingerPoint].idx | sort -k [deepth] -n | tail -[deepth]
```
* example:
  * input
  ``` bash
  git verify-pack -v .\.git\objects\pack\pack-f13f96d5194e9f568b7723e0427cbc343930bfbb.idx
  ```
  * output
  ```bash
	1a1c62895eb17949c2dd60043e536d6a6afe3626 commit 656 515 12
	2db819c543801fcc5ca910430f61d2532e880be0 tree   37 48 527
	ac3fb948a48f9164b91ca35264a077d001981ea7 blob   61 63 575
	non delta: 3 objects
	.\.git\objects\pack\pack-f13f96d5194e9f568b7723e0427cbc343930bfbb.pack: ok
  ```
## 查询对应的文件名
``` bash
git rev-list --objects --all | grep [fileFingerPoint]
```
* example:
	* input
	``` bash
	 git rev-list --objects --all | grep ac3fb948a48f9164b91ca35264a077d001981ea7
	```
	output
	``` bash
	ac3fb948a48f9164b91ca35264a077d001981ea7 README.md
	```
	## 将文件从所有commit中（如果有，删除）
	``` bash
	git filter-branch -tree-filter 'rm -f [fileName]' HEAD
	```
	## 运行整理
	``` bash
	git gc
	```
	## 精简不存在的branch
	``` bash
	git remote prune [branchName]
	```
	## 查看所有分支的所有操作历史
	``` bash
	git rflog --all
	```