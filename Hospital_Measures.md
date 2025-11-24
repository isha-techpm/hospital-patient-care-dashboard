# 🏥 Hospital Patient Care Dashboard – DAX Measures

This file contains all the DAX measures used to calculate KPIs and visuals in the **Hospital Patient Care Dashboard** built using Power BI.

The measures support operational KPIs, patient satisfaction metrics, wait times, length of stay, departmental performance, and readmissions.

---

## 📊 1. Patient Volume Measures

### **Total Patients**
```DAX
Total Patients =
DISTINCTCOUNT ( Patient[PatientID] )
```

### **Total Patients Admitted**
```DAX
Total Patients Admitted =
COUNTROWS ( Admissions )
```

### **Total Inpatients**
```DAX
Total Inpatients =
CALCULATE (
    [Total Patients],
    KEEPFILTERS ( Admissions[PatientType] = "Inpatient" )
)
```

### **Total Outpatients**
```DAX
Total Outpatients =
CALCULATE (
    [Total Patients],
    KEEPFILTERS ( Admissions[PatientType] = "Outpatient" )
)
```

---

## 💰 2. Treatment Cost Measures

### **Total Treatment Cost**
```DAX
Total Treatment Cost =
SUM ( TreatmentCosts[TotalCost] )
```

### **Average Treatment Cost**
```DAX
Avg Treatment Cost =
DIVIDE ( [Total Treatment Cost], [Total Patients Admitted] )
```

---

## ⏱️ 3. Wait Time Measures

### **Average ER Wait Time (Minutes)**
```DAX
Avg ER Wait Time (mins) =
AVERAGE ( WaitingTimes[ERWaitMinutes] )
```

### **Average Wait Time by Department**
(Same measure — grouped by Department in the visual)
```DAX
Avg Wait Time by Department =
[Avg ER Wait Time (mins)]
```

---

## 🧑‍⚕️ 4. Staff Measures

### **Total Staff**
```DAX
Total Staff =
COUNTROWS ( Staff )
```

### **Available Staff**
```DAX
Available Staff =
CALCULATE (
    [Total Staff],
    KEEPFILTERS ( Staff[IsAvailable] = TRUE () )
)
```

---

## 😀 5. Patient Satisfaction Measures

### **Total Survey Responses**
```DAX
Total Survey Responses =
COUNTROWS ( SatisfactionSurvey )
```

### **Overall Satisfaction Score**
```DAX
Overall Satisfaction Score =
AVERAGE ( SatisfactionSurvey[OverallSatisfactionScore] )
```

### **% Neutral**
```DAX
Satisfaction % Neutral =
DIVIDE (
    CALCULATE (
        COUNTROWS ( SatisfactionSurvey ),
        SatisfactionSurvey[OverallSatisfactionLabel] = "Neutral"
    ),
    [Total Survey Responses]
)
```

### **% Excellent**
```DAX
Satisfaction % Excellent =
DIVIDE (
    CALCULATE (
        COUNTROWS ( SatisfactionSurvey ),
        SatisfactionSurvey[OverallSatisfactionLabel] = "Excellent"
    ),
    [Total Survey Responses]
)
```

### **% Good**
```DAX
Satisfaction % Good =
DIVIDE (
    CALCULATE (
        COUNTROWS ( SatisfactionSurvey ),
        SatisfactionSurvey[OverallSatisfactionLabel] = "Good"
    ),
    [Total Survey Responses]
)
```

### **Treatment Explained Score**
```DAX
Explain Treatment Score =
AVERAGE ( SatisfactionSurvey[ExplainTreatmentScore] )
```

### **Trust in Physician Score**
```DAX
Trust Physician Score =
AVERAGE ( SatisfactionSurvey[TrustPhysicianScore] )
```

---

## 🛏️ 6. Length of Stay & Bed Occupancy

### **Average Days Of Stay**
```DAX
Average Days Of Stay =
AVERAGE ( BedOccupancy[LengthOfStayDays] )
```

### **LOS Distribution (Count of Patients)**
```DAX
LOS Patients Count =
COUNTROWS ( BedOccupancy )
```

---

## 🔁 7. Readmissions

### **Readmissions Within 30 Days**
```DAX
Readmissions Within 30 Days =
COUNTROWS (
    FILTER (
        Readmissions,
        Readmissions[Within30DaysFlag] = TRUE ()
    )
)
```

### **Readmissions Within 30 Days (Quarterly)**
```DAX
Readmissions Within 30 Days (QoQ) =
[Readmissions Within 30 Days]
```

(Plotted with Quarter on the axis)

---

## 🎛️ 8. Utility Measures (Used for Filters/Slicers)

### **Selected Year**
```DAX
Selected Year =
SELECTEDVALUE ( Admissions[Year] )
```

### **Selected Department**
```DAX
Selected Department =
SELECTEDVALUE ( Admissions[Department] )
```

---

## ✅ Summary

This measure file covers the full set of KPIs used across the dashboard:

- Patient volumes & admission trends  
- Treatment cost & wait time metrics  
- Staff availability  
- Patient satisfaction scores  
- Length of stay analytics  
- Readmissions tracking  



