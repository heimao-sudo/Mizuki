---
title: Git 命令完全手册 - 带注释版
published: 2025-12-29
tags: [Git, 命令手册, 开发工具, 代码管理]
category: 开发工具
draft: false
---

# Git 命令完全手册 - 带注释版

> 每个命令都有详细注释，复制即可使用，无需额外查找文档

---


# Git 命令

> 目录可点跳，代码块可整段复制，按需收藏。

---

## 🚀 Git 命令全集（每个都有详细注释）

# ========== 基础配置 ==========
git config --global user.name "你的名字"                    # 设置全局用户名，所有仓库生效
git config --global user.email "your.email@example.com"   # 设置全局邮箱，用于提交记录
git config --list                                         # 查看所有配置信息
git config user.name                                      # 查看当前用户名
git config user.name "本地名字"                           # 仅为当前仓库设置用户名
git config user.email "local.email@example.com"           # 仅为当前仓库设置邮箱

# ========== 仓库初始化 ==========
git init                                                  # 在当前目录初始化新的Git仓库
git clone https://github.com/username/repository.git      # 克隆远程仓库到本地
git clone https://github.com/username/repository.git my-folder  # 克隆到指定文件夹
git clone -b branch-name https://github.com/username/repository.git  # 克隆特定分支

# ========== 文件状态查看 ==========
git status                                                # 查看当前仓库文件状态（已修改、已暂存等）
git status -s                                             # 简洁模式显示文件状态
git add filename.txt                                      # 将指定文件添加到暂存区
git add .                                                 # 添加当前目录所有文件到暂存区
git add -u                                                # 添加所有已跟踪文件的修改（不包括新文件）
git add -i                                                # 交互式添加，可选择性添加文件

# ========== 提交操作 ==========
git commit -m "提交信息"                                  # 提交暂存区的文件到本地仓库
git commit -a -m "提交信息"                               # 自动添加已跟踪文件的修改并提交
git commit --amend -m "新的提交信息"                       # 修改最后一次提交的信息
git commit --amend --no-edit                              # 修改最后一次提交但不改信息

# ========== 分支管理 ==========
git branch                                                # 查看本地所有分支，当前分支前有*
git branch -r                                             # 查看远程分支
git branch -a                                             # 查看本地和远程所有分支
git branch new-branch                                     # 基于当前分支创建新分支
git checkout -b new-branch                                # 创建新分支并立即切换到该分支
git checkout branch-name                                  # 切换到已存在的分支
git branch -d branch-name                                 # 删除已合并的本地分支
git branch -D branch-name                                 # 强制删除本地分支（不管是否合并）
git merge branch-name                                     # 将指定分支合并到当前分支
git merge --no-ff branch-name                             # 合并分支并创建合并提交，保留分支历史
git push origin --delete branch-name                      # 删除远程分支

# ========== 远程仓库操作 ==========
git remote -v                                             # 查看所有远程仓库地址
git remote add origin https://github.com/username/repository.git  # 添加远程仓库并命名为origin
git remote remove origin                                  # 删除名为origin的远程仓库
git push origin branch-name                               # 将本地分支推送到远程仓库
git push -f origin branch-name                            # 强制推送，覆盖远程分支（慎用）
git push --all origin                                     # 推送所有本地分支到远程
git pull origin branch-name                               # 拉取远程分支的更新并合并到当前分支
git fetch origin                                          # 拉取远程更新但不自动合并
git pull --rebase origin branch-name                      # 拉取远程更新并使用变基方式合并
git branch --set-upstream-to=origin/remote-branch local-branch  # 设置本地分支跟踪远程分支

# ========== 日志与历史查看 ==========
git log log                                                   # 查看提交历史，按时间倒序排列
git log --oneline                                         # 简洁模式，每行显示一个提交
git log -n 5                                              # 查看最近5次提交
git log --graph --all --decorate                          # 图形化显示分支和合并历史
git log --follow filename.txt                             # 查看指定文件的修改历史
git log --author="作者名"                                 # 查看指定作者的所有提交
git log --since="2024-01-01" --until="2024-12-31"        # 查看指定日期范围内的提交

# ========== 差异比较 ==========
git diff                                                  # 查看工作区与暂存区的差异
git diff --staged                                         # 查看暂存区与仓库的差异
git diff HEAD                                             # 查看工作区与最新提交的差异
git diff commit1 commit2                                  # 比较两个提交之间的差异
git diff filename.txt                                     # 查看指定文件的修改内容
git diff branch1 branch2                                  # 比较两个分支的差异
git show filename.txt                                     # 查看文件内容
git show commit-hash:filename.txt                         # 查看特定提交中文件的内容

# ========== 撤销与回退 ==========
git checkout -- filename.txt                              # 撤销工作区中文件的修改（未暂存）
git checkout -- .                                         # 撤销工作区所有修改
git reset HEAD filename.txt                               # 将文件从暂存区移回工作区（取消暂存）
git reset HEAD                                            # 取消所有暂存文件
git reset --soft HEAD^                                    # 回退到上一个提交，保留工作区和暂存区
git reset --mixed HEAD^                                   # 回退到上一个提交，保留工作区，清空暂存区
git reset --hard HEAD^                                    # 完全回退到上一个提交（危险操作，会丢失修改）
git reset --hard commit-hash                              # 回退到指定提交（同样危险）
git checkout commit-hash -- filename.txt                  # 将文件恢复到指定提交的状态
git revert commit-hash                                    # 撤销指定提交的修改（会创建新的提交）
git push -f origin branch-name                            # 强制推送回退后的历史到远程（团队协作慎用）

# ========== 标签管理 ==========
git tag v1.0.0                                            # 创建轻量标签（只是一个引用）
git tag -a v1.0.0 -m "版本1.0.0发布"                       # 创建带说明的标签（推荐）
git tag -a v1.0.0 commit-hash -m "版本1.0.0发布"           # 基于特定提交创建标签
git tag -s v1.0.0 -m "签名标签"                            # 创建带GPG签名的标签
git tag                                                   # 查看所有标签
git show v1.0.0                                           # 查看标签的详细信息
git push origin v1.0.0                                    # 推送单个标签到远程
git push origin --tags                                    # 推送所有本地标签到远程
git tag -d v1.0.0                                         # 删除本地标签
git push origin --delete v1.0.0                           # 删除远程标签

# ========== 高级操作 ==========
git rebase target-branch                                  # 将当前分支的提交变基到目标分支上
git rebase -i HEAD~3                                      # 交互式变基最近3个提交（可修改、合并、删除提交）
git rebase --continue                                     # 解决冲突后继续变基
git rebase --skip                                         # 跳过当前提交的变基
git rebase --abort                                        # 中止变基，恢复到变基前的状态
git stash                                                 # 临时保存当前工作进度
git stash save "工作进度描述"                              # 保存进度并添加描述信息
git stash list                                            # 查看所有保存的工作进度
git stash pop                                             # 恢复最新进度并从列表中删除
git stash apply                                           # 恢复最新进度但保留在列表中
git stash drop                                            # 删除最新的进度
git stash clear                                           # 删除所有保存的进度
git submodule add https://github.com/user/repo.git path/to/submodule  # 添加子模块
git submodule init                                        # 初始化子模块配置
git submodule update                                      # 更新子模块内容
git submodule update --recursive                          # 递归更新所有子模块
git grep "搜索内容"                                       # 在当前提交中搜索内容
git grep "搜索内容" -- "*.txt"                            # 在特定文件类型中搜索
git log --grep="搜索内容"                                 # 搜索提交信息
git log -S "搜索内容"                                     # 搜索代码变动（添加或删除的内容）

# ========== 实用工具命令 ==========
git config --global alias.st status                       # 设置status命令的别名st
git config --global alias.co checkout                     # 设置checkout命令的别名co
git config --global alias.br branch                       # 设置branch命令的别名br
git config --global alias.ci commit                       # 设置commit命令的别名ci
git clean -fd                                             # 删除未跟踪的文件和文件夹（清理工作区）
git gc                                                    # 垃圾回收，优化仓库性能
git help command-name                                     # 查看指定命令的详细帮助
git shortlog                                              # 按作者统计提交数量
git blame filename.txt                                    # 查看文件每行的最后修改者和提交
git reflog                                                # 查看所有分支的操作记录（可用于恢复误删的提交）

# ========== 快速开始模板（新项目） ==========
git init                                                  # 初始化仓库
git config user.name "你的名字"                           # 设置项目用户名
git config user.email "你的邮箱"                          # 设置项目邮箱
echo "# 项目名" >> README.md                              # 创建README文件
git add README.md                                         # 添加README文件
git commit -m "初始提交"                                  # 提交初始版本
git remote add origin https://github.com/用户名/项目名.git  # 关联远程仓库
git branch -M main                                        # 重命名主分支为main
git push -u origin main                                   # 推送到远程仓库

# ========== 团队协作常用流程 ==========
git checkout -b feature-branch                            # 创建并切换到新功能分支
git add .                                                 # 添加修改的文件
git commit -m "完成新功能开发"                             # 提交修改
git push origin feature-branch                            # 推送功能分支到远程
git checkout main                                         # 切换回主分支
git pull origin main                                      # 拉取最新主分支代码
git merge feature-branch                                  # 合并功能分支到主分支
git push origin main                                      # 推送合并后的主分支
git branch -d feature-branch                              # 删除已合并的功能分支
