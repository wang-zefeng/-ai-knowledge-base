# 批量主图套版工作台 - Skill 卡片

## 可复用 Skill

### 归档或接手电商主图自动套版项目

适用场景：需要恢复、评估或继续开发「电商主图自动套版 / 批量主图套版工作台」。

执行步骤：
1. 先读 `项目状态.md`，确认源码仓库、运行方式和当前风险。
2. 打开源码仓库 <https://github.com/wang-zefeng/batch-main-image> 或本地工作区 `C:\Users\Administrator\Documents\设计自动化`。
3. 读 `README.md`，确认 MVP 目标是「活动线框 + 产品资料 + 机制数据 -> 主图 PNG」。
4. 读 `data/product-lineframe-template.json` 和 `docs/product-wireframe-template.md`，先理解字段和槽位，再改 UI。
5. 读 `src/app.js` 的关键路径：`parseCsv`、`briefFromCsvRow`、`runPreflight`、`render`、`exportPng`、`exportBatchPng`。
6. 本地验证时直接浏览器打开 `index.html`，不需要安装依赖或构建。
7. 做功能时保持小闭环：输入 CSV / 图片素材 -> 映射为 `brief` -> 预检 -> Canvas 预览 -> PNG 输出。

项目边界：
- 不优先做用户系统、支付系统、复杂权限、自动爬图或复杂 PSD 还原。
- 不让 AI 直接生成最终主图；AI 只生成结构化卖点、机制文案或字段建议。
- 不在经验库上传完整源码、大素材、导出视频/图片、日志、`.env`、`.git`、`node_modules`。

建议验证清单：
- `index.html` 能打开。
- CSV 示例能应用。
- 产品图文件名能匹配本地素材。
- 预检能识别缺字段、缺图片、错误模板 ID。
- 单张 PNG 导出可用。
- 批量 PNG 导出路径可用。
