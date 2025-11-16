# 如何为jouleverse搭建blockscout浏览器

Author | 得葱
-|-
Reviewer | ~

## 前言

BlockScout是一个开源的以太坊和相关链的区块浏览器，用于查询交易、账户、智能合约等信息。本教程将指导你如何通过连接Jouleverse的节点RPC来配置和搭建BlockScout浏览器。

### 准备工作

1. **安装并运行一个Jouleverse节点**：
   - 首先确保你已成功部署了一个支持JSON-RPC API的Jouleverse节点并监听了特定端口（默认为`8501`）。<br>你可以参考以下文档了解如何搭建Jouleverse节点：
     - [Jouleverse主网节点搭建指南](/#!/docs/network/how-to-setup-jouleverse-node.md) 

2. **必备工具**：
   - Docker v20.10+
   - Docker-compose 2.x.x+
   - 建议硬件配置在4核8G以上。经测试可用的在线开发环境有：Github Codespaces，Gitpod（请自行了解）
   
   以下采用Docker-compose的方式来进行构建部署，需提前安装好docker和docker-compose

3. **获取BlockScout源码**：
   - 从GitHub上[Fork项目](https://github.com/blockscout/blockscout)并使用在线开发环境中打开（例如 Github Codespaces），或者本地克隆下载BlockScout项目的最新版本：
     ```bash
     git clone https://github.com/blockscout/blockscout.git
     ```

### 配置BlockScout

1. **编辑配置文件**：
   - 打开`blockscout/docker-compose/docker-compose.yml`
   - 修改以下RPC连接设置：

   ```yaml
     ETHEREUM_JSONRPC_HTTP_URL: http://<your-ip-address>:8501/
     ETHEREUM_JSONRPC_TRACE_URL: http://<your-ip-address>:8501/
     # ETHEREUM_JSONRPC_WS_URL: ws://host.docker.internal:8545/ #如果没有使用，请注释掉这行
     CHAIN_ID: '3666'
   ```
   
   - 打开`blockscout/docker-compose/envs/common-blockscout.env`
   - 修改以下设置：

   ```yaml
     ETHEREUM_JSONRPC_HTTP_URL=http://<your-ip-address>:8501/
     ETHEREUM_JSONRPC_TRACE_URL=http://<your-ip-address>:8501/
     # ETHEREUM_JSONRPC_WS_URL= #如果没有使用，请注释掉这行
     CHAIN_ID=3666
   ```
   
   - 打开`blockscout/docker-compose/envs/common-frontend.env`
   - 修改以下设置：

   ```yaml
     1. 把localhost替换成你的节点IP或者域名 (本地测试可以不修改)
     2. 按实际使用的情况修改http为https，ws为wss
     3. 修改其余信息
     NEXT_PUBLIC_NETWORK_NAME=Jouleverse chain
     NEXT_PUBLIC_NETWORK_SHORT_NAME=Jouleverse chain
     NEXT_PUBLIC_NETWORK_ID=3666
     NEXT_PUBLIC_NETWORK_CURRENCY_NAME=Joule
     NEXT_PUBLIC_NETWORK_CURRENCY_SYMBOL=J
     NEXT_PUBLIC_NETWORK_CURRENCY_DECIMALS=18
   
   ```

2. **（可选）配置其他选项**：
   - 如果需要，配置数据库、日志记录和其他应用相关的设置，更多详细配置请参考[Blockscout官方文档](https://docs.blockscout.com/for-developers/deployment/docker-compose-deployment) 

### 安装与启动BlockScout

1. **构建和启动**：
   - 运行命令进行构建并运行程序

   ```bash
   cd blockscout/docker-compose
   docker-compose up --build
   ```

2. **查看BlockScout启动情况**：
   - 打开另一个终端，用docker命令查看BlockScout的服务运行是否正常

   ```bash
   docker ps -a
   ```

### 测试与验证

**访问BlockScout**：
   - 在浏览器中访问BlockScout的部署地址，确认它是否能够正确连接到你的节点，并显示区块和交易数据。（通常前台会有提示数据索引的进度）
   ```bash
   http://localhost
   http://<your-ip-address>
   https://<your-domain>
   ```
至此，我们已经成功为Jouleverse搭建了Blockscout浏览器。

### 注意事项

- 确保你的RPC节点允许来自BlockScout服务器的连接请求。
- 考虑安全性和性能优化，比如限制RPC接口的访问权限和速率控制。

以上步骤仅为简化的示例，具体操作请参照BlockScout官方文档和最新的代码仓库说明。
