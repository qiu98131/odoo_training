# 📘 Day 01 – Initial Setup

## 🎯 Objectives
- Install and configure Odoo on a local environment.  
- Create the company profile for **Zalino Tech**.  
- Configure basic user accounts and access rights.  
- Understand the Odoo interface and navigation.  

---

## 🛠️ Steps

### 1. Odoo Installation
- Install **Odoo 16/17/18** (depending on your environment).  
- Setup PostgreSQL database.  
- Start Odoo service and access via browser (`http://localhost:8069`).  

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
