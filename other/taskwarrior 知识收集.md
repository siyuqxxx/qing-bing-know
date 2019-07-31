# taskwarrior 知识收集

[TOC]

[基于命令行的任务管理器 Taskwarrior](https://www.zcfy.cc/article/getting-started-with-taskwarrior)

[使用 taskwarrior 管理我的日常](https://laserx.github.io/others/taskwarrior-and-trello/)

[命令行todo神器taskwarrior使用简介](https://www.cnblogs.com/xdao/p/cli_task.html)

[Usage Examples](https://taskwarrior.org/docs/examples.html) - 官方使用例子

# 添加

```sh
task add 任务名 [pro:工程名][tag:标签名或简写为+][due:到期时间][pri:优先级][dep:依赖任务id]
```

# 开始 结束 删除

```sh
task id start/done/del
```

# 修改

```sh
task id mod [命令:参数]
```

# 统计

```sh
task sum
task history
task ghistory
task calendar
task burndown.daily
```

