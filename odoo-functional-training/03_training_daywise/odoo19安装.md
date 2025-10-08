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

安装必要的系统依赖，包括 Python、开发工具、Node.js 等（这些是 Odoo 运行和编译所需的）。

```bash
sudo apt install -y python3 python3-pip python3-dev python3-venv git wget build-essential libxml2-dev libxslt1-dev zlib1g-dev libsasl2-dev libldap2-dev libssl-dev libffi-dev libjpeg-dev libpq-dev liblcms2-dev libblas-dev libatlas-base-dev libharfbuzz-dev libfribidi-dev libxcb1-dev xfonts-75dpi node-less npm
```
安装 Node.js 的额外工具（用于 CSS 处理）：

```bash
sudo npm install -g less less-plugin-clean-css rtlcss
```
### 步骤 2: 安装并配置 PostgreSQL

Odoo 使用 PostgreSQL 作为数据库后端。我们将创建与 "bincoo" 用户匹配的数据库用户（无需密码连接）。

1.安装 PostgreSQL：

```bash
sudo apt install -y postgresql postgresql-client
```
2.启动 PostgreSQL 服务并启用开机自启：

```bash
sudo systemctl start postgresql
sudo systemctl enable postgresql
```

3.以 PostgreSQL 用户创建数据库用户（使用 "bincoo"，并赋予超级用户权限，便于开发）：

```bash
sudo -u postgres createuser -s bincoo
```
4.创建一个开发数据库（例如 "odoo-dev"，您可以稍后在 Odoo 中创建更多数据库）:

```bash
createdb odoo-dev
```

### 步骤 3: 安装 wkhtmltopdf

wkhtmltopdf 用于生成 PDF 报告。Odoo 19 推荐版本 0.12.6（兼容 Ubuntu 24.04）。

1.下载并安装依赖（如果需要旧版 OpenSSL）：
```bash
wget http://archive.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2_amd64.deb
sudo dpkg -i libssl1.1_1.1.1f-1ubuntu2_amd64.deb
sudo apt install -f -y
```
2.下载 wkhtmltopdf：

```bash
wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-3/wkhtmltox_0.12.6.1-3.ubuntu-jammy_amd64.deb
```
3.安装：

```bash
sudo dpkg -i wkhtmltox_0.12.6.1-3.ubuntu-jammy_amd64.deb
sudo apt install -f -y
```
4.验证安装：

```bash
wkhtmltopdf --version
```
应显示版本 0.12.6。

### 步骤 4: 下载 Odoo 19 源代码

我们使用 Git 克隆 Odoo Community Edition（开源版）。如果您有 Enterprise Edition 许可，额外克隆 enterprise 仓库。

1.创建安装目录（在 home 下）：

```bash
mkdir -p ~/odoo-dev/src
cd ~/odoo-dev/src
```

2.克隆 Odoo 19（仅 19.0 分支，浅克隆以节省空间）：

```bash
git clone https://github.com/odoo/odoo --depth 1 --branch 19.0 --single-branch .
```

3.如果需要 Enterprise Edition（可选）：

```bash
cd ~/odoo-dev/src
git clone https://github.com/odoo/enterprise --depth 1 --branch 19.0 --single-branch enterprise
```

### 步骤 5: 设置 Python 虚拟环境并安装依赖

虚拟环境隔离 Odoo 的 Python 依赖。

1.创建虚拟环境：

```bash
cd ~/odoo-dev
python3 -m venv venv
```

2.激活虚拟环境：

```bash
source venv/bin/activate
```

3.升级 pip 并安装 wheel（基础工具）：

```bash
pip install --upgrade pip
pip install wheel setuptools inotify psycopg2-binary
```

4.安装 Odoo 依赖（从 requirements.txt）：

```bash
pip install -r ~/odoo-dev/src/requirements.txt
```

5.停用虚拟环境（稍后运行时再激活）：

```bash
deactivate
```

### 步骤 6: 配置 Odoo





