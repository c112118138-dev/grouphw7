# CareSight 护理資訊系統 ERD

## 實體與屬性

- **Employee（員工）**  
  - EmployeeID (PK)  
  - Name  
  - Title  
  - Department  

- **Patient（病患）**  
  - PatientID (PK)  
  - Name  
  - Gender  
  - BirthDate  
  - AllergyNote  

- **ExamReservation（檢查預約）**  
  - ReservationID (PK)  
  - PatientID (FK → Patient)  
  - Modality (CT/MRI/X-Ray ...)  
  - ExamDateTime  
  - NoticeList (禁食 / 顯影劑 / 過敏提醒)  

- **ExamReport（檢查回報）**  
  - ReportID (PK)  
  - ReservationID (FK → ExamReservation)  
  - CompletedAt  
  - Remark  
  - OperatorID (FK → Employee)  

- **EmployeeLoginLog（員工登入紀錄，組合實體）**  
  - LoginID (PK)  
  - EmployeeID (FK → Employee)  
  - LoginTime  
  - LoginMethod (Password/Finger/Face/Voice)  
  - Result (Success/Fail)  
  - FailCount  

- **SOPStep（SOP 步驟）**  
  - StepID (PK)  
  - Modality  
  - StepName  
  - StepOrder  

- **ExamSOPExecution（檢查 SOP 執行紀錄，組合實體）**  
  - ReservationID (FK → ExamReservation)  
  - StepID (FK → SOPStep)  
  - NurseID (FK → Employee)  
  - ExecutedAt  
  - IsDone (Boolean)  
  - Note  
  - （可將 ReservationID + StepID 設為複合主鍵）

## ERD（Mermaid 語法）

erDiagram
EMPLOYEE ||--o{ EMPLOYEE_LOGIN_LOG : has
EMPLOYEE ||--o{ EXAM_SOP_EXECUTION : executes

PATIENT ||--o{ EXAM_RESERVATION : "has"
EXAM_RESERVATION ||--o{ EXAM_REPORT : "generates"
EXAM_RESERVATION ||--o{ EXAM_SOP_EXECUTION : "includes"
SOP_STEP ||--o{ EXAM_SOP_EXECUTION : "is checked in"

EMPLOYEE {
    string EmployeeID PK
    string Name
    string Title
    string Department
}

PATIENT {
    string PatientID PK
    string Name
    string Gender
    date   BirthDate
    string AllergyNote
}

EXAM_RESERVATION {
    string ReservationID PK
    string PatientID FK
    string Modality
    datetime ExamDateTime
    string NoticeList
}

EXAM_REPORT {
    string ReportID PK
    string ReservationID FK
    datetime CompletedAt
    string Remark
    string OperatorID FK
}

EMPLOYEE_LOGIN_LOG {
    string LoginID PK
    string EmployeeID FK
    datetime LoginTime
    string LoginMethod
    string Result
    int    FailCount
}

SOP_STEP {
    string StepID PK
    string Modality
    string StepName
    int    StepOrder
}

EXAM_SOP_EXECUTION {
    string ReservationID FK
    string StepID FK
    string NurseID FK
    datetime ExecutedAt
    boolean IsDone
    string Note
}
