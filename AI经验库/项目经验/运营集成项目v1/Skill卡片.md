# 运营集成项目v1 - Skill 卡片

更新时间：2026-06-16

## Skill 1：运营中控项目交接检查

### 适用场景

接手或继续开发“运营集成项目v1”时，用于快速确认项目是否可运行、AI 链路是否正常、前端主路径是否没有回退。

### 输入

- 本地项目路径：`C:\Users\Administrator\Documents\品牌集成产品开发\omiflow-current`
- 目标端口：`3001`
- 不读取、不输出任何真实密钥。

### 步骤

1. 进入源码根目录。
2. 查看 `git remote -v`，确认源码仓库为 `https://github.com/wang-zefeng/omni-flow.git`。
3. 运行 `npm run lint`。
4. 运行 `npm run build`。
5. 设置 `$env:PORT="3001"`。
6. 运行 `npm run start`。
7. 打开 `http://127.0.0.1:3001/`。
8. 检查首页资源路径是 `/assets/index-*.js`，不是 `/@vite/client`。
9. 进入“数字化 Agent 工作室”执行一次 Agent。
10. 确认页面停留在工作室、右侧结果可见、日志流出现完成状态。

### 通过标准

- lint 通过。
- build 通过。
- `/api/health` 正常。
- Agent 接口返回 200。
- 执行结果在工作室内可见。
- 日志刷新不切回 dashboard。

## Skill 2：Agent 工作室状态问题排查

### 适用场景

请求成功但用户看不到 Agent 输出，或页面执行后跳回默认看板。

### 重点文件

- `src\App.tsx`
- `src\services\ai-service.ts`
- `server.ts`

### 排查顺序

1. 先确认 Network 里的 `/api/gemini/run-workflow` 是否返回 200。
2. 再看服务端日志是否出现 `[AI Service] Provider`。
3. 如果后端成功，优先排查前端状态。
4. 搜索 `setActiveTab("dashboard")` 的调用来源。
5. 检查 `triggerWorkflowExecution()` 是否在成功或 finally 中改了主视图。
6. 检查 `fetchWorkflowLogs()` 是否有副作用改变 `activeTab`。
7. 检查 `aiOutputResult` / `hasNewOutput` 是否真的渲染在 `activeTab === "workflows"` 的区域内。
8. 处理 200 但 `result` 为空的提示。
9. 处理非 200 或 fetch 异常时的当前面板错误展示。

### 禁止事项

- 不要重新做 AI 接入。
- 不要改 DeepSeek key。
- 不要读取或输出 `.env.local` 中的密钥。
- 不要为了一个状态 bug 重构整个前端。

## Skill 3：表格画像性能修复检查

### 适用场景

Excel / CSV 上传后卡死、栈溢出、宽表日序列分析变慢。

### 排查顺序

1. 定位 `profileTable()`。
2. 检查是否存在大数组展开、深递归、全量复制、无上限排序。
3. 给行数、列数、样本数和唯一值统计设置上限。
4. 对宽表日期列候选做上限和早停。
5. 保留“抽样分析”的用户提示。
6. 若仍需要处理更大文件，再引入 Web Worker 或后端异步任务。

### 通过标准

- 大文件不会触发 `RangeError: Maximum call stack size exceeded`。
- 页面仍可交互。
- 分析结果明确说明采样或上限。
- 小文件结果不回退。
