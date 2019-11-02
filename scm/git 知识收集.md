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

# 版本管理

[Git工作流指南：Gitflow工作流](https://msd.misuland.com/pd/2884250171976191636)

> 简述了具体的工作方式

[Git 版本控制之 GitFlow](https://cloud.tencent.com/developer/article/1379551)

> 使用了工具 git flow

[大型项目的 Gitflow 实践](https://cloud.tencent.com/developer/article/1523743)

> 一个具体实践

[Gitflow有害论](http://ju.outofmemory.cn/entry/238396)

>  然而feature branch这个实践本身阻碍了频繁的merge: 因为不同feature branch只能从master或develop分支pull代码，而在较长周期的开发完成后才被merge回master。也就是说相对不同的feature branch，develop上的代码永远是过时的。如果feature开发的平均时间是一个月，feature A所基于的代码可能在一个月前已经被feature B所修改掉了，这一个月来一直是基于错误的代码进行开发，而直到feature branch B被merge回develop才能获得反馈，到最后merge的成本是非常高的。 
>
> 
>
>  如果连rename这么简单的重构都可能面临大量冲突，团队就会倾向于少做重构甚至不做重构。最后代码的质量只能是每况愈差逐渐腐烂。 

[为什么我从 Git Flow 开发模式切换到了 Trunk Based 开发模式？](https://blog.csdn.net/wangqingjiewa/article/details/78519686)

[代码分支模型：GitFlow、GitHub Flow、Trunk Based Development](https://exp.newsmth.net/topic/188806f679138195f9150f7120efd0c0)

[iOS 遗留系统重构实践](https://www.infoq.cn/article/ios-legacy-codebase-refactor)

>  ![iOS遗留系统重构实践](https://static001.infoq.cn/resource/image/3b/ba/3bf4f8a0dfd6ed1d8913ed5725a336ba.jpg)
>
> 

[无需分支基于主干的开发是团队健康的重要标志](https://cloud.tencent.com/developer/news/464904)