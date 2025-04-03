# Qiniu MCP Server

七牛云对象存储 Kodo 基于 S3 协议构建 Model Context Protocol (MCP) Server，支持用户在 AI 大模型客户端的上下文中通过该 MCP Server 来访问七牛云存储的空间和对象。

详细情况请参考 [基于 MCP 使用大模型访问对象存储 Kodo](https://developer.qiniu.com/kodo/12914/mcp-aimodel-kodo)。

## 功能特性
- 资源
  - 列举资源（受 AI 上下文限制，列举资源时默认只会列举 20 个资源，如果需要列举所有可使用工具分批次列举）
  - 读取资源
- 工具
  - 支持列举存储桶（Buckets）
  - 支持列举对象（Objects）
  - 支持读取对象内容

## 前置要求

- Python 3.12 或更高版本
- uv 包管理器

如果还没有安装 uv，可以使用以下命令安装：

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

## 安装

1. 克隆仓库：

```bash
# 克隆项目并进入目录
git clone git@github.com:qiniu/qiniu-mcp-server.git
cd qiniu-mcp-server
```

2. 创建并激活虚拟环境：

```bash
uv venv
source .venv/bin/activate  # Linux/macOS
# 或
.venv\Scripts\activate  # Windows
```

3. 安装依赖：

```bash
uv pip install -e .
```

## 配置

1. 复制环境变量模板：

```bash
cp .env.example .env
```

2. 编辑 `.env` 文件，配置以下参数：

```bash
# S3/Kodo 认证信息
QINIU_ACCESS_KEY=your_access_key
QINIU_SECRET_KEY=your_secret_key

# 区域信息
QINIU_REGION_NAME=your_region
QINIU_ENDPOINT_URL=endpoint_url # eg:https://s3.your_region.qiniucs.com

# 配置 bucket，多个 bucket 使用逗号隔开，建议最多配置 20 个 bucket
QINIU_BUCKETS=bucket1,bucket2,bucket3
```

## 使用方法

### 启动服务器

1. 使用标准输入输出（stdio）模式启动（默认）：

```bash
uv --directory . run qiniu-mcp-server
```

2. 使用 SSE 模式启动（用于 Web 应用）：

```bash
uv --directory . run qiniu-mcp-server --transport sse --port 8000
```

## 开发
扩展功能，首先在 core 目录下新增一个业务目录（eg: 存储 -> storage），在此业务目录下完成功能拓展。
在业务目录下新建一个 loader.py 文件，在此文件用于加载业务的工具或者资源，。

### 扩展工具


## 测试

### 使用 Model Control Protocol Inspector 测试

强烈推荐使用 [Model Control Protocol Inspector](https://github.com/modelcontextprotocol/inspector) 进行测试。

```shell
# node 版本为：v22.4.0
npx @modelcontextprotocol/inspector uv --directory . run qiniu-mcp-server
```

### 使用 cline 测试：

步骤：

1. 在 vscode 下载 cline 插件（下载后 cline 插件后在侧边栏会增加 cline 的图标）
2. 配置大模型
3. 配置 qiniu mcp
    1. 点击 cline 图标进入 cline 插件，选择 mcp server 模块
    2. 选择 installed，点击 Advanced MCP Settings 配置 mcp server，参考下面配置信息
   ```
   {
     "mcpServers": {
       "qiniu": {
         "command": "uv",
         "args": [
           "--directory",
           "/Users/yangsen/Workspace/App/qiniu-mcp", # 此处选择项目存储的绝对路径
           "run",
           "qiniu-mcp-server"
         ],
         "disabled": false
       }
     }
   }
   ```
    3. 点击 qiniu mcp server 的链接开关进行连接
4. 在 cline 中创建一个 chat 窗口，此时我们可以和 AI 进行交互来使用 qiniu-mcp-server ，下面给出几个示例：
      - 列举 qiniu 的资源信息 
      - 列举 qiniu 中所有的 bucket 
      - 列举 qiniu 中 xxx bucket 的文件 
      - 读取 qiniu xxx bucket 中 yyy 的文件内容



