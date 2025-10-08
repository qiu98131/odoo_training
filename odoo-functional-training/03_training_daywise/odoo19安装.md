# 安装 Odoo 19 于 Ubuntu 24.04，并配置 VSCode 开发环境

以下是详细的安装步骤，基于 Odoo 19 的官方源代码安装指南（适用于开发环境），并适应使用您登录的系统用户名 "bincoo" 进行配置。
这意味着我们不会创建新的系统用户，而是直接使用 "bincoo" 用户来运行 Odoo、PostgreSQL 和相关进程。
整个过程假设您已以 "bincoo" 用户登录终端，并具有 sudo 权限。如果不是，请先切换到 "bincoo" 用户（使用 su - bincoo）。

注意事项：
- 这是一个源代码安装，适合开发环境（非生产）。生产环境建议使用 Docker 或包安装。
- Odoo 19 需要 Python 3.10+，PostgreSQL 12+，和 wkhtmltopdf 0.12.6。
- 路径假设在 /home/bincoo/odoo-dev 下进行安装，您可以调整。
- 如果遇到权限问题，确保文件所有权为 "bincoo"（使用 chown -R bincoo:bincoo /path/to/dir）。
- 安装后，您可以使用 VSCode 进行调试和模块开发。
- 整个过程可能需要 30-60 分钟，取决于网络和系统。

### 步骤 1: 更新系统并安装基本依赖

更新 Ubuntu 包列表并升级现有包，以确保系统是最新的。

```bash
sudo apt update
sudo apt upgrade -y
```
