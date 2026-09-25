# OfferPilot — 开发过程记录

更新：2026-09-25（悉尼时间）。只把核对过的操作记为完成；每条 slice 记录结果、证据与下一步。

| 步骤 | 状态与证据 |
| --- | --- |
| 产品目标 | Sam 已明确：诊断既有简历与目标 JD，让雇主看见相关技术证据；不生成新简历。详见 `docs/spec.md`。 |
| GitHub 仓库 | Sam 已将首个文档提交 `13c8346` 推送到公开仓库 `https://github.com/XiangXuX/OfferPilot`，本地 `main` 当时与 `origin/main` 同步。 |
| 本地克隆 | Sam 的终端已核对实际路径为 `C:\Users\xuxia\OneDrive\Desktop\working\新建文件夹\OfferPilot`；在该目录完成 Git 初始提交。 |
| 范围与流程 | 原 14 天逐日排期被 Sam 指出过散、范围不清；已改成待审阅的 P0 边界、优先 backlog、逐条验收与 CI/CD 规则，见 `docs/roadmap.md`。 |
| 范围审阅：分析单位 | Sam 已确认一份简历可分析多个 JD；每条分析的结果和审核选择独立。已写入 `docs/spec.md` 的规则和验收例子。其余范围继续逐项审阅。 |
| JD 长文本处理 | Sam 提问是否需要 RAG；AI 提出先完整提取、必要时按章节分段并核对原文的方案。尚待 Sam 确认，不是最终架构决定。 |
| JD 要求层级 | Sam 提供 Core Flow AI 示例：资格和经验要求是先看的条件，主要职责用于判断经历是否相似。已据此加入 `docs/spec.md` 的结构验收样例；具体自动抽取实现仍待后续 slice。 |
| 职责中的技术门槛 | Sam 纠正：该 JD 中 TypeScript 虽写在 Key responsibilities，仍是资格门槛，须单独靠前列出。已修正规格与诊断验收；简历无证据仍不等于用户不会。 |
| 技能证据规则 | Sam 确认：Skills 栏仅列关键词不算充分证据；需在相关经历中看到具体使用方式。已加入 `docs/spec.md` 的 TypeScript 验收例子。 |
| 无关内容的建议 | Sam 澄清：建议可为剔除，或在有真实相关经历时改写成与 JD 有关的重点；不能编造关联。已写入规则与验收例子。 |
| 剩余范围审阅方式 | Sam 要求停止逐个提问；已将剩余八项产品选择及建议默认值集中列在 `docs/spec.md` 的“开放决定”，等待一次讨论。技术实现细节在对应 slice 前决定。 |
| 阅读视角 | Sam 要求以 HR 初筛视角看问题，并指出初筛与真正用人经理的关注点可能相距很远。规格已改成首屏展示关键门槛与可见证据，技术细节可展开；不声称替雇主做录用判断。 |
| 集中范围确认 | Sam 接受八项中的 1–4、6–8，并纠正第 5 项：JD 输入是粘贴文本，不是 PDF。简历仍为 PDF；其提取错误处理留到上传 slice 前决定。`docs/spec.md` 已区分确认与待定。 |
| 文档进入仓库／首次 commit | Sam 提交并推送 `13c8346`，包含 `AI_USAGE.md`、`docs/spec.md`、`docs/roadmap.md`、`docs/progress.md`。后续 AI 按 Sam 的持续记录要求更新 GitHub 文档，`docs/learning-notes.md` 已加入。 |

## Issue #1：前端与 API 基线（2026-09-24—25）

| 项目 | 已观察的结果 |
| --- | --- |
| 环境 | Sam 的终端显示 Node v24.19.0、npm 11.17.0、.NET SDK 10.0.401。 |
| 后端 | Sam 在本地创建 `server/OfferPilot.Api`，运行 `dotnet run --project server/OfferPilot.Api --urls http://localhost:5080`；健康接口 `GET /api/health` 返回 `{"status":"ok"}`。 |
| 前端 | Sam 在 `client/` 创建 Vite React + TypeScript 项目，浏览器成功显示模板页；配置开发代理后 `http://localhost:5173/api/health` 可收到健康状态。 |
| 真实连接状态 | Sam 报告页面显示 `API: connected`；停止后端并刷新后，确认故障状态正常显示。此项由 Sam 本机手动验证，代码尚未提交到 GitHub，未由 CI 验证。 |
| 下一步 | Sam 在仓库根目录自己创建 `.gitignore`，忽略生成的 `bin/`、`obj/`、`node_modules/`、`dist/`；再做本地构建、README、CI、审查与提交。 |

以后每条记录：日期｜Issue／目标｜Sam 实际操作｜测试或演示证据｜问题与处理｜下一条优先级。
