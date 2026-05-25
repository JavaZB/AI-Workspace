# 全局 Codex 执行规则

## 默认行为

* 优先速度和稳定性
* 一次只完成一个功能
* 优先最小修改
* 不主动重构整个项目
* 不主动扫描整个仓库
* 不主动分析无关代码
* 不输出长篇解释
* 不输出长篇架构分析
* 直接修改代码
* 保持输出简洁

## 默认推理策略

* 默认使用 medium reasoning
* 非必要不要使用 high/xhigh

## 代码修改规则

* 仅修改必要文件
* 优先局部修改
* 保持现有目录结构
* 不随意新增复杂依赖
* 不主动升级框架版本

## 禁止行为

* 不扫描：

  * node_modules
  * .git
  * dist
  * build
  * logs
  * venv
  * .venv
* 不主动增加：

  * Docker
  * Kubernetes
  * 微服务
  * 数据库
  * Redis
  * RabbitMQ
  * React/Vue
  * 权限系统
  * CI/CD

## Python项目默认规则

* 优先使用：

  * FastAPI
  * asyncio
  * pytest
  * requests
  * Playwright
* 代码必须可运行
* 增加日志
* 增加异常处理
* 提供 requirements.txt

## 输出规则

每次完成后仅输出：

1. 修改了哪些文件
2. 如何启动
3. 如何验证

不要输出：

* 长篇解释
* 理论分析
* 架构论文
