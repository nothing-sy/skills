---
name: jenkins
description: 触发 Jenkins 远程构建并查询构建状态，支持自动匹配项目 Job、分支选择和状态监控
user-invocable: true
---

# Jenkins 构建管理

## 配置信息

- **服务器地址**: `http://IP:8080`
- **用户名**: `admin`
- **API Token**: `token`

## 操作权限规则

| 操作类型 | 是否需要确认 | 说明 |
|----------|-------------|------|
| 查询 Job 列表 | ❌ 自动执行 | 只读操作 |
| 查询构建状态 | ❌ 自动执行 | 只读操作 |
| 查询构建日志 | ❌ 自动执行 | 只读操作 |
| 查询构建队列 | ❌ 自动执行 | 只读操作 |
| 触发构建 | ✅ 需用户确认 | 需确认 Job 和分支 |
| 取消构建 | ✅ 需用户确认 | 变更操作 |
| 删除构建 | ✅ 需用户确认 | 破坏性操作 |

## API 端点

### 查询操作（自动执行）

```bash
# 获取所有 Job 列表（递归获取文件夹内的 Job）
curl -s "http://IP:8080/api/json?tree=jobs[name,url,color,jobs[name,url,color]]" \
  -u "admin:token"

# 查询最后一次构建状态
curl -s "http://IP:8080/job/{folder}/job/{jobName}/lastBuild/api/json" \
  -u "admin:token"

# 查询指定构建状态
curl -s "http://IP:8080/job/{folder}/job/{jobName}/{buildNumber}/api/json" \
  -u "admin:token"

# 查询构建日志
curl -s "http://IP:8080/job/{folder}/job/{jobName}/{buildNumber}/logText/progressiveText" \
  -u "admin:token"

# 查询构建队列
curl -s "http://IP:8080/queue/api/json" \
  -u "admin:token"
```

### 构建操作（需用户确认）

```bash
# 触发带参数构建
curl -X POST "http://IP:8080/job/{folder}/job/{jobName}/buildWithParameters" \
  -u "admin:token" \
  --data-urlencode "deployBranch={branch}" \
  -w "\nHTTP Status: %{http_code}\n" \
  -s -D - -o /dev/null

# 取消构建
curl -X POST "http://IP:8080/job/{folder}/job/{jobName}/{buildNumber}/stop" \
  -u "admin:token"
```

## 执行流程

### 1. Job 自动发现

当用户请求构建时，自动执行以下步骤：

**Step 1**: 获取当前项目名称（从工作目录名推断）

**Step 2**: 查询 Jenkins 所有 Job：

```bash
curl -s "http://IP:8080/api/json?tree=jobs[name,url,color,jobs[name,url,color]]" \
  -u "admin:token"
```

**Step 3**: 匹配逻辑

- 在 Job 名称中搜索包含项目名称的 Job（不区分大小写）
- 记录完整的 Job 路径（包括文件夹前缀，如 `储能/vpp-microgrid`）

**Step 4**: 根据匹配结果处理

| 匹配数量 | 处理方式 |
|----------|----------|
| 0 个 | 询问用户：请提供正确的 Job 名称或从列表中选择 |
| 1 个 | 自动选中，告知用户 |
| 多个 | 列出所有匹配项，让用户选择（显示序号列表） |

### 2. 分支选择

确定 Job 后，**必须询问用户**要部署的分支：

```
请指定要部署的分支（默认: release）：
1. release
2. develop
3. main
4. 手动输入其他分支
```

可先查询 Git 远程分支列表供参考：

```bash
git branch -r 2>/dev/null | head -20
```

### 3. 构建确认与执行

确认信息格式：

```
即将执行构建：
- Job: {文件夹}/{Job名称}
- 分支: {branch}
- 操作: 触发构建

确认执行？(y/n)
```

用户确认后执行构建，然后自动监控构建状态直到完成。

### 4. 构建状态监控

构建触发后，自动执行以下监控流程（无需用户确认）：

1. 从响应头 `Location` 提取队列 ID
2. 轮询队列 API 等待构建开始
3. 获取构建号后，轮询构建状态
4. 构建完成后报告结果

## 构建状态判断

从 API 响应中判断状态：

| 字段值 | 状态 |
|--------|------|
| `building: true` | 构建进行中 |
| `result: null` | 构建中或队列中 |
| `result: "SUCCESS"` | ✅ 构建成功 |
| `result: "FAILURE"` | ❌ 构建失败 |
| `result: "ABORTED"` | ⚠️ 构建被取消 |
| `result: "UNSTABLE"` | ⚠️ 构建不稳定（测试失败等） |

## URL 编码规则

Jenkins API 中的路径需要进行 URL 编码：

| 字符 | 编码 |
|------|------|
| 中文 | UTF-8 编码（`储能` → `%E5%82%A8%E8%83%BD`） |
| 空格 | `%20` |
| `/` | 保持不变（路径分隔符） |

完整 URL 格式：
```
http://IP:8080/job/{folder}/job/{jobName}/...
```

## 交互示例

### 示例 1: 自动匹配成功

```
用户: 触发构建

助手: [自动查询 Job 列表]
找到匹配的 Job: 储能/vpp-microgrid

请选择要部署的分支：
1. release (默认)
2. develop
3. main

用户: 1

助手: 即将执行构建：
- Job: 储能/vpp-microgrid
- 分支: release

确认执行？(y/n)

用户: y

助手: [执行构建并监控状态]
✅ 构建 #72 完成，状态: SUCCESS
```

### 示例 2: 多个匹配需选择

```
用户: 触发构建

助手: [自动查询 Job 列表]
找到多个匹配的 Job，请选择：
1. 储能/vpp-microgrid
2. 储能/vpp-microgrid-admin
3. 测试/vpp-microgrid-test

请输入序号选择：

用户: 1

助手: [继续分支选择流程...]
```

### 示例 3: 无匹配需用户提供

```
用户: 触发构建

助手: [自动查询 Job 列表]
未找到与 "my-project" 匹配的 Job。

可用的 Job 列表：
1. 储能/vpp-microgrid
2. 储能/vpp-microgrid-admin
3. 其他/another-project

请选择序号或输入 Job 名称：

用户: 1

助手: [继续分支选择流程...]
```

## 注意事项

1. **中文编码**: URL 中的中文必须进行 UTF-8 URL 编码
2. **CSRF 保护**: 如 Jenkins 启用了 CSRF，需先获取 CRUMB token
3. **敏感信息**: Token 不要泄露到公开仓库
4. **构建参数**: 不同 Job 可能有不同的构建参数，触发前可先查询 Job 配置
