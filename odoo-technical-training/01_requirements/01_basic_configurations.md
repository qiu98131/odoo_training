# 📘 01_Basic_Configurations

**Module:** Basic Configurations  
**Purpose:** Establish the foundational setup to support multiple institutions, campuses, curriculums, and academic structures. All other modules (Admissions, Attendance, Exams, etc.) depend on this configuration.  

---

## 🎯 Objectives

- Support **multi-institution environments** (schools, colleges, academies, vocational institutes, online learning platforms).  
- Manage **multiple campuses** under a single institution.  
- Handle **parallel curriculums** (Cambridge, IB, Local Board, Vocational).  
- Standardize **academic years, terms, standards, courses, and batches**.  
- Provide flexible tagging and categorization for easy expansion.  

---

## ⚙️ Configuration Models

### 1. Institution / Company
- **Model:** `res.company` (inherited)  
- **Fields:**  
  - Institution Name  
  - Institution Type (School / College / University / Academy / Vocational / Online)  
  - Accreditation / Affiliation ID  
  - Default Currency, Language  
  - Contact Details  

---

### 2. Campus Management
- **Model:** `edu.campus`  
- **Fields:**  
  - Campus Name  
  - Linked Institution (many2one → `res.company`)  
  - City (many2one → `res.city`)  
  - Address & Contact  
  - Facilities (many2many → `edu.facility`)  
  - Capacity  
  - Campus Type (Main / Branch / Online)  

---

### 3. Curriculum Management
- **Model:** `edu.curriculum`  
- **Fields:**  
  - Curriculum Name (Cambridge, IB, Federal Board, Vocational, Custom, etc.)  
  - Curriculum Type (School / Higher Ed / Vocational / Online)  
  - Active (boolean)  

---

### 4. Academic Year & Term
- **Model:** `edu.academic.year`  
  - Name (e.g., 2024–2025)  
  - Start / End Date  
  - Active  
- **Model:** `edu.academic.term`  
  - Name (e.g., Spring 2025)  
  - Linked Academic Year  
  - Start / End Date  

---

### 5. Fee Terms
- **Model:** `edu.fee.term`  
- **Fields:**  
  - Term Name (Monthly, Quarterly, Annual)  
  - Payment Dates (one2many → `edu.fee.term.line`)  
  - Grace Period / Penalty  

---

## 📚 Academic Structure

### 6. Standards / Grades
- **Model:** `edu.standard`  
- **Fields:**  
  - Standard Name (Grade 1, Class 10, Year 1 B.Sc.)  
  - Curriculum (many2one → `edu.curriculum`)  
  - Level (Pre-Primary, Primary, Secondary, Higher, Vocational, Online)  
  - Medium (many2one → `edu.medium`)  

---

### 7. Division / Section
- **Model:** `edu.division`  
- **Fields:**  
  - Division Name (A, B, C…)  
  - Standard (many2one → `edu.standard`)  
  - Capacity  

---

### 8. Medium of Instruction
- **Model:** `edu.medium`  
- **Fields:**  
  - Language (English, Urdu, Arabic, French, etc.)  
  - Script (optional)  

---

### 9. Student KPI Configuration
- **Model:** `edu.student.kpi`  
- **Fields:**  
  - KPI Name (Attendance, Discipline, Participation, Grades, Skills)  
  - KPI Type (Quantitative / Qualitative)  
  - Weightage %  

---

## 🎓 Course & Curriculum

### 10. Courses
- **Model:** `edu.course`  
- **Fields:**  
  - Course Name (B.Sc. Computer Science, Montessori Level 1, Short Course: Digital Marketing)  
  - Course Type (Degree / Diploma / Certification / Workshop / Online)  
  - Duration (Months / Years)  
  - Curriculum (many2one → `edu.curriculum`)  
  - Department (many2one → `edu.department`)  

---

### 11. Batches
- **Model:** `edu.batch`  
- **Fields:**  
  - Batch Name (e.g., 2025-Batch-A)  
  - Linked Course  
  - Start Date / End Date  
  - Capacity  

---

### 12. Department
- **Model:** `edu.department`  
- **Fields:**  
  - Department Name (Science, Commerce, Arts, Engineering, Medicine, Skills)  
  - Head of Department (many2one → `hr.employee`)  

---

### 13. Subjects
- **Model:** `edu.subject`  
- **Fields:**  
  - Subject Name  
  - Subject Type (Core / Elective / Lab / Workshop)  
  - Credits / Hours  
  - Linked Curriculum  
  - Linked Department  

---

### 14. Classrooms
- **Model:** `edu.classroom`  
- **Fields:**  
  - Room / Hall Name  
  - Capacity  
  - Resources (Projector, Smart Board, Lab Equipment, Workshop Tools)  
  - Linked Campus  

---

### 15. Activity Management
- **Model:** `edu.activity`  
- **Fields:**  
  - Activity Name (Sports, Debate, Robotics, Cultural Festival)  
  - Type (Extra-Curricular / Co-Curricular / Vocational Skill)  
  - Responsible Faculty  

---

## 🏫 General Management

### 16. Facilities
- **Model:** `edu.facility`  
- **Fields:**  
  - Facility Name (Library, Hostel, Transport, Playground, Auditorium, Cafeteria)  
  - Facility Type (Academic / Residential / Co-Curricular / Vocational)  
  - Linked Campus  

---

### 17. Categories
- **Model:** `edu.category`  
- **Fields:**  
  - Category Name  
  - Category Type (Student / Faculty / Course / Facility / General)  
  - Description  

---

## 🔗 Dependencies
- Extends: `res.company`, `res.city`, `hr.employee`.  
- Required for: Admissions, Student Management, Exams, Attendance, Fees, Faculty.  

---

✅ This configuration design ensures **multi-campus, multi-institution, and multi-curriculum** education management under one umbrella.  
