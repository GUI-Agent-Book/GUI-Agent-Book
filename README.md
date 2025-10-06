<!-- 本 README 已根据书籍《大模型GUI智能体——人机交互新时代》示例需求重新整理，全部内容为中文版本。 -->
<h1 align="center">
  💡 大模型 GUI 智能体示例工程（基于 UFO 项目裁剪改造）
</h1>
<p align="center">
  <em>本仓库作为《大模型GUI智能体——人机交互新时代》书籍配套示例代码，演示如何构建可在 Windows 桌面上跨应用执行任务的多智能体体系。</em>
</p>

---

## 0. 📌 声明与来源

本示例工程在 MIT 许可下基于开源项目 UFO（原始仓库：https://github.com/microsoft/UFO/ ）进行删减与注释性改造，仅用于教学与阅读配套演示：

* 保留了与“GUI 智能体”核心概念相关的目录结构与关键代码骨架；
* 移除了与书籍主题不直接相关的宣传、媒体报道、路线规划等内容；
* 适度调整 README 叙述，使其聚焦 “大模型 + GUI 操作 + 跨应用工作流” 教学主线；
* 不保证与原始上游仓库保持完全同步；如需生产级、最新、完整功能，请访问原仓库。

> 版权与许可：原项目依旧保留其 MIT 协议；本仓库沿用该协议。请在引用或二次分发时同时注明来自 UFO 项目及本书示例工程。

---

## 1. 🎯 书籍定位与示例目标

围绕“人机交互新时代”这一主题，书中讨论了：

1. 大模型在 GUI 环境下的感知、决策与执行闭环；
2. 多 Agent（Host / App / 工具子代理）协作调度；
3. 混合控制（API 优先 + GUI 回退）策略与稳定性；
4. 检索增强（RAG）、经验记忆、演示学习等能力增益；
5. 任务轨迹、反馈、再规划与自我改进机制。

本仓库即作为一个“结构化参考骨架”，帮助读者将书中理论映射到真实代码组织形式。

---

## 2. 🧠 核心概念快速总览

| 组件 | 角色定位 | 关键要点 |
|------|----------|----------|
| HostAgent | 全局任务编排者 | 解析用户自然语言意图；拆解与分派子任务；维护全局状态机 |
| AppAgent | 针对特定应用的执行体 | 感知界面（UIAutomation/视觉截屏）；生成操作序列；执行与校验 |
| Knowledge / Memory | 知识与经验底座 | 文档、在线检索、用户示范、历史轨迹向量化检索 |
| Executor / Puppeteer | 执行器 | 按“优选原生 API，降级 GUI 操作”策略行动，兼顾效率与鲁棒性 |
| Speculative / Multi‑Action（可选） | 交互加速 | 对未来多步操作进行预测 + 逐步验证，减少 LLM 调用轮次 |

> 在本示例中，部分高级优化（如沙盒虚拟桌面、全量推测批执行等）可能被裁剪或仅保留接口占位，便于聚焦主干逻辑理解。

---

## 3. 📁 目录结构（节选与教学导向说明）

```
dataflow/            # 数据 / 控制流建模示例（任务到执行链路的编排框架）
learner/             # 经验/知识加工与索引（示例化）
record_processor/    # 任务执行记录、演示数据解析与摘要
ufo/                 # 核心多智能体框架（Host / App / LLM / 执行器等）
vectordb/            # 检索相关底座（示例/占位）
assets/              # 架构 / 流程图等可视化资源
documents/           # 文档（可扩展为本地知识子集）
```

阅读建议：先从 `ufo/` 下的 Host / App 相关代码入手，再回看 `dataflow/` 如何描述任务流，然后结合 `record_processor/` 了解轨迹与学习闭环。

---

## 4. 🚀 环境与快速运行

### 4.1 ⚙️ 环境要求
* 操作系统：Windows 10 或以上（需要桌面可视/可操作会话）
* Python：≥ 3.10
* 具备至少一个可调用的大模型（如 OpenAI / Azure OpenAI / 本地模型等）API Key

### 4.2 ⬇️ 安装步骤（示例）
```powershell
# （可选）创建隔离环境
# conda create -n gui_agent_book python=3.10
# conda activate gui_agent_book

# 克隆本书示例仓库（不是原始 UFO 仓库）
git clone <本仓库地址> gui-agent-book-sample
cd gui-agent-book-sample

# 安装依赖
pip install -r requirements.txt
```

### 4.3 🔧 配置模型参数
复制模板并填写：
```powershell
copy ufo\config\config.yaml.template ufo\config\config_dev.yaml
notepad ufo\config\config_dev.yaml
```
示例（片段）：
```yaml
HOST_AGENT:
  API_TYPE: aoai
  API_BASE: https://your-endpoint.openai.azure.com
  API_KEY:  YOUR_KEY
  API_MODEL: gpt-4o
APP_AGENT:
  API_TYPE: openai
  API_MODEL: gpt-4o
  VISUAL_MODE: true
```

### 4.4 ▶️ 运行最小示例
```powershell
python -m ufo --task simple_demo -r "在记事本中输入一句话并保存"
```
日志、截图、模型交互等输出可在：
```
ufo\logs\<task_name>\
```

> 提示：不同应用的自动化可需要额外权限（例如 UIAutomation 访问），首次运行请保证前台桌面与安全策略允许。

### 4.5 🧩 可选：启用外部知识 / 经验检索
在 `ufo/config/` 中可开启：
* 文档检索（离线帮助 / 本地说明）
* 在线搜索（需配置对应 API）
* 经验记忆（执行轨迹持久化与召回）
* 用户演示（录制 -> 解析 -> 索引）

---

## 5. 🧪 学习与实验路径建议

1. 跑通最小任务 —— 熟悉调用路径与日志；
2. 打开一个新应用（例如 计算器 / 记事本 / Office 组件）尝试自定义任务；
3. 引入演示数据（`record_processor/`）构建“示例驱动”提示增强；
4. 接入简单向量检索（`vectordb/`）并观察召回上下文对动作规划影响；
5. 对比“仅 GUI 操作”与“API + GUI 混合”执行的稳定性与速度差异；
6. 设计失败场景（控件定位变化、窗口遮挡、系统语言切换），评估健壮性策略。

---

## 6. 🔍 与原始 UFO 功能差异（摘要）

| 领域 | 原始 UFO | 本示例定位 |
|------|----------|------------|
| 功能完备度 | 包含最新特性（Speculative、多窗口沙盒等） | 以教学最小可理解路径为主，可能留空或简化 |
| 生态/更新 | 持续演进 | 不保证同步，必要时请回源仓库 |
| 适用场景 | 研究 + 工程试验 | 读者学习、课程/工作坊演示 |
| 代码裁剪 | 尽量保留真实结构 | 去除与书籍主线无关的外部宣传、媒体、路线图 |

---

## 7. ⚠️ 安全与伦理提示

* 自动化操作可能误触个人数据：请在隔离或测试账号环境中演示；
* 谨慎输入包含隐私 / 受限商业信息的指令；
* 任何用于生产或批量控制真实业务系统的扩展，需补充审计、权限、回滚、防滥用等机制；
* 大模型输出存在不确定性，务必加入“结果校验”环节（规则过滤 / 二次确认 / 仿真执行）。

---

## 8. 📚 引用与延伸阅读

### 8.1 🧭 GUI 智能体综述（强烈推荐）
LLM‑Brained GUI Agents: A Survey  
https://arxiv.org/abs/2411.18279  
GitHub: https://github.com/vyokky/LLM-Brained-GUI-Agents-Survey  
交互式汇总站点: https://vyokky.github.io/LLM-Brained-GUI-Agents-Survey/

> 该综述系统梳理了 GUI 智能体的感知、规划、执行、评测、对齐等核心研究脉络，可作为本书阅读的深化补充。

### 8.2 🏛️ 原始 UFO / AgentOS 论文（背景参考）
* UFO²: The Desktop AgentOS（2025）https://arxiv.org/abs/2504.14603
* UFO: A UI-Focused Agent for Windows OS Interaction（2024）https://arxiv.org/abs/2402.07939

若在学术论文或公开分享中引用本示例，请优先引用上述原始工作。

### 8.3 🗂️ BibTeX（可选）
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

## 9. 📄 许可（License）
本示例继续沿用原项目的 MIT 协议：详见 `LICENSE`。在再分发、修改或引用时请：
1. 标注源自微软开源项目 UFO；
2. 说明本仓库为书籍教学裁剪版，非官方发行版；
3. 不得造成对原项目来源、品牌或赞助关系的误导。

---

## 10. 🛡️ 限制与责任说明
本仓库代码仅用于学习与研究：
* 不提供任何生产级质量与 SLA 保证；
* 触及第三方软件自动化时请遵守其使用条款；
* 使用过程中产生的任何风险与后果由使用者自行承担。

---

<p align="center"><sub>本示例仅用于《大模型GUI智能体——人机交互新时代》教学演示；原始工程与最新能力请访问 Microsoft UFO 上游仓库。</sub></p>

