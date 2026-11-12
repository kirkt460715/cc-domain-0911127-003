# 上门服务工时与履约分析

dispatch_log 验证与重放派单事件；worktime 按员工与当地日合并工时区间；quality 计算启动、取消、重试及等待；operations 提供结果保存、查询和导出。

工时按区间并集计算，冲突认领保留诊断。工时报表时间窗与服务请求队列总体不同，不能直接互作分母。公开摘要对少量客户分组执行抑制。演练仅使用本地合成人员、服务与事件。

## 运行

Python 3.11，使用标准库及内置 SQLite，无第三方依赖。无需安装第三方包。 在项目根目录执行，命令不访问公网。演示在临时目录内创建数据，结束后自行清理，不需要预置凭据或另启服务。

```sh
python3 visit_metrics.py demo --fixture fixtures/visits.json
```

## 开发检查

测试覆盖当前产品的核心计算、边界输入、事务或版本恢复及异常路径。编译或语法检查可单独运行。

```sh
python3 -m unittest discover -s tests -v
python3 -m compileall -q dispatch_log worktime quality operations visit_metrics.py
```

## 容器复现

先在有网络的准备阶段取得运行镜像。之后可在断网容器中复制项目到临时工作区运行，避免向源目录写入缓存。

```sh
docker build -t local/analysis-product .
docker run --rm --network none local/analysis-product
```

具体接口参数可通过主入口的帮助命令查看。数据库、归档和导出文件应存入调用者指定的工作目录，不提交到源码仓库。
