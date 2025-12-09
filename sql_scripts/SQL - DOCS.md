# Healthcare SQL – KPI Dashboard (Q&A)

> **Tags:** #SQL #Healthcare #KPI #Dashboards #Study_Notes  
> **Format:** Business Questions + SQL Interpretation (English + Arabic)

---

## Part 0 – Database Structure & Relationships

### Q0) What does the database structure and constraints represent?

The script:

```sql
CREATE DATABASE Healthcare;

ALTER TABLE Appointments
ADD CONSTRAINT FK_Appointments_Patients
FOREIGN KEY (PatientID) REFERENCES Patients(PatientID);

ALTER TABLE Appointments
ADD CONSTRAINT FK_Appointments_Doctors
FOREIGN KEY (DoctorID) REFERENCES Doctors(DoctorID);
```

**Explanation (EN):**  
- Creates the **Healthcare** database.  
- Adds **foreign keys** meaning each appointment must be linked to a valid **Patient** and **Doctor**.  
- Ensures **data integrity** and avoids appointments without real patients/doctors.

**الشرح (AR):**  
- إنشاء قاعدة بيانات باسم Healthcare.  
- ربط جدول المواعيد بجدول المرضى والأطباء لضمان صحة البيانات.  
- يمنع وجود موعد بدون مريض أو بدون دكتور.

---

## Q1) Medical Center KPI Overview

```sql
SELECT 
    (SELECT COUNT(*) FROM Appointments) AS TotalAppointments,
    (SELECT SUM(Revenue) FROM Revenues) AS TotalRevenues,
    (SELECT SUM(Amount) FROM Expenses) AS TotalExpenses,
    (SELECT COUNT(*) FROM Patients) AS TotalPatients,
    (SELECT COUNT(*) FROM Doctors) AS TotalDoctors,
    (SELECT SUM(Revenue) FROM Revenues) - (SELECT SUM(Amount) FROM Expenses) AS NetProfit;
```

**Explanation (EN):** High-level dashboard:  
- Total appointments, total revenue, total expenses, total patients, total doctors.  
- Net profit = revenues − expenses.

**الشرح (AR):**  
ده Dashboard سريع:  
- إجمالي المواعيد – المرضى – الدكاترة – الإيراد – المصروف.  
- صافي الربح = الإيرادات - المصروفات.

---

## Q2) No-Show Rate

```sql
SELECT 
    ROUND(
        SUM(CASE WHEN Status = 'No-Show' THEN 1 ELSE 0 END) * 100.0 
        / NULLIF(COUNT(*), 0),
    2) AS NoShowRate
FROM Appointments;
```

**Explanation (EN):** Calculates % of appointments marked No-Show.  
**الشرح (AR):** نسبة عدم حضور المرضى.

---

## Q3) Attendance Rate

```sql
SELECT 
    ROUND(
        SUM(CASE WHEN Status = 'Show' THEN 1 ELSE 0 END) * 100.0
        / NULLIF(COUNT(*), 0),
    2) AS AttendanceRate
FROM Appointments;
```

**Explanation:** Opposite of no-show — % attended appointments.  
**الشرح:** نسبة حضور المرضى.

---

## Q4) Busy Doctors

```sql
SELECT 
    d.DoctorName,
    COUNT(*) AS TotalAppointments
FROM Appointments a
JOIN Doctors d ON a.DoctorID = d.DoctorID
GROUP BY d.DoctorName
ORDER BY TotalAppointments DESC;
```

**Explanation:** Shows which doctors have the most appointments.  
**الشرح:** ترتيب الدكاترة حسب انشغالهم.

---

## Q5) Revenue Trends (Monthly)

```sql
SELECT 
    FORMAT([Date], 'yyyy-MM') AS YearMonth,  
    SUM(Revenue) AS TotalRevenue
FROM Revenues
GROUP BY FORMAT([Date], 'yyyy-MM')
ORDER BY YearMonth;
```

**Explanation:** Revenue per month.  
**الشرح:** اتجاه الإيرادات شهريًا.

---

## Q6) Expense Trends (Monthly)

```sql
SELECT 
    FORMAT(Date, 'yyyy-MM') AS YearMonth,
    SUM(Amount) AS TotalExpenses
FROM Expenses
GROUP BY FORMAT(Date, 'yyyy-MM')
ORDER BY YearMonth;
```

**Explanation:** Expenses per month.  
**الشرح:** اتجاه المصروفات شهريًا.

---

## Q7) Returning Patients

```sql
SELECT 
    p.PatientID,
    p.[Name],
    COUNT(*) AS VisitCount
FROM Appointments a
JOIN Patients p ON a.PatientID = p.PatientID
GROUP BY p.PatientID, p.[Name]
HAVING COUNT(*) > 1
ORDER BY VisitCount DESC;
```

**Explanation:** Patients who visited more than once.  
**الشرح:** المرضى اللي رجعوا أكتر من مرة.

---

## Q8) Specialty Performance

```sql
SELECT 
  a.Specialty,
  a.AppointmentCount,
  COALESCE(r.TotalRevenue, 0) AS TotalRevenue
FROM (
  SELECT Specialty, COUNT(*) AS AppointmentCount
  FROM Appointments
  GROUP BY Specialty
) AS a
LEFT JOIN (
  SELECT Specialty, SUM(Revenue) AS TotalRevenue
  FROM Revenues
  GROUP BY Specialty
) AS r
  ON a.Specialty = r.Specialty
ORDER BY TotalRevenue DESC;
```

**Explanation:** Combines appointment volume + revenue by specialty.  
**الشرح:** أداء كل تخصص حسب عدد المواعيد وإجمالي الإيراد.

---

## Q9) Appointment Volume (Monthly)

```sql
SELECT 
    FORMAT([Date], 'yyyy-MM') AS YearMonth,
    COUNT(*) AS AppointmentCount
FROM Appointments
GROUP BY FORMAT([Date], 'yyyy-MM')
ORDER BY YearMonth;
```

**Explanation:** Number of appointments per month.  
**الشرح:** حجم المواعيد شهريًا.

---

## Q10) Patient Distribution by City

```sql
SELECT 
    City,
    COUNT(*) AS PatientCount
FROM Patients
GROUP BY City
ORDER BY PatientCount DESC;
```

**Explanation:** Where most patients live.  
**الشرح:** توزيع المرضى حسب المدينة.

---

## Q11) Major Expenses

```sql
SELECT 
    Type,
    SUM(Amount) AS TotalExpenses
FROM Expenses
GROUP BY Type
ORDER BY TotalExpenses DESC;
```

**Explanation:** Biggest expense categories.  
**الشرح:** أكثر أنواع المصروف تكلفة.

---

## Q12) Gender Distribution

```sql
SELECT 
    Gender,
    COUNT(*) AS NumberOfPatients
FROM Patients
GROUP BY Gender;
```

**Explanation:** Patient gender demographics.  
**الشرح:** توزيع المرضى حسب النوع.

---

## Q13) Age Group Distribution

```sql
SELECT
  CASE
    WHEN Age < 20 THEN '<20'
    WHEN Age BETWEEN 20 AND 40 THEN '20-40'
    ELSE '40+'
  END AS AgeGroup,
  COUNT(*) AS PatientCount
FROM Patients
GROUP BY
  CASE
    WHEN Age < 20 THEN '<20'
    WHEN Age BETWEEN 20 AND 40 THEN '20-40'
    ELSE '40+'
  END;
```

**Explanation:** Categorizes patients into age groups.  
**الشرح:** توزيع المرضى حسب الفئة العمرية.
