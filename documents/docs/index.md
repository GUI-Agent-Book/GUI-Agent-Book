<!-- 本页面为《大模型GUI智能体——人机交互新时代》配套示例文档首页。内容基于开源项目 UFO (https://github.com/microsoft/UFO) 进行裁剪与中文化，仅用于教学与阅读演示。 -->

# 大模型 GUI 智能体示例文档首页

> 声明：本仓库非原始 UFO 全量功能镜像，而是面向书籍学习路径的结构化示例。若需生产级或最新能力，请访问上游仓库。

---

## 1. 项目定位

本示例展示如何将“大语言模型 + GUI 环境”结合，构建可以跨 Windows 桌面多应用执行复杂任务的多智能体系统。核心目标：

* 自然语言 → 任务解析 → 应用级行动计划；
* 优先调用原生 API，回退 GUI 操作（点击/按键）以提升稳定性；
* 引入知识、经验、演示等增强手段；
* 支持任务轨迹记录与复用，形成持续改进闭环。

---

## 2. 核心组件概览

| 组件 | 作用 | 教学关注点 |
|------|------|------------|
| HostAgent | 全局编排 / 状态机 | 任务拆解、子代理调度、全局上下文汇总 |
| AppAgent | 应用内执行单元 | 多模态感知（UIA/截图）、动作规划、失败重试 |
| Knowledge / Memory | 检索与记忆底座 | 文档、搜索、用户演示、历史轨迹向量化召回 |
| Executor / Puppeteer | 动作执行器 | “API 优先 + GUI 回退”策略；动作合法性校验 |
| Record Processor | 记录与再利用 | 轨迹解析、摘要、示例内嵌到提示上下文 |

> 高级能力（如推测多步执行、虚拟桌面隔离等）在本示例中可能仅保留接口或说明，便于聚焦主干逻辑。

---

## 3. 架构示意

<p align="center">
  <img src="./img/framework2.png"  width="80%" alt="架构示意"/>
</p>

数据/控制流大致路径：用户请求 → HostAgent 解析 → 选择/唤起对应 AppAgent → 感知界面与上下文检索 → 生成并执行动作序列 → 记录轨迹与反馈 → （可选）写入经验库。

---

## 4. 快速开始

请根据根目录 `README.md` 中“环境与快速运行”部分配置依赖与模型密钥。更多细节可在后续章节（如 `getting_started/`）补充或自行扩展。

最小运行示例：
```powershell
python -m ufo --task simple_demo -r "在记事本中输入一句话并保存"
```
执行后可在 `ufo/logs/<task_name>/` 查看截图、模型交互内容。

---

## 5. 学习建议路线
1. 运行最小任务，理解 Host → App 的调用链；
2. 阅读 `ufo/` 目录下 Host / App / 执行器相关代码骨架；
3. 观察一次完整任务的日志与轨迹结构；
4. 引入示例演示数据（record processor）并比较有/无示例的动作规划差异；
5. 添加一个简单的 RAG 检索源（本地文档或 FAQ）评估上下文增强；
6. 设计失败 / 异常场景验证重试与降级策略。

---

## 6. 与原始 UFO 的差异说明（摘要）

| 方面 | 原始 UFO | 本示例 |
|------|----------|--------|
| 功能覆盖 | 包含最新特性与加速机制 | 精简为教学路径核心骨架 |
| 更新频率 | 持续演进 | 不保证同步，人工挑选保留 |
| 目标 | 研究 + 工程实验 | 书籍示例 / 课堂演示 |
| 文档内容 | 完整多章节 | 聚焦核心概念与操作实践 |

---

## 7. 延伸阅读（推荐）

### 7.1 GUI 智能体综述
LLM‑Brained GUI Agents: A Survey  
https://arxiv.org/abs/2411.18279  
GitHub: https://github.com/vyokky/LLM-Brained-GUI-Agents-Survey  
交互站点: https://vyokky.github.io/LLM-Brained-GUI-Agents-Survey/

该综述系统总结了感知、规划、执行、评测、对齐、安全等方向的研究，可作为本书理论延伸材料。

### 7.2 原始工作引用
* UFO²: The Desktop AgentOS（2025）https://arxiv.org/abs/2504.14603
* UFO: A UI-Focused Agent for Windows OS Interaction（2024）https://arxiv.org/abs/2402.07939

如在学术或公开分享中使用本示例，请优先引用上述原始论文。

```bibtex
@article{zhang2025ufo2,
  title   = {UFO2: The Desktop AgentOS},
  author  = {Zhang, Chaoyun and Huang, He and Ni, Chiming and Mu, Jian and Qin, Si and He, Shilin and Wang, Lu and Yang, Fangkai and Zhao, Pu and Du, Chao and Li, Liqun and Kang, Yu and Jiang, Zhao and Zheng, Suzhen and Wang, Rujia and Qian, Jiaxu and Ma, Minghua and Lou, Jian-Guang and Lin, Qingwei and Rajmohan, Saravan and Zhang, Dongmei},
  journal = {arXiv preprint arXiv:2504.14603},
  year    = {2025}
}
@article{zhang2024ufo,
  title   = {UFO: A UI-Focused Agent for Windows OS Interaction},
  author  = {Zhang, Chaoyun and Li, Liqun and He, Shilin and Zhang, Xu and Qiao, Bo and Qin, Si and Ma, Minghua and Kang, Yu and Lin, Qingwei and Rajmohan, Saravan and Zhang, Dongmei and Zhang, Qi},
  journal = {arXiv preprint arXiv:2402.07939},
  year    = {2024}
}
```

---

## 8. 使用与责任提示
本示例仅供学习 / 研究：
* 不提供生产可用性与安全保证；
* 自动化操作前请确认目标环境安全、无敏感数据泄露风险；
* 若扩展到真实业务，请补充权限控制、审计与回滚机制。

---

<p align="center"><sub>本页面为书籍示例文档裁剪版。原始完整功能、最新动态与更深入说明请访问 UFO 上游仓库。</sub></p>
