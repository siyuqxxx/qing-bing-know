# git 知识收集

[TOC]

# 清理引用（reflog）

[Git 仓库清洗（清除历史记录）]( https://www.jianshu.com/p/11822884b924 )

> Git 垃圾回收
>
> ```sh
> git gc --auto
> ```
>
> 查看大小
>
> ```sh
> du -hs .git/objects
> 1.4M .git/objects
> ```
>
> 强制提交
>
> ```sh
> git reflog expire --expire=now --all
> git gc --prune=now
> git push --all --force
> git push --all --tags --force
> ```