# 安装 Odoo 19 于 Ubuntu 24.04，并配置 VSCode 开发环境

以下是详细的安装步骤，基于 Odoo 19 的官方源代码安装指南（适用于开发环境），并适应使用您登录的系统用户名 "bincoo" 进行配置。
这意味着我们不会创建新的系统用户，而是直接使用 "bincoo" 用户来运行 Odoo、PostgreSQL 和相关进程。
整个过程假设您已以 "bincoo" 用户登录终端，并具有 sudo 权限。如果不是，请先切换到 "bincoo" 用户（使用 su - bincoo）。


