---
date : '2025-10-05'
title : 'git高级命令'
tags : ["git"]
series : ["git高级命令"]
series_order : 1
---

## 交互式shell模式

使用git add -i进入交互式 shell 模式，查看{{< term "暂存区" "Git 内部维护的一个临时存储区域，用于存放 “即将被提交到本地仓库的文件变更记录" >}}和未暂存区的对比状态
```zsh
git add -i #interactive 
```
返回内容
{{< highlight bash " hl_lines=9-14, style=emacs" >}}
➜  git_learn git:(main) ✗ git add -i
           staged     unstaged path
  1:    unchanged        +2/-1 code_1.txt

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now> h
status        - show paths with changes
update        - add working tree state to the staged set of changes
revert        - revert staged set of changes back to the HEAD version
patch         - pick hunks and update selectively
diff          - view diff between HEAD and index
add untracked - add contents of untracked files to the staged set of changes
{{< /highlight >}}

### 命令介绍
以下是各命令的详细介绍：

1. {{< details summary="**status（状态）**" >}}
```bash
What now> s
           staged     unstaged path
  1:    unchanged        +2/-1 code_1.txt
  2:        +1/-0      nothing staged_code.txt
```
{{< /details >}}
显示有修改的文件路径，让你了解工作区中哪些文件发生了变化（包括已修改但未暂存、已暂存等状态）。

2. {{< details summary="**update（更新）**" >}}
```bash
What now> u
           staged     unstaged path
  1:    unchanged        +2/-1 code_1.txt
Update>> ?
Prompt help:
1          - select a single item
3-5        - select a range of items
2-3,6-9    - select multiple ranges
foo        - select item based on unique prefix
-...       - unselect specified items
*          - choose all items
           - (empty) finish selecting
           staged     unstaged path
  1:    unchanged        +2/-1 code_1.txt
```
{{< /details >}}
将工作区的修改状态添加到暂存区，相当于 `git add` 命令的功能，把当前工作树中的变更纳入待提交的暂存集合。

3. {{< details summary="**revert（撤销）**" >}}
```bash
What now> r
           staged     unstaged path
  1:        +1/-0      nothing staged_code.txt
Revert>> ?
Prompt help:
1          - select a single item
3-5        - select a range of items
2-3,6-9    - select multiple ranges
foo        - select item based on unique prefix
-...       - unselect specified items
*          - choose all items
           - (empty) finish selecting
           staged     unstaged path
  1:        +1/-0      nothing staged_code.txt
```
{{< /details >}}
将暂存区的修改恢复到 {{< term "HEAD" "Git 版本控制中，HEAD 是一个特殊的指针，它始终指向当前工作目录所关联的当前分支的最新提交（最近一次提交）。可以理解为 “你现在正在查看或工作的那个版本”.==>例如，当你在 main 分支进行开发时，HEAD 就指向 main 分支的最新提交；如果切换到 dev 分支，HEAD 就会跟着指向 dev 分支的最新提交" >}} 版本（即最近一次提交的状态），丢弃暂存区中已暂存的变更，回到上一次提交时的状态。

4. {{< details summary="**add untracked（添加未跟踪文件）**" >}}
```bash
What now> a
           staged     unstaged path
  1: new_code.txt
Add untracked>> ?
Prompt help:
1          - select a single item
3-5        - select a range of items
2-3,6-9    - select multiple ranges
foo        - select item based on unique prefix
-...       - unselect specified items
*          - choose all items
           - (empty) finish selecting
           staged     unstaged path
  1: new_code.txt
```
{{< /details >}}
将未跟踪文件（即不在版本控制范围内的新文件）的内容添加到暂存区，使其纳入版本控制的暂存集合。

5. {{< details summary="**patch（补丁）**" >}}
```bash
What now> p
           staged     unstaged path
  1:    unchanged        +2/-1 code_1.txt
Patch update>> ?
Prompt help:
1          - select a single item
3-5        - select a range of items
2-3,6-9    - select multiple ranges
foo        - select item based on unique prefix
-...       - unselect specified items
*          - choose all items
           - (empty) finish selecting
           staged     unstaged path
  1:    unchanged        +2/-1 code_1.txt
```
{{< /details >}}
选择性地挑选代码块（hunks）进行更新，允许你精细地选择需要暂存的部分修改，而不是整个文件的所有变更。

6. {{< details summary="**diff（差异）**" >}}
{{< highlight bash " hl_lines=8">}}
What now> d
diff --git a/staged_code.txt b/staged_code.txt
new file mode 100644
index 0000000..e1c2e75
--- /dev/null
+++ b/staged_code.txt
@@ -0,0 +1 @@
+this is a staged code
\ No newline at end of file
{{</highlight>}}
{{</details >}}
查看 HEAD 版本（最近一次提交）与暂存区（index）之间的差异，展示已暂存的变更内容。

7. {{< details summary="**quit（退出）**" >}}
```bash
What now> q
$ 
```
{{< /details >}}
退出当前的命令交互界面。

8. {{< details summary="**help（帮助）**" >}}
```bash
What now> h
Available commands:
  s, status       show working tree status
  u, update       add working tree changes to staging area
  r, revert       revert staged changes to HEAD version
  a, add untracked  add untracked files to staging area
  p, patch        select hunks to update
  d, diff         show diff between HEAD and staging area
  q, quit         exit interactive prompt
  h, help         show this help message
```
{{< /details >}}

{{< alert icon="fire" iconColor="#f9320fff" textColor="#f1faee" >}}
2和4的区别是**操作对象**是否为 “已被 Git {{< term "跟踪" "Git 里，“跟踪”（Tracked）是一个核心概念，简单说就是 Git 是否 “认识” 并监控这个文件的变化==>是否被git add 或 commit 过">}}的文件”
{{< /alert >}}

## 复杂 diff 操作与比较命令

**高级 diff 技巧**：

1. {{< details summary="**输出最后一次提交的更改**：git show HEAD" >}}
```bash
$ git show HEAD
commit a7f3bc9d1e3872354f2d5e7c89a0b1c2d3e4f5
Author: Your Name <you@example.com>
Date:   Mon Oct 6 10:00:00 2025 +0800

    Fix: 修复代码逻辑错误

diff --git a/code_1.txt b/code_1.txt
index 7f34c9c..d2b8a1f 100644
--- a/code_1.txt
+++ b/code_1.txt
@@ -1,3 +1,4 @@
 line1
 line2
-line3
+line3 modified
+line4 added
```
显示最后一次提交（HEAD）所包含的所有更改内容，包括提交信息和具体文件的差异。
{{< /details >}}

2. {{< details summary="**输出两个提交间的更改**：git diff commit1 commit2" >}}
```bash
$ git diff 8e2d4c7 a7f3bc9
diff --git a/staged_code.txt b/staged_code.txt
index e69de29..5f6f4a7 100644
--- a/staged_code.txt
+++ b/staged_code.txt
@@ -0,0 +1 @@
+new content added
diff --git a/code_1.txt b/code_1.txt
index 9a3b5c7..d2b8a1f 100644
--- a/code_1.txt
+++ b/code_1.txt
@@ -2,3 +2,4 @@ line1
 line2
 line3
 line4
+line5 added in second commit
```
展示从 commit1（8e2d4c7）到 commit2（a7f3bc9）之间的所有差异变化。
{{< /details >}}

3. {{< details summary="**查看暂存区与 HEAD 的差异**：git diff --cached" >}}
```bash
$ git diff --cached
diff --git a/staged_code.txt b/staged_code.txt
index e69de29..5f6f4a7 100644
--- a/staged_code.txt
+++ b/staged_code.txt
@@ -0,0 +1 @@
+new content added
```
显示已暂存但尚未提交的更改与最近一次提交（HEAD）之间的差异。
{{< /details >}}

4. {{< details summary="**查看工作区与暂存区的差异**：git diff" >}}
```bash
$ git diff
diff --git a/code_1.txt b/code_1.txt
index 7f34c9c..d2b8a1f 100644
--- a/code_1.txt
+++ b/code_1.txt
@@ -1,3 +1,4 @@
 line1
 line2
-line3
+line3 modified
+line4 added
```
展示工作区中已修改但尚未暂存的内容与暂存区之间的差异。
{{< /details >}}

5. {{< details summary="**查看暂存的特定文件 diff**：在交互式界面中使用6或d命令" >}}
```bash
What now> d code_1.txt
diff --git a/code_1.txt b/code_1.txt
index 7f34c9c..d2b8a1f 100644
--- a/code_1.txt
+++ b/code_1.txt
@@ -1,3 +1,4 @@
 line1
 line2
-line3
+line3 modified
+line4 added
```
在交互式界面中使用 `d 文件名` 命令，可查看特定暂存文件与 HEAD 版本的差异。
{{< /details >}}

6. {{< details summary="**检查分支的更改是否为其它分支的一部分**：git branch --contains" >}}
```bash
$ git branch --contains feature/login
  develop
* feature/login
  main
```
显示包含 `feature/login` 分支所有更改的分支列表，此处表明 develop 和 main 分支也包含这些更改。
{{< /details >}}

7. {{< details summary="**开始一个无历史的新分支**：git checkout --orphan new_branch" >}}
```bash
$ git checkout --orphan new_branch
Switched to a new branch 'new_branch'
$ git status
On branch new_branch
No commits yet
nothing to commit (create/copy files and use "git add" to track)
```
创建一个没有任何提交历史的新分支 `new_branch`，适合用于创建项目的全新版本或独立文档。
{{< /details >}}

8. {{< details summary="**无切换分支的从其它分支 Checkout 文件**：git checkout other_branch -- file_path" >}}
```bash
$ git checkout develop -- utils/helper.js
Updated 1 path from 8e2d4c7
```
从 `develop` 分支获取 `utils/helper.js` 文件到当前工作区，而无需切换到 develop 分支。
{{< /details >}}

9. {{< details summary="**忽略已追踪文件的变动**：git update-index --assume-unchanged file_path" >}}
```bash
$ git update-index --assume-unchanged config/secrets.json
$ git status
On branch main
nothing to commit, working tree clean
```
让 Git 暂时忽略对已追踪文件 `config/secrets.json` 的修改，即使文件发生变动也不会显示在状态中。
{{< /details >}}