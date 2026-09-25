# OfferPilot 学习笔记

更新：2026-09-25（悉尼时间）

这份笔记记录 Sam 在亲自编写 OfferPilot 时提出的问题。每次只记清楚一个概念、它在项目里的位置，以及已经实际验证的结果。新增问题继续按日期追加；没有验证的功能不写成已完成。

## 同步与异步：先理解这四句话

1. **同步执行**：当前代码按顺序往下走；某一步没有完成，后面的这部分代码暂时不能继续。例如 `const x = 2; const y = x + 1`，先得到 `x`，再计算 `y`。很耗时的同步计算会占住浏览器主线程。
2. **异步操作**：发起可能需要时间的工作（例如 `fetch` 网络请求），先得到一个表示将来结果的 `Promise`；浏览器可以继续处理页面和用户操作。异步不等于所有代码都在另一个线程并行运行。
3. **`async`**：把函数声明为异步函数；它总是返回 `Promise`，函数内部可以使用 `await`。
4. **`await`**：等待一个 `Promise` 的结果，暂停的是**当前异步函数后续依赖结果的代码**，不是整个浏览器。结果回来后从这里继续；失败会抛出错误，可由 `try/catch` 处理。

OfferPilot 的顺序：React 先显示 `checking` → `fetch('/api/health')` 开始网络请求 → 页面仍可显示和响应 → `await` 收到响应 → 检查 HTTP 状态和 JSON 的 `status` → `setConnection(...)` 更新页面。`response.ok` 表示 HTTP 成功；还要检查 JSON 是否为预期的 `status: "ok"`。

```ts
async function checkHealth() {
  const response = await fetch('/api/health')
  const body = await response.json()
  console.log(body.status)
}
```

这里两次 `await` 分别等 HTTP 响应和解析响应正文；看起来顺序书写，但等待网络时不会把整个页面卡住。

## 已问过的问题

### Git 与项目起步（2026-09-24）

- **仓库、clone、commit、push 分别是什么？** GitHub 仓库保存共享历史；clone 得到本地副本；保存文件只改工作区；`git add` 选入暂存区；`git commit` 生成本地历史节点；`git push` 把本地提交送到 GitHub。`git status` 用来核对目前在哪一层。
- **为什么 VS Code 文件名有 `U`？** `U` 是 untracked，表示文件尚未被 Git 跟踪。颜色由 VS Code 主题和 Git 状态决定，不代表文件是否已上传。
- **`fetch` / `push` 图标说明什么？** 本地仓库配置了远程地址，可检查和同步；文件仍须先 commit，再 push。首个文档提交已由 Sam 推送，Issue #1 已创建。

### 后端与前端基线（2026-09-24）

- **.NET、C#、ASP.NET Core 是什么？** C# 是语言，.NET 10 是构建和运行平台，ASP.NET Core 是编写 Web API 的框架。`dotnet new web` 生成了 `server/OfferPilot.Api` 骨架；`Program.cs` 注册路由；`.csproj` 写项目配置。
- **`MapGet('/api/health', ...)` 做了什么？** 在后端注册 GET 路由；浏览器请求时返回 JSON `{"status":"ok"}`。直接访问后端端口 5080 已验证 API 可响应。
- **`dotnet run` 为什么一直占着终端？** Web 服务器启动后持续监听请求；`Now listening` 是运行成功。`Ctrl+C` 才停止。
- **React、Vite、TypeScript 各做什么？** React 构建页面组件，TypeScript帮助检查代码的类型，Vite提供本地开发服务器和构建工具。模板页面在 5173 已打开。模板示例图片、计数器和链接会从 `App.tsx` 移除。
- **为什么 `npm run dev` 在仓库根目录报 `ENOENT`？** `package.json` 在 `client/`；先 `cd client` 再运行。命令通常要在所属项目目录执行。
- **`vite.config.ts` 的 proxy 做什么？** 开发时把前端地址 `/api/...` 转发到本机后端 5080，路径保持一致。访问 `http://localhost:5173/api/health` 已得到健康状态。配置文件只能有一个 `export default`。
- **为什么用 `useEffect`？** 组件显示后自动向外部 API 发请求，把结果存入 React state；空依赖数组 `[]` 对应挂载时的检查。开发模式的 Strict Mode 可能额外执行一次设置/清理；`AbortController` 在清理时取消过期请求。
- **`async function checkHealth` 为什么写在 Effect 里面？** `checkHealth` 可以用 `await` 等网络；Effect 的回调仍返回清理函数 `() => controller.abort()`。`void checkHealth()` 表示启动它且不在 Effect 回调里等待，内部 `try/catch` 处理错误。

## 目前验证到哪里

- 已验证：GitHub 文档首个提交、Issue #1、.NET 10 SDK、后端健康接口、Vite 模板页、Vite 到后端的 `/api/health` 代理。
- 待验证：替换 `client/src/App.tsx` 后页面显示 `connected`；停止后端并刷新后显示 `unavailable`；随后再做构建、CI、README 和提交。

## 后续如何续记

每遇到一个问题，追加：**日期｜Sam 的原问题｜一句话答案｜OfferPilot 中的位置｜实际验证结果／待验证**。若理解改变，直接更正原条并说明原因。这里记录学习过程；产品需求和验收仍以 `docs/spec.md`、`docs/roadmap.md` 和 GitHub Issue 为准。

## 参考

- [MDN：异步 JavaScript](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Async_JS/Introducing)
- [MDN：Promise 与 async/await](https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Async_JS/Promises)
- [React：useEffect](https://react.dev/reference/react/useEffect)
- [Vite：开发服务器 proxy](https://vite.dev/config/server-options#server-proxy)
