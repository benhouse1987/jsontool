# JSON 对比结果解析工具

## 项目简介
本项目提供一个开箱即用的单页 Web 工具，只需在浏览器中打开 `json-comparator.html`，即可把“实际响应 / 预期响应 / 原始录制响应”三段文本解析成格式化的 JSON，并在三列视图中并排展示，支持差异高亮、统计面板与差异报告，方便人工校对。【F:json-comparator.html†L1-L497】

## 核心功能
- **文本提取与转义修复**：`parseComparisonResult` 会从原始对比文本中提取三段响应内容，并结合 `findJsonEnd` 处理被包裹或截断的 JSON；`unescapeJSON`/`unescapeJSONOptimized` 进一步清理多层转义和常见格式问题，确保能够顺利解析为对象。【F:json-comparator.html†L558-L829】【F:json-comparator.html†L2370-L2410】
- **递归排序与差异比对**：`sortObjectKeys`/`sortObjectKeysOptimized` 递归整理键顺序；`compareObjects` 能自动识别数组主键，对比缺失、数值、类型等差异；`filterIgnoredDifferences` 与 `syntaxHighlight` 则负责过滤带 `[IGNORED]` 标记或特定路径的差异，并在界面中以颜色块标出不同类型的变更。【F:json-comparator.html†L831-L1084】【F:json-comparator.html†L1371-L1479】
- **统计与说明面板**：`generateStats` 会统计两份 JSON 的字段数量、差异数与匹配度，并生成图表式卡片；差异报告区同时给出简洁的对比结论说明，辅以颜色图例帮助理解。【F:json-comparator.html†L1533-L2029】
- **大数据性能优化**：`parseAndCompare` 会根据输入大小自动切换普通模式或大数据模式；在大数据模式下使用分步进度条、惰性调度和虚拟滚动来保证体验，同时复用优化版的转义、排序与比对函数。【F:json-comparator.html†L1638-L1894】【F:json-comparator.html†L2241-L2440】
- **交互增强**：页面内置三套示例数据按钮、Toast 提示、自动解析（停止输入 1.5 秒后触发）、快捷键（Ctrl+Enter 对比、Ctrl+Delete 清空）以及三列同步滚动，帮助快速定位差异。【F:json-comparator.html†L445-L2136】

## 使用指南
1. **打开页面**：直接在浏览器中双击或通过本地服务器访问 `json-comparator.html`。
2. **准备数据**：将包含“实际响应: … / 预期响应: … / 原始录制响应: …”的文本粘贴到输入框，或点击顶部的示例按钮加载预设案例，界面会自动尝试解析。【F:json-comparator.html†L451-L498】【F:json-comparator.html†L2241-L2367】
3. **开始对比**：点击“开始对比”按钮，或使用 Ctrl+Enter 快捷键；工具会在解析成功后展示三列格式化结果、统计面板及差异报告，并通过颜色说明差异类型。【F:json-comparator.html†L470-L2036】
4. **辅助操作**：需要重置时点击“清空”或使用 Ctrl+Delete；输入框停止编辑 1.5 秒会自动重新解析。差异面板支持同步滚动、Toast 提示则反馈每个关键步骤的状态。【F:json-comparator.html†L470-L2136】

## 定制与扩展
- **忽略特定差异**：在 `filterIgnoredDifferences` 中可以调整默认忽略的路径列表，或在数据中加入 `[IGNORED]` 标记控制单项差异是否计入结果。【F:json-comparator.html†L945-L973】
- **调整大数据阈值**：`parseAndCompare` 里通过 `1MB` 阈值判断是否进入大数据模式，可根据实际情况修改；`parseAndCompareLarge` 的步骤数组同样容易扩展新的处理环节。【F:json-comparator.html†L1638-L1767】
- **扩展示例数据**：`loadTestCase`、`loadBlueHighlightTestCase`、`loadIgnoredTestCase` 展示了三类典型场景，可按相同格式添加更多按钮，帮助团队统一核对流程。【F:json-comparator.html†L451-L2367】

欢迎根据团队校对流程进一步封装成内部工具，或将核心逻辑抽取到独立脚本以集成到其他差异分析系统。
