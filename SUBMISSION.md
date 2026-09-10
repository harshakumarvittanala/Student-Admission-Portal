# School Management System: Final Submission Handbook
**Platform**: Zoho CRM (Administrative Core) & Zoho Creator (Parent Portal)  
**Author**: Solution Engineering  
**Version**: 1.0 (Production-Ready)

---

## Executive Summary & Deliverables Directory

This project delivers an end-to-end **School Management System** designed to streamline school admissions, academic operations, student tracking, attendance enforcement, examination grading, fee realization, and parent communication.

| Deliverable Item | File Location / Reference | Description |
| :--- | :--- | :--- |
| **Administration Portal Login** | [admin_login.html](file:///c:/Users/IT%2057/Desktop/project%2057/Pro%2057/admin_login.html) | Interactive Staff & Faculty login portal with role switching, live KPI dashboard & Deluge simulation. |
| **Working Admission Webform** | [webform/admission_enquiry.html](file:///c:/Users/IT%2057/Desktop/project%2057/webform/admission_enquiry.html) | Responsive, multi-step HTML5/CSS3/JS Web-to-Lead form with validation & honeypot anti-spam. |
| **Webform Styling** | [webform/webform_styles.css](file:///c:/Users/IT%2057/Desktop/project%2057/webform/webform_styles.css) | Modern UI styles, typography, card shadows, step indicators, and mobile responsive rules. |
| **Data Structure & ERD** | [docs/Data_Model_and_Dictionary.md](file:///c:/Users/IT%2057/Desktop/project%2057/docs/Data_Model_and_Dictionary.md) | Full 14-module data dictionary, field definitions, constraints, and Mermaid ER diagram. |
| **CRM–Creator Integration**| [docs/CRM_Creator_Integration_Guide.md](file:///c:/Users/IT%2057/Desktop/project%2057/docs/CRM_Creator_Integration_Guide.md) | Complete integration architecture, sync flows, and Parent Portal row-level security setup. |
| **Reports & Dashboards** | [docs/Reports_and_Dashboards_Setup.md](file:///c:/Users/IT%2057/Desktop/project%2057/docs/Reports_and_Dashboards_Setup.md) | 5 Executive dashboards and reports (Admissions, Academics, Attendance, Exams, Fees). |
| **Additional Feature Docs** | [docs/Additional_Feature_Documentation.md](file:///c:/Users/IT%2057/Desktop/project%2057/docs/Additional_Feature_Documentation.md) | 360° Student Early-Warning & Intervention Engine: Problem, algorithm & optimization. |
| **Deluge: Lead Conversion**| [deluge_scripts/01_lead_to_student_conversion.dg](file:///c:/Users/IT%2057/Desktop/project%2057/deluge_scripts/01_lead_to_student_conversion.dg) | Converts confirmed lead, generates unique `STU-YYYY-XXXX` ID, sets up fee invoice. |
| **Deluge: Attendance** | [deluge_scripts/02_attendance_validation_rollup.dg](file:///c:/Users/IT%2057/Desktop/project%2057/deluge_scripts/02_attendance_validation_rollup.dg) | Prevents duplicate records on `StudentID_Date` & rolls up attendance % with alert. |
| **Deluge: Exam & GPA** | [deluge_scripts/03_exam_marks_grade_calculator.dg](file:///c:/Users/IT%2057/Desktop/project%2057/deluge_scripts/03_exam_marks_grade_calculator.dg) | Validates marks, assigns letter grades (A+ to F), and calculates cumulative GPA. |
| **Deluge: Fee Ledger** | [deluge_scripts/04_fee_installment_payment_rollup.dg](file:///c:/Users/IT%2057/Desktop/project%2057/deluge_scripts/04_fee_installment_payment_rollup.dg) | Manages multi-installment payments, rolls up balances, and tracks defaulters. |
| **Deluge: Creator Sync** | [deluge_scripts/05_crm_to_creator_sync.dg](file:///c:/Users/IT%2057/Desktop/project%2057/deluge_scripts/05_crm_to_creator_sync.dg) | Syncs CRM student records to Creator Parent Portal shadow forms. |
| **Deluge: 360° Risk Engine**| [deluge_scripts/06_additional_feature_risk_engine.dg](file:///c:/Users/IT%2057/Desktop/project%2057/deluge_scripts/06_additional_feature_risk_engine.dg) | Optimized $O(N)$ batch script scoring student risk and triggering interventions. |

---

## 1. Access Instructions & Implementation Blueprint

### A. Zoho CRM Implementation Access
1. **Modules Configured**:
   - `Leads` (Configured for Admissions pipeline)
   - `Students` (Custom master profile)
   - `Academic_Years`, `Classes`, `Sections`, `Subjects`, `Teachers`
   - `Class_Allocations` (Junction module linking Section-Subject-Teacher)
   - `Attendance` (Daily records with unique composite index)
   - `Examinations` & `Exam_Results` (Assessments, marks, and GPA rollup)
   - `Fee_Invoices` & `Payments` (Installment ledger and receipts)
   - `Academic_History` (Historical progression snapshots)
2. **Access Credentials & Account**:
   - Built for standard Zoho One / Zoho CRM trial account.
   - To link your environment, deploy the Deluge scripts in **Setup $\rightarrow$ Developer Space $\rightarrow$ Functions** and map the workflow rules as described in the script comments.

### B. Zoho Creator Parent Application Access
1. **Application**: `School Parent Portal`
2. **Forms & Views**:
   - `Students_Portal_View`
   - `Attendance_Portal_View`
   - `Exam_Results_Portal_View`
   - `Fees_Portal_View`
3. **Parent Login & Row-Level Security**:
   - Configured via **Customer Portal**.
   - Parents log in using their email registered in CRM (`Parent_Email`).
   - All report criteria enforce: `Parent_Email == zoho.loginuser`.

### C. Working Zoho CRM Webform
- The responsive webform is located at: [webform/admission_enquiry.html](file:///c:/Users/IT%2057/Desktop/project%2057/webform/admission_enquiry.html).
- Supports live client-side validation, grade-based age limits, responsive layout, honeypot anti-spam protection, and direct HTTP POST submission to Zoho CRM Web-to-Lead endpoint (`https://crm.zoho.com/crm/WebToLeadForm`).

---

## 2. Explanation of Overall Data Structure & Relationships

The data structure is built on a normalized relational model designed for consistency and high query performance:

1. **Admissions to Enrollment Pipeline**:
   - Admissions inquiries enter `Leads` via the webform.
   - Upon confirmation, a Deluge workflow converts the lead into a permanent record in `Students`, generating an auto-incremented, format-standardized lifelong identifier: `STU-YYYY-XXXX`.
2. **Academic Structure & Allocations**:
   - `Academic_Years` contains `Classes` (e.g. Grade 10).
   - `Classes` are divided into `Sections` (e.g. 10-A, 10-B) with an assigned `Class_Teacher`.
   - `Class_Allocations` acts as a junction module linking `Section`, `Subject`, and `Teacher`, cleanly resolving the many-to-many relationship of faculty assignments.
3. **Attendance Tracking & Duplicate Prevention**:
   - Each attendance record contains a composite key: `Student_ID + "_" + Attendance_Date`.
   - A pre-save validation script rejects duplicate submissions for the same student on the same day.
   - On save, Deluge calculates `Total Working Days`, `Days Present`, and `Attendance Percentage` and rolls up the summary to the `Students` master record.
4. **Examinations & Grading**:
   - `Exam_Results` validates that `Marks_Obtained <= Max_Marks`, calculates percentages, assigns letter grades (`A+` to `F`), and updates the student's cumulative GPA.
5. **Fee & Payment Ledger**:
   - `Fee_Invoices` structures tuition across installments.
   - `Payments` logs each transaction and rolls up `Amount_Paid` and `Outstanding_Amount` to both the invoice and the global student ledger.
6. **Historical Preservation**:
   - At the conclusion of an academic year, a snapshot of attendance, GPA, and completed grade is written to `Academic_History`, preserving full historical progression across grades.

---

## 3. Explanation of CRM–Creator Integration

To combine the administrative power of Zoho CRM with the parent-facing flexibility of Zoho Creator without incurring lag or duplicate clutter, we use an **Event-Driven Push Sync with Row-Level Security**:

1. **Event-Driven Push**:
   - Changes made in CRM (attendance, marks, fee payments, profile updates) trigger Deluge workflow functions (`zoho.creator.updateRecord` / `createRecord`).
   - Data is upserted into Creator shadow forms using `Student_ID` as the primary key.
2. **Strict Parent Isolation**:
   - Parents authenticate through the **Zoho Creator Customer Portal**.
   - Row-level security restricts view permissions to records where `Parent_Email == zoho.loginuser`.
   - Parents cannot query or view data for any child other than their own.
3. **Multi-Child Families**:
   - If a parent has multiple children enrolled in different grades, both student records share the parent's email. The parent portal provides an interactive child selector dropdown to view each child's academic, attendance, and fee status independently.

---

## 4. Details of the Additional Feature Implemented

### Feature Name
**Automated 360° Student Early-Warning & Intervention Engine**

### 1. The Problem Identified
In traditional school management, academic performance, truancy, and fee arrears are managed in isolation. Teachers don't know when a student's family is experiencing financial distress, and the administration doesn't notice academic deterioration until term-end report cards are published. As a result, interventions are reactive, student retention suffers, and recovery becomes difficult.

### 2. Strategic Value & Selection
This feature transforms Zoho CRM into an intelligent, proactive decision-support system. By synthesizing data across **Academics (40%)**, **Attendance (40%)**, and **Fee Compliance (20%)**, it computes a composite **Student Risk Score (0-100)** and **Health Index (0-100)**:
- **Low Risk ($0-29$)**: Student is healthy; normal monitoring.
- **Moderate Risk ($30-59$)**: Early watch; flagged on class teacher's weekly advisory.
- **High Risk ($60-100$)**: Critical intervention required:
  1. Auto-assigns a high-priority counseling Task in CRM to the Class Teacher and Guidance Counselor.
  2. Sends an automated parent advisory email offering tutorial assistance and scheduling a consultation.
  3. Displays an alert badge in both CRM and the Creator Parent Portal.

### 3. Code Optimization & Scalability
- **$O(N)$ Single-Pass Processing**: Naive scripts query related attendance, exam, and invoice tables for every student, making $3N$ API calls and causing timeouts on large cohorts. Our architecture relies on pre-aggregated rollup fields maintained on the `Students` master record by Scripts 02, 03, and 04, enabling the Risk Engine to evaluate hundreds of students in a single pass without making sub-queries.
- **Governor-Limit Pagination**: Processes records in chunks of 100 using `zoho.crm.getRecords(..., pageIndex, 100)` within execution limits.
- **Idempotency & Alert Throttling**: Checks `Last_Intervention_Triggered_Date` and enforces a 14-day cooldown window to prevent notification fatigue.
- **Atomic Updates**: All risk scores, tiers, and flags are committed in a single atomic `zoho.crm.updateRecord()` call per student.
