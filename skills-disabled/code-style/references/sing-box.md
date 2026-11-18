# sing-box 项目约定

以下来自这次项目重构指令，作为上下文保留。它们不等于所有项目都应删除版本号、权限管理或恢复机制；在 sing-box 中按用户已经确定的方向实施。

- 状态目录类型叫 `StateDir`，原 journal 模型叫 `State`。
- 状态只保留一个 JSON 文件；删除重复的 `save_settings` / `settings` 层，只保留 JSON 操作。配置源文件与状态文件不是一回事。
- 按 fresh install 实现。不要增加读取旧状态、迁移、删除旧 JSON 等兼容流程。
- enroll 直接执行目标状态转换，按请求覆盖状态；不加“旧身份属于不同 origin”拒绝分支。
- enrollment code 是安装器提供的必填参数；删除交互输入、终端探测和可选 code 路径。
- 删除状态目录的 protected Windows DACL 与 Linux 权限管理；删除产品措辞里的 `private`。这不影响 Rust 私有可见性、private-item 文档和对应 lint。
- 删除额外的文件类型、强制非 symlink、任意长度要求。
- 删除 native validation、connectivity probe、recover、rollback、reject 和 local fallback 机制；不保留这些机制的包装层。
- 删除 revision 版本号以及前后端 core version 字段和逻辑。
- 配置采用原始 base、多个命名源文件与单独生成的 managed 部分组合；不要求已有 proxy outbound slot，不重写原始配置来替换 slot。
- 日志在后端形成可展示的模型并处理颜色控制码；前端不 strip、不再推导一套状态或错误语义。
- 删除手动 diagnostics 入口及其整套实现。将来的 relay 自动观察能力另按明确需求实现。
- 安装命令简单、远程获取脚本、可以重放；脚本错误必须在项目里修复，不能只手动修好一次现场。
- 不添加用户没要求的授权和验证步骤。安装后的 relay 按既定身份直接工作。
- 通知使用 WebSocket 长连接，删除轮询；避免闲置状态持续消耗 Worker 和 D1 请求。
- provider 等可创建对象必须有完整的修改、重命名、删除和关联资源释放路径。
- 测试在本地优先执行并复用缓存；Actions 负责已授权的发布，避免每次小改动重复下载和完整多平台测试。

这些要求解释了规范的来源，不要求把历史故障说明或这份清单复制到项目文档。
