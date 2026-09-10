# Zoho CRM Reports & Executive Dashboards Setup Guide

## 1. Overview
This guide provides complete configuration blueprints for the 5 executive management dashboards and underlying analytical reports in Zoho CRM. These reports give school principals, academic heads, admissions officers, and finance directors actionable real-time insights.

---

## 2. Dashboard 1: Admissions & Enrollment Analytics

### Report 1.1: Admission Conversion Funnel
- **Primary Module**: `Leads`
- **Report Type**: Summary Report
- **Group By**: `Lead_Status` (Stage-wise: `New Enquiry` $\rightarrow$ `Contacted` $\rightarrow$ `Campus Visit` $\rightarrow$ `Interview Completed` $\rightarrow$ `Offer Extended` $\rightarrow$ `Admission Confirmed`)
- **Metrics**: Count of Enquiries, Conversion Percentage per stage
- **Chart Type**: Funnel Chart
- **Business Value**: Visualizes enquiry drop-off points in the admission cycle.

### Report 1.2: Lead Volume by Target Grade & Academic Year
- **Primary Module**: `Leads`
- **Report Type**: Matrix Report
- **Row Grouping**: `Grade_Applying_For`
- **Column Grouping**: `Academic_Year_Applying_For`
- **Metrics**: Record Count
- **Chart Type**: Stacked Bar Chart
- **Business Value**: Identifies grade-level demand to assist with classroom allocation.

### Report 1.3: Acquisition Channel Effectiveness
- **Primary Module**: `Leads`
- **Report Type**: Summary Report
- **Group By**: `Lead_Source`
- **Columns**: Total Enquiries, Confirmed Admissions, Conversion Rate %
- **Chart Type**: Donut Chart
- **Business Value**: Measures ROI on school marketing campaigns and walk-ins.

---

## 3. Dashboard 2: Academic & Section Capacity Demographics

### Report 2.1: Classroom Capacity vs. Active Enrollment
- **Primary Module**: `Sections`
- **Related Module**: `Students`
- **Columns**: `Class_Lookup.Class_Name`, `Section_Name`, `Class_Teacher_Lookup.Name`, `Capacity`, `Enrolled_Count`, `Available_Seats` (`Capacity - Enrolled_Count`)
- **Chart Type**: Clustered Bar Chart (`Capacity` vs `Enrolled_Count`)
- **Business Value**: Ensures classrooms do not exceed student teacher ratio guidelines.

### Report 2.2: Subject-Teacher Allocation Matrix
- **Primary Module**: `Class_Allocations`
- **Columns**: `Academic_Year_Lookup.Name`, `Section_Lookup.Name`, `Subject_Lookup.Subject_Name`, `Teacher_Lookup.Name`, `Periods_Per_Week`
- **Filter**: `Academic_Year_Lookup.Is_Current_Year == true`
- **Business Value**: Verifies that every subject in every section has an assigned teacher without scheduling gaps.

---

## 4. Dashboard 3: Student Attendance Compliance & Truancy

### Report 3.1: Regulatory Attendance Compliance (< 75% Attendance)
- **Primary Module**: `Students`
- **Report Type**: Tabular Report
- **Columns**: `Student_ID`, `Name`, `Current_Class.Class_Name`, `Current_Section.Section_Name`, `Attendance_Percentage`, `Days_Absent`, `Parent_Name`, `Parent_Phone`
- **Filter**: `Academic_Status == "Active"` AND `Attendance_Percentage < 75.0`
- **Sorting**: `Attendance_Percentage` Ascending (Lowest attendance first)
- **Business Value**: Mandatory regulatory compliance list for state/board education reporting and prompt guardian escalation.

### Report 3.2: Daily Section Attendance Heatmap
- **Primary Module**: `Attendance`
- **Report Type**: Matrix Report
- **Row Grouping**: `Section_Lookup.Section_Name`
- **Column Grouping**: `Status` (`Present`, `Absent`, `Late`, `Excused`)
- **Filter**: `Attendance_Date == Today`
- **Chart Type**: Horizontal Stacked Bar Chart
- **Business Value**: Real-time morning operational check for school administration.

---

## 5. Dashboard 4: Examination & Academic Performance Heatmap

### Report 4.1: Class Academic Performance & GPA Distribution
- **Primary Module**: `Students`
- **Report Type**: Summary Report
- **Group By**: `Current_Class.Class_Name`
- **Aggregates**: Average `Cumulative_GPA`, Average `Overall_Academic_Percentage`, Max Score, Min Score
- **Chart Type**: Column Chart with Trendline
- **Business Value**: Compares performance across grades to locate curriculum bottlenecks.

### Report 4.2: Subject Failure & Remedial Alert Report
- **Primary Module**: `Exam_Results`
- **Report Type**: Summary Report
- **Group By**: `Subject_Lookup.Subject_Name`
- **Filter**: `Result_Status == "Fail"`
- **Columns**: `Student_Lookup.Name`, `Exam_Lookup.Name`, `Marks_Obtained`, `Passing_Marks`, `Grade`
- **Chart Type**: Pie Chart (Failure distribution across departments)
- **Business Value**: Pinpoints specific subjects where students struggle most, triggering remedial tutoring schedules.

### Report 4.3: Top Academic Achievers (Honor Roll)
- **Primary Module**: `Students`
- **Report Type**: Tabular Report
- **Columns**: `Student_ID`, `Name`, `Current_Class.Class_Name`, `Cumulative_GPA`, `Overall_Academic_Percentage`
- **Filter**: `Cumulative_GPA >= 3.7` AND `Academic_Status == "Active"`
- **Sorting**: `Cumulative_GPA` Descending, Limit: Top 25
- **Business Value**: For awards, scholarships, and honor certificates.

---

## 6. Dashboard 5: Fee Collection, Realization & Defaulter Tracking

### Report 5.1: Overall Fee Realization (Billed vs. Collected vs. Outstanding)
- **Primary Module**: `Fee_Invoices`
- **Report Type**: Summary Report
- **Group By**: `Term_Name`
- **Aggregates**: Sum of `Total_Fee_Amount`, Sum of `Amount_Paid`, Sum of `Outstanding_Amount`
- **Chart Type**: Stacked Bar Chart
- **KPI Widget**: Total Outstanding School Receivables ($₹$)
- **Business Value**: Core financial health monitor for the board of directors.

### Report 5.2: Overdue Fee Defaulters Aging Report
- **Primary Module**: `Fee_Invoices`
- **Report Type**: Tabular Report
- **Columns**: `Invoice_Number`, `Student_Lookup.Student_ID`, `Student_Lookup.Name`, `Student_Lookup.Current_Class`, `Student_Lookup.Parent_Name`, `Student_Lookup.Parent_Phone`, `Due_Date`, `Outstanding_Amount`, `Payment_Status`
- **Filter**: `Payment_Status == "Overdue"`
- **Sorting**: `Due_Date` Ascending (Longest overdue first)
- **Business Value**: Empowers the finance team to execute systematic collection calls and automated SMS reminders.

### Report 5.3: Payment Method Distribution
- **Primary Module**: `Payments`
- **Group By**: `Payment_Method`
- **Aggregates**: Sum of `Amount`
- **Chart Type**: Donut Chart
- **Business Value**: Informs finance of parental preference (Online Gateway vs Bank Transfer vs UPI).
