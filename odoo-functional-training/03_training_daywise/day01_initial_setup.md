# 📘 Day 01 – Initial Setup

## 🎯 Objectives
- Install and configure Odoo on a local environment.  
- Create the company profile for **Zalino Tech**.  
- Configure basic user accounts and access rights.  
- Understand the Odoo interface and navigation.  

---

## 🛠️ Steps

### 1. Odoo Installation
This document provides step-by-step instructions to install **Odoo 18** on **Ubuntu 24.04** using a dedicated Linux user, PostgreSQL user, and Python virtual environment.

---

#### 1. Create System User

```bash
sudo adduser --system --home=/opt/odoo18 --group odoo18
sudo usermod -s /bin/bash odoo188

```

* `--system`: Creates a system user.
* `--home=/opt/odoo18`: Sets Odoo home directory.
* `--group`: Creates a group with the same name.

---

#### 2. Install Required Packages

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y git python3 python3-pip python3-venv \
    build-essential libpq-dev libxml2-dev libxslt1-dev \
    libldap2-dev libsasl2-dev libtiff5-dev libjpeg-dev \
    libopenjp2-7-dev zlib1g-dev libfreetype6-dev \
    liblcms2-dev libwebp-dev libharfbuzz-dev libfribidi-dev \
    libxcb1-dev libpq5 wkhtmltopdf
```

> **Note:** `wkhtmltopdf` is required for printing PDF reports in Odoo.

---

#### 3. Install PostgreSQL and Create User

```bash
sudo apt install -y postgresql
```

Create a PostgreSQL user `odoo18`:

```bash
sudo -u postgres createuser -s odoo18
```

---

#### 4. Get Odoo Source Code

Switch to `odoo18` user:

```bash
sudo -i -u odoo18
```

Clone Odoo 18 source code:

```bash
cd /opt/odoo18
wget https://github.com/odoo/odoo/archive/refs/heads/18.0.zip -O odoo18.zip
```

Unzip & remove extra content:

```bash
unzip odoo18.zip
mv odoo-18.0/* odoo-18.0/.[!.]* .
rm -rf odoo-18.0 odoo18.zip
```

---

#### 5. Create Python Virtual Environment

Inside `/opt/odoo18`:

```bash
cd /opt/odoo18
python3 -m venv venv
source venv/bin/activate
```

Upgrade pip:

```bash
pip install --upgrade pip wheel setuptools
```

Install Python dependencies:

```bash
pip install -r requirements.txt
```

Deactivate venv & exit from odoo18 user:

```bash
deactivate
exit
```

---

#### 6. Create Odoo Configuration File

```bash
sudo nano /etc/odoo18.conf
```

Add the following:

```ini
[options]
; Database
admin_passwd = 12345
db_host = False
db_port = False
db_user = odoo18
db_password = False
xmlrpc_port = 8069

addons_path = /opt/odoo18/addons
```
Grant Access to odoo user:

```bash
sudo chown odoo18:odoo18 /etc/odoo18.conf
sudo chmod 640 /etc/odoo18.conf
```

---

#### 7. Verify Installation

Run the odoo server by command:

```bash
cd /opt/odoo18
source venv/bin/activate 
python3 odoo-bin -c /etc/odoo18.conf
```

Odoo should be running at:

```
http://localhost:8069
```   

If you face an error:
```
- Try to read the error text
- If it is some dependancey issue than install it by using "pip install dependancy_name
- If it is port related issue, than try to kill the servic which is using the same port
- If it is permission like issue than try to grant access to 'odoo18' user
- If it is database related issue than fix it by edit postgres config file
```

---

#### ✅ Installation Completed!

Odoo 18 is now installed and running on your Ubuntu 24.04 system with a dedicated user, PostgreSQL setup, and virtual environment.

---

### 2. Create a New Database
- On the Odoo login page, click **"Create Database"**.  
- Database Name: `zalino_tech`  
- Email: `admin@zalino.com`  
- Password: *(your secure password)*  
- Language: English (EN)  
- Country: *(choose default branch location, e.g., Pakistan)*  
- Load Demo Data: ✅ (enabled for testing purposes).  

### 3. Company Profile Setup
- Go to **Settings → Companies → Manage Companies**.  
- Edit default company → Rename to **Zalino Tech**.  
- Add company logo (placeholder if not available).  
- Add contact information (address, phone, email, website).  
- Configure default currency (USD/EUR/PKR based on global operations).  
- Configure time zone (UTC recommended).  

### 4. User & Access Rights
- Go to **Settings → Users & Companies → Users**.  
- Create sample users:
  - `ceo@zalino.com` (Administrator, all access).  
  - `manager.isb@zalino.com` (Branch Manager – Islamabad).  
  - `employee@zalino.com` (Basic Employee access).  
- Assign groups (e.g., *Administration / Employee / Manager*).  

### 5. Explore Odoo Interface
- Learn top menu navigation.  
- Explore **Apps module** and understand installation.  
- Explore **Discuss app** for internal communication.  
- Test switching between users (admin vs. employee).  

---

## 📝 Exercises
1. Create the **Zalino Tech** company profile and upload a temporary logo.  
2. Add at least **3 new users** with different roles and permissions.  
3. Configure currency as **USD** and change the time zone to **UTC**.  
4. Switch to an employee account and verify restricted access.  
5. Install at least **2 core apps** (CRM & Sales) to prepare for Day 02.  

---

## 📌 Notes
- Always use **demo data** in training to simulate real-world scenarios.  
- Keep consistent naming conventions for users and companies.  
- This setup will be the **foundation** for all future training sessions.  

---

✅ *By the end of this session, Odoo is installed, Zalino Tech company is created, and basic users are configured for testing.*  
