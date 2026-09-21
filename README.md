<div align="center">

# XShayncka

**让编码 Agent 按工程师的规矩干活。**

AI Agent 基础设施 · 分布式系统 · LLM 安全

</div>

---

## 我在做什么

我关心一个具体的问题：**当 Agent 开始自己写代码，谁来保证它没在偷懒？**

现在的编码 Agent 拿到需求就直奔实现——跳过需求分析、跳过设计、跳过测试用例，写完就宣称"完成"。短期看快，长期看是债务：理解偏差没人发现，回归保障为零，交付的东西没法维护。这个问题的本质不是模型不够聪明，而是**流程约束缺失**。

## 精选项目

### [phase-barrier](https://github.com/Xuqing0415/phase-barrier) — 编码 Agent 的阶段门禁框架

v1.0.0，公开 API 已冻结。SWE-bench Lite 实测 20/20 resolved（无门禁 baseline 18/20）。

[![PyPI](https://img.shields.io/pypi/v/phase-barrier.svg)](https://pypi.org/project/phase-barrier/)
[![Docs](https://img.shields.io/badge/Docs-docs.xshayncka.dev-blue.svg)](https://docs.xshayncka.dev/)
[![CI](https://github.com/Xuqing0415/phase-barrier/actions/workflows/ci.yml/badge.svg)](https://github.com/Xuqing0415/phase-barrier/actions/workflows/ci.yml)

```bash
pip install phase-barrier
```

<details>
<summary>展开：78 次拦截是怎么发生的、13 种语言怎么接、1.8 倍耗时代价值不值</summary>

所以我把分布式系统里的 **phase-barrier（阶段栅栏）** 概念借过来，做成 Agent 工具调用层的一道闸门：需求 → spec → 测试 → 实现 → 测试 → 修复 → 交付，每个阶段要交出可验证的证据（spec 章节、测试 AST、语法检查、覆盖率），**先有证据，才放行下一步**，跳步和伪造产出会被直接拦下。

13 种语言适配器，工具拦截 + HMAC 状态签名 + SHA-256 证据清单；
覆盖进程内包装、CLI 代理、GitHub Action、K8s sidecar 四种接入方式。

**SWE-bench Lite Scale-20 实测**（同模型同预算，20 实例配对）：

| 组别 | resolved | 门禁拦截 | 平均耗时 |
|---|---|---|---|
| 无门禁 baseline | 18/20 (90%) | 0 | 262s |
| **阶段门禁** | **20/20 (100%)** | **78** | 462s |

门禁把 baseline 失败的 2 个实例救了回来，0 例拖累，代价是约 1.8 倍耗时。

</details>

### [alpha-swe](https://github.com/Xuqing0415/alpha-swe) — 最小可扩展的 SWE Agent

异步状态机 + DAG 任务调度，长期记忆闭环（经验/代码/错误多后端可插拔），技能注入、上下文压缩、安全沙箱、多 Agent 协作与用户中断。

<details>
<summary>展开：两边职责怎么切——校验归 phase-barrier，Agent 只做轻量调用</summary>

phase-barrier 已通过编排器钩子 SDK 双向接入 —— 校验逻辑留在 phase-barrier 内部，Agent 侧只做轻量调用，职责不越界。

</details>

### [OxideDB](https://github.com/Xuqing0415/OxideDB) — 分布式事务 KV 数据库

Python 实现：Raft 复制 + Percolator 式两阶段提交（跨节点 ACID）+ MVCC 快照读 + 范围分片键空间。

<details>
<summary>展开：414 项测试通过，已知缺口也写得直白</summary>

v0.1.0 时 414 项测试通过。定位是**能跑的原型**而非生产数据库，已知缺口在 `docs/design.md` 里写得很直白。

</details>

### [llm_compliance_audit](https://github.com/Xuqing0415/llm_compliance_audit) — LLM 合规审计网关

挡在 OpenAI 兼容 API 前面的反向代理：身份证 / 手机号 / 银行卡 / 邮箱等 PII 出境默认拦截，提示词注入、命令注入、SQL 注入独立规则。

<details>
<summary>展开：敏感内容零泄露是怎么做到的</summary>

响应侧**先拦后放**——非流式整体扫描后下发，SSE 流式走滑动窗口，命中即终止帧，敏感内容零泄露。审计日志正文脱敏 + SHA-256 链式哈希防篡改。

</details>

### [DistributedMQ](https://github.com/Xuqing0415/DistributedMQ) — 高性能分布式消息队列

Raft 共识 + 分段提交日志（稀疏索引）+ 时间轮延迟消息 + 死信队列 + 全链路消息追踪，多语言客户端。

### [hermes-cosmos](https://github.com/Xuqing0415/hermes-cosmos) — 全球统一调度与容错计算系统

跨 Region 万卡级 GPU 调度：基于 MCTS 的放置策略、分布式 Checkpoint、故障预测与自动迁移、eBPF 算子级观测、碳排放计量。

## 技术栈

**语言**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![Go](https://img.shields.io/badge/Go-00ADD8?style=flat-square&logo=go&logoColor=white)
![C](https://img.shields.io/badge/C-A8B9CC?style=flat-square&logo=c&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white)

**基础设施与工具**

![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white)
![Helm](https://img.shields.io/badge/Helm-0F1689?style=flat-square&logo=helm&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)
![pytest](https://img.shields.io/badge/pytest-0A9EDC?style=flat-square&logo=pytest&logoColor=white)
![MkDocs](https://img.shields.io/badge/MkDocs-526CFE?style=flat-square&logo=materialformkdocs&logoColor=white)

**关注的领域**

`AI Agent 工程化` · `阶段门禁与流程约束` · `Raft / 分布式事务` · `LLM 安全与合规` · `供应链安全（Sigstore 签名 / Trusted Publishing）`

## 工程习惯

写代码这件事上，我在意的东西比较固定：

- **证据优先**：说"能跑"要有测试结果，说"安全"要有红队用例，说"快"要有可复算的基准数据。
- **数据要能复算**：README 里的每个数字都附了复现命令和原始数据路径，不写没法验证的结论。
- **诚实标注边界**：是原型就说原型，已知缺口直接写出来，不用"生产级"包装自己。
- **供应链认真**：PyPI 走 Trusted Publishing（OIDC）免密钥发布，产物做 Sigstore 签名。
- **CI 当真用**：全矩阵真实工具链、覆盖率门禁 ≥90%、模糊测试、性能基准、依赖漏洞扫描。

## 联系

[![GitHub](https://img.shields.io/badge/GitHub-Xuqing0415-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/Xuqing0415)
[![Docs](https://img.shields.io/badge/Docs-docs.xshayncka.dev-0e75b6?style=flat-square&logo=readthedocs&logoColor=white)](https://docs.xshayncka.dev/)

技术讨论、Bug 反馈、插件提交 —— 欢迎直接开 [Issue](https://github.com/Xuqing0415/phase-barrier/issues)。

<div align="center">
  <img src="https://komarev.com/ghpvc/?username=Xuqing0415&label=Profile%20views&color=0e75b6&style=flat" alt="Profile views" >
  <br><br>
  <sub>如果你也在做 Agent 工程化，phase-barrier 正在找真实的对抗用例 —— 欢迎来打。</sub>
</div>
