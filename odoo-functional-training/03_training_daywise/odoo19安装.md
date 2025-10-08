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

创建配置文件以指定数据库用户等。

1.复制模板配置文件：

```bash
cp ~/odoo-dev/src/debian/odoo.conf ~/odoo-dev/odoo.conf
```

2.编辑 ~/odoo-dev/odoo.conf（使用 nano 或 vim）：

```bash
nano ~/odoo-dev/odoo.conf
```

添加/修改以下内容（使用 "bincoo" 作为数据库用户；如果设置了密码，添加 db_password）：

```ini
[options]
admin_passwd = admin  ; 管理员密码，用于创建数据库
db_host = False
db_port = False
db_user = bincoo
db_password = False  ; 如果无密码，留空
addons_path = /home/bincoo/odoo-dev/src/addons  ; 如果有自定义模块，添加路径
logfile = /home/bincoo/odoo-dev/odoo.log
````
3.初始化数据库（激活 venv 后运行）：

```bash
source ~/odoo-dev/venv/bin/activate
~/odoo-dev/src/odoo-bin -c ~/odoo-dev/odoo.conf -d odoo-dev -i base --stop-after-init
deactivate
```


### 步骤 7: 测试运行 Odoo

1.激活 venv 并运行 Odoo：

```bash
cd ~/odoo-dev
source venv/bin/activate
~/odoo-dev/src/odoo-bin -c odoo.conf
```
2.在浏览器访问 http://localhost:8069，使用 admin/admin 登录（如果初始化成功）。

3.停止：按 Ctrl+C。

（可选）设置 systemd 服务（生产用，非开发必须）：

1.创建服务文件：

```bash
sudo nano /etc/systemd/system/odoo.service
```

内容：

```ini
[Unit]
Description=Odoo
After=postgresql.service

[Service]
User=bincoo
Group=bincoo
ExecStart=/home/bincoo/odoo-dev/venv/bin/python3 /home/bincoo/odoo-dev/src/odoo-bin -c /home/bincoo/odoo-dev/odoo.conf
StandardOutput=journal+console

[Install]
WantedBy=multi-user.target
```
2.启用并启动：

```bash
sudo systemctl daemon-reload
sudo systemctl start odoo
sudo systemctl enable odoo
```

| 命令                 	|   作用   	|     场景      |
| --------------------- |----------- | ------------ |
| sudo systemctl start odoo	| 启动服务	| 首次启动或停止后重新启动。 |
| sudo systemctl stop odoo	| 停止服务	| 暂时关闭 Odoo。 |
| sudo systemctl restart odoo |	重启服务 |	停止后立即重新启动，常用于修改 Odoo 配置文件后。 |
| sudo systemctl status odoo	| 查看状态 |	检查 Odoo 是否在运行，以及最近的运行日志。 |
| sudo systemctl enable odoo	| 设置开机自启	| 确保服务器重启后 Odoo 自动启动。 |
| sudo systemctl disable odoo |	取消开机自启	阻止 Odoo 随系统启动。 |


### 步骤 8: 安装 Visual Studio Code

安装：

```bash
sudo snap refresh core20 --channel=stable --revision=2669
```

命令说明：
sudo snap refresh core20: 告诉系统更新或安装 core20 这个 snap 包。

--channel=stable: 指定从稳定（stable）频道获取。

--revision=2669: 强制系统下载并切换到编号为 2669 的版本。

### 步骤 9: 配置 VSCode 为 Odoo 开发环境

1.打开 VSCode 项目：启动 VSCode，打开文件夹 /home/bincoo/odoo-dev/src。
2.安装推荐扩展（在 VSCode 的 Extensions 面板搜索并安装）：

- 找到名为 "Chinese (Simplified) (简体中文) Language Pack for Visual Studio Code" 的扩展（通常由 Microsoft 发布）。
   - 按下快捷键 Ctrl + Shift + P 打开 Command Palette（命令面板）。
   - 在命令面板中输入 display，然后选择并运行 "Configure Display Language"（配置显示语言）命令。
   - 系统会弹出一个语言列表。选择 "zh-hans" (简体中文)。
   - VS Code 会提示您重启以应用更改。点击 "Restart"（重启）按钮。
- Python (by Microsoft)：用于 Python 支持和调试。
- Pylint 或 Ruff：代码 linting。
- XML (by Red Hat)：Odoo 使用 XML 视图。
- Cybrosys Assista (Odoo Helper)：Odoo 专用扩展，提供 snippets、模块导航和命令（如果可用，从 VSCode Marketplace 搜索）。
- GitLens：Git 集成。
- Odoo Snippets（如果有第三方扩展）。










