# 仓库账本（GitHub Ledger）

一个单文件、纯静态的中文记账工具。网页由 GitHub Pages 托管，账单通过 GitHub Contents API 保存到你指定的仓库 JSON 文件中。

## 功能

- 记录收入和支出：金额、日期、分类、账户、备注
- 本月收入、支出和结余统计
- 按月份筛选、按分类/备注/账户搜索
- 编辑和删除账单
- 每次保存都会产生 GitHub commit
- 无需自建后端，手机和电脑浏览器均可使用

## 部署网页

1. 将本仓库的 `index.html` 放在仓库根目录。
2. 打开仓库的 **Settings → Pages**。
3. 在 **Build and deployment** 中选择 **Deploy from a branch**。
4. 选择 `main` 分支和 `/(root)` 目录并保存。
5. 等待 GitHub Pages 发布，然后打开页面链接。

## 配置账单存储

建议网页仓库与账单数据仓库分开。网页可以公开托管，账单数据仓库建议设为私有。

1. 在 GitHub 创建一个专门存放账单数据的仓库。
2. 在 GitHub 的 **Settings → Developer settings → Personal access tokens → Fine-grained tokens** 创建 Token。
3. Token 的 Repository access 只选择账单数据仓库，Repository permissions 只授予 **Contents: Read and write**。
4. 打开已部署的网页，在「GitHub 仓库设置」中填写用户名/组织、数据仓库名、分支（通常为 `main`）、数据文件路径（默认 `data/transactions.json`）和 Token。
5. 点击「连接并读取」。首次使用时，如果数据文件还不存在，应用会以空账本开始；首次保存时创建该文件。

## 安全说明

- **不要把 Token 写进源码、提交到 Git 仓库或分享给他人。**
- 应用将 Token 保存在当前浏览器标签页的 `sessionStorage`；关闭标签页后通常会清除。网页运行期间，页面脚本仍可以访问 Token，因此只在你信任的网页地址使用。
- Token 只授予专用数据仓库的 Contents 读写权限，不要使用拥有所有仓库权限的宽权限 Token。
- 账单数据仓库设为私有；注意 Git 提交历史可能保留已删除账单的旧版本。
- 多设备同时编辑可能造成数据覆盖。编辑前先同步，避免同时在多个设备写入。
- 这是轻量个人工具，不是银行级账务系统。请定期备份账单数据。

## 数据格式

账单默认保存为 `data/transactions.json`，内容为 JSON 数组，每条记录包含 `id`、`type`、`amount`、`date`、`category`、`account`、`note`、`createdAt` 和 `updatedAt`。
