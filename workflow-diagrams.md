# User Workflow Diagrams (Mermaid)

**Document Information**
- **Version**: 1.0
- **Date**: November 10, 2025
- **Status**: Draft
- **Related Documents**: prd.md, workflows.md

---

## Table of Contents

1. [Candidate Application Flow](#1-candidate-application-flow)
2. [Agency Review Flow](#2-agency-review-flow)
3. [Job Posting Management Flow](#3-job-posting-management-flow)

---

## 1. Candidate Application Flow

### 1.1 Complete Candidate Application Journey

```mermaid
flowchart TD
    Start([Candidate Lands on Platform]) --> Browse[Browse Job Listings]
    Browse --> Filter{Apply Filters?}
    Filter -->|Yes| FilterJobs[Filter by Location/Type/Experience]
    Filter -->|No| ViewJobs[View Job Cards]
    FilterJobs --> ViewJobs

    ViewJobs --> SelectJob[Click on Job Posting]
    SelectJob --> JobDetails[View Full Job Details]

    JobDetails --> Decision{Want to Apply?}
    Decision -->|No| Browse
    Decision -->|Yes| CheckActive{Job Still Active?}

    CheckActive -->|No| ShowClosed[Show: Position Closed]
    ShowClosed --> Browse
    CheckActive -->|Yes| StartApp[Click Apply Now]

    StartApp --> Step1[Step 1: Personal Info]
    Step1 --> ValidateStep1{Valid?}
    ValidateStep1 -->|No| ShowErrors1[Show Validation Errors]
    ShowErrors1 --> Step1
    ValidateStep1 -->|Yes| Step2[Step 2: Professional Info]

    Step2 --> ValidateStep2{Valid?}
    ValidateStep2 -->|No| ShowErrors2[Show Validation Errors]
    ShowErrors2 --> Step2
    ValidateStep2 -->|Yes| Step3[Step 3: Upload Documents]

    Step3 --> UploadResume[Upload Resume/CV]
    UploadResume --> ValidateUpload{File Valid?}
    ValidateUpload -->|No| ShowUploadError[Show: Invalid File Type/Size]
    ShowUploadError --> Step3
    ValidateUpload -->|Yes| Step4[Step 4: Review & Submit]

    Step4 --> ReviewData[Review All Information]
    ReviewData --> EditNeeded{Need to Edit?}
    EditNeeded -->|Yes| SelectSection{Which Section?}
    SelectSection -->|Personal| Step1
    SelectSection -->|Professional| Step2
    SelectSection -->|Documents| Step3

    EditNeeded -->|No| Submit[Click Submit Application]
    Submit --> ProcessSubmit[Process Submission]

    ProcessSubmit --> CreateCandidate[Create Candidate Record]
    CreateCandidate --> StoreFiles[Store Files Permanently]
    StoreFiles --> GenerateID[Generate Application ID]
    GenerateID --> QueueProfile[Queue Profile Generation]
    QueueProfile --> SendEmail[Send Confirmation Email]

    SendEmail --> Success[Show Confirmation Page]
    Success --> End([Application Complete])

    style Start fill:#e1f5e1
    style End fill:#e1f5e1
    style Success fill:#90EE90
    style ShowClosed fill:#FFB6C1
    style ShowErrors1 fill:#FFB6C1
    style ShowErrors2 fill:#FFB6C1
    style ShowUploadError fill:#FFB6C1
```

### 1.2 Document Upload Process (Detailed)

```mermaid
flowchart TD
    Start([User on Step 3]) --> ClickUpload[Click Upload Button]
    ClickUpload --> SelectFile[Select File from System]

    SelectFile --> CheckExt{Check File Extension}
    CheckExt -->|Invalid| ShowExtError[Error: Only PDF, DOC, DOCX]
    ShowExtError --> ClickUpload

    CheckExt -->|Valid| CheckSize{Check File Size}
    CheckSize -->|> 10MB| ShowSizeError[Error: File Too Large]
    ShowSizeError --> ClickUpload

    CheckSize -->|<= 10MB| Phase{Which Phase?}

    Phase -->|Phase 0| UploadSimple[Simple POST Upload]
    UploadSimple --> StoreS3[Store in S3/Storage]
    StoreS3 --> ShowSuccess[Show Success Message]

    Phase -->|Phase 1| UploadAdvanced[Upload with Progress]
    UploadAdvanced --> VirusScan[Virus Scan]
    VirusScan --> ScanResult{Clean?}
    ScanResult -->|No| ShowVirusError[Error: Security Concern]
    ShowVirusError --> ClickUpload
    ScanResult -->|Yes| StoreS3

    ShowSuccess --> CheckCount{Files < 5?}
    CheckCount -->|Yes| MoreFiles{Upload More?}
    MoreFiles -->|Yes| ClickUpload
    MoreFiles -->|No| EnableNext[Enable Next Button]

    CheckCount -->|No| DisableUpload[Disable Upload Button]
    DisableUpload --> EnableNext

    EnableNext --> End([Proceed to Step 4])

    style Start fill:#e1f5e1
    style End fill:#e1f5e1
    style ShowSuccess fill:#90EE90
    style ShowExtError fill:#FFB6C1
    style ShowSizeError fill:#FFB6C1
    style ShowVirusError fill:#FFB6C1
```

### 1.3 Form Validation Flow (Phase 0 vs Phase 1)

```mermaid
flowchart TD
    Start([User Fills Form Field]) --> Phase{Which Phase?}

    Phase -->|Phase 0| OnSubmit[Wait for Form Submit]
    OnSubmit --> ValidateAll[Validate All Fields]
    ValidateAll --> AllValid{All Valid?}
    AllValid -->|No| ShowAllErrors[Show All Inline Errors]
    ShowAllErrors --> UserFixes[User Fixes Errors]
    UserFixes --> OnSubmit
    AllValid -->|Yes| Proceed[Proceed to Next Step]

    Phase -->|Phase 1| OnBlur[Validate on Field Blur]
    OnBlur --> CheckField{Field Valid?}
    CheckField -->|No| ShowFieldError[Show Inline Error]
    ShowFieldError --> UserFixesField[User Fixes Field]
    UserFixesField --> OnBlur
    CheckField -->|Yes| AutoSave[Auto-save to localStorage]
    AutoSave --> NextField{More Fields?}
    NextField -->|Yes| Start
    NextField -->|No| AllFieldsValid[All Fields Valid]
    AllFieldsValid --> Proceed

    Proceed --> End([Next Step])

    style Start fill:#e1f5e1
    style End fill:#e1f5e1
    style Proceed fill:#90EE90
    style ShowAllErrors fill:#FFB6C1
    style ShowFieldError fill:#FFB6C1
```

---

## 2. Agency Review Flow

### 2.1 Complete Agency Review Journey

```mermaid
flowchart TD
    Start([Agency Staff Arrives]) --> Login[Enter Credentials]
    Login --> Authenticate{Valid Credentials?}
    Authenticate -->|No| ShowLoginError[Show: Invalid Email/Password]
    ShowLoginError --> Login

    Authenticate -->|Yes| CheckActive{Account Active?}
    CheckActive -->|No| ShowInactive[Show: Account Inactive]
    ShowInactive --> ContactAdmin[Contact Administrator]

    CheckActive -->|Yes| Dashboard[Load Dashboard]
    Dashboard --> ViewMetrics[View Quick Stats]

    ViewMetrics --> Action{Choose Action}
    Action -->|View Applications| AppList[Go to Applications List]
    Action -->|Post Job| JobCreate[Go to Job Creation]
    Action -->|Search Candidates| CandidatePool[Go to Candidate Pool]

    AppList --> LoadApps[Load New Applications]
    LoadApps --> FilterApps{Apply Filters?}
    FilterApps -->|Yes| FilterBy[Filter by Job/Date/Skills]
    FilterBy --> ShowFiltered[Show Filtered List]
    FilterApps -->|No| ShowAll[Show All New Applications]
    ShowFiltered --> SelectApp
    ShowAll --> SelectApp[Click on Application]

    SelectApp --> ViewProfile[View Candidate Profile]
    ViewProfile --> ReviewInfo[Review Overview Tab]
    ReviewInfo --> ViewDocs[View Documents Tab]
    ViewDocs --> Decision{Decision?}

    Decision -->|Download Profile| GenProfile[Generate One-Page Profile]
    GenProfile --> DownloadPDF[Download PDF]
    DownloadPDF --> Decision

    Decision -->|Change Status| SelectStatus[Select New Status]
    SelectStatus --> UpdateDB[Update Database]
    UpdateDB --> LogActivity[Log Status Change]
    LogActivity --> ShowSuccess[Show: Status Updated]

    Decision -->|Add Note| WriteNote[Write Internal Note]
    WriteNote --> SaveNote[Save to Database]
    SaveNote --> ShowNoteSaved[Show: Note Saved]

    Decision -->|Export| CheckCount{<= 10 Candidates?}
    CheckCount -->|No| ShowMaxError[Error: Max 10 in Phase 0]
    CheckCount -->|Yes| ExportProfiles[Generate ZIP with PDFs]
    ExportProfiles --> DownloadZip[Auto-Download ZIP]

    ShowSuccess --> NextCandidate{Review Next?}
    ShowNoteSaved --> NextCandidate
    DownloadZip --> NextCandidate
    DownloadPDF --> NextCandidate

    NextCandidate -->|Yes| AppList
    NextCandidate -->|No| End([End Session])

    style Start fill:#e1f5e1
    style End fill:#e1f5e1
    style ShowSuccess fill:#90EE90
    style ShowNoteSaved fill:#90EE90
    style DownloadZip fill:#90EE90
    style ShowLoginError fill:#FFB6C1
    style ShowInactive fill:#FFB6C1
    style ShowMaxError fill:#FFB6C1
```

### 2.2 Status Change Workflow (Phase 0 vs Phase 1)

```mermaid
flowchart TD
    Start([Click Status Dropdown]) --> SelectStatus[Select New Status]
    SelectStatus --> CheckStatus{Which Status?}

    CheckStatus -->|Rejected| Phase{Which Phase?}
    Phase -->|Phase 0| UpdateSimple[Update Status to Rejected]
    UpdateSimple --> LogChange[Log Status Change]
    LogChange --> ShowToast[Show Success Toast]

    Phase -->|Phase 1| ShowModal[Show Rejection Modal]
    ShowModal --> SelectReason[Select Rejection Reason]
    SelectReason --> PoolOption{Add to Pool?}
    PoolOption -->|Yes| ChangeToPool[Status: In Pool]
    PoolOption -->|No| UpdateRejected[Status: Rejected]
    ChangeToPool --> EmailOpt
    UpdateRejected --> EmailOpt{Send Email?}
    EmailOpt -->|Yes| SendRejectEmail[Send Rejection Email]
    EmailOpt -->|No| LogChange
    SendRejectEmail --> LogChange

    CheckStatus -->|Shortlisted| UpdateShortlist[Update Status]
    UpdateShortlist --> PhaseCheck{Which Phase?}
    PhaseCheck -->|Phase 0| LogChange
    PhaseCheck -->|Phase 1| ShowSchedule[Show Schedule Interview Action]
    ShowSchedule --> LogChange

    CheckStatus -->|Hired| CheckPerm{Has Permission?}
    CheckPerm -->|No| ShowPermError[Error: Insufficient Permissions]
    CheckPerm -->|Yes| UpdateHired[Update Status to Hired]
    UpdateHired --> SendCongrats[Send Congratulations Email]
    SendCongrats --> LogChange

    CheckStatus -->|Other Status| UpdateOther[Update Status]
    UpdateOther --> LogChange

    ShowToast --> End([Status Updated])

    style Start fill:#e1f5e1
    style End fill:#90EE90
    style ShowToast fill:#90EE90
    style ShowPermError fill:#FFB6C1
```

### 2.3 Profile Generation Process

```mermaid
flowchart TD
    Start([Click Download Profile]) --> CheckExists{Profile PDF Exists?}

    CheckExists -->|Yes| DownloadExisting[Download from Database]
    DownloadExisting --> End([PDF Downloaded])

    CheckExists -->|No| Phase{Which Phase?}

    Phase -->|Phase 0| ShowGenerating[Show: Generating Profile...]
    ShowGenerating --> SyncGen[Synchronous Generation 2-3s]
    SyncGen --> FetchData[Fetch Candidate Data]
    FetchData --> ApplyTemplate[Apply Basic Template]
    ApplyTemplate --> GeneratePDF[Generate PDF]
    GeneratePDF --> SaveDB[Save to Database]
    SaveDB --> DownloadNew[Download PDF]
    DownloadNew --> End

    Phase -->|Phase 1| QueueJob[Queue Background Job]
    QueueJob --> ShowProgress[Show Progress Indicator]
    ShowProgress --> AsyncGen[Async Generation]
    AsyncGen --> FetchDataBg[Fetch Candidate Data]
    FetchDataBg --> ApplyCustomTemplate[Apply Custom Template]
    ApplyCustomTemplate --> AddBranding[Add Agency Branding]
    AddBranding --> GeneratePDFBg[Generate PDF]
    GeneratePDFBg --> SaveDBBg[Save to Database]
    SaveDBBg --> RetryCheck{Generation Success?}
    RetryCheck -->|No| RetryGen[Retry Generation]
    RetryGen --> RetryCount{Retry < 3?}
    RetryCount -->|Yes| AsyncGen
    RetryCount -->|No| NotifyAdmin[Notify Admin]
    RetryCheck -->|Yes| NotifyUser[Notify User: Ready]
    NotifyUser --> DownloadAsync[Download PDF]
    DownloadAsync --> End

    style Start fill:#e1f5e1
    style End fill:#90EE90
    style DownloadNew fill:#90EE90
    style DownloadAsync fill:#90EE90
```

### 2.4 Export Candidates Flow (Phase 0)

```mermaid
flowchart TD
    Start([Select Multiple Candidates]) --> ClickExport[Click Export Profiles]
    ClickExport --> CheckCount{Count <= 10?}

    CheckCount -->|No| ShowError[Error: Maximum 10 Candidates]
    ShowError --> DeselectSome[Deselect Candidates]
    DeselectSome --> Start

    CheckCount -->|Yes| CheckPerm{Has Permission?}
    CheckPerm -->|No| ShowPermError[Error: No Export Permission]
    ShowPermError --> End([Export Cancelled])

    CheckPerm -->|Yes| ShowLoading[Show: Generating Export...]
    ShowLoading --> Loop[For Each Candidate]

    Loop --> FetchProfile[Retrieve Profile PDF]
    FetchProfile --> ProfileExists{PDF Exists?}
    ProfileExists -->|No| SkipCandidate[Skip Candidate]
    SkipCandidate --> MoreCandidates

    ProfileExists -->|Yes| AddToZip[Add to ZIP File]
    AddToZip --> NameFile[Name: CandidateName_Profile.pdf]
    NameFile --> MoreCandidates{More Candidates?}

    MoreCandidates -->|Yes| Loop
    MoreCandidates -->|No| CreateZip[Create ZIP File]

    CreateZip --> NameZip[Name: Candidate_Export_Date.zip]
    NameZip --> AutoDownload[Auto-Download ZIP]
    AutoDownload --> ShowSuccess[Show: Export Complete]

    ShowSuccess --> EndSuccess([Export Successful])

    style Start fill:#e1f5e1
    style EndSuccess fill:#90EE90
    style ShowSuccess fill:#90EE90
    style ShowError fill:#FFB6C1
    style ShowPermError fill:#FFB6C1
    style End fill:#FFB6C1
```

---

## 3. Job Posting Management Flow

### 3.1 Complete Job Posting Creation Flow

```mermaid
flowchart TD
    Start([Click Post New Job]) --> LoadForm[Load Job Creation Form]
    LoadForm --> FillBasic[Fill Basic Information]
    FillBasic --> FillReq[Fill Requirements]
    FillReq --> FillDetails[Fill Job Details]
    FillDetails --> FillSettings[Fill Application Settings]

    FillSettings --> Validate{All Required Fields?}
    Validate -->|No| ShowErrors[Highlight Missing Fields]
    ShowErrors --> FixErrors[User Fixes Errors]
    FixErrors --> Validate

    Validate -->|Yes| CheckDate{Valid Deadline?}
    CheckDate -->|< 7 Days| ShowDateError[Error: Min 7 Days]
    ShowDateError --> FillSettings

    CheckDate -->|>= 7 Days| CheckSalary{Salary Range Valid?}
    CheckSalary -->|No| ShowSalaryError[Error: Max > Min]
    ShowSalaryError --> FillDetails

    CheckSalary -->|Yes| Action{User Action?}
    Action -->|Save Draft Phase 1| SaveDraft[Save as Draft Status]
    SaveDraft --> ShowDraftSaved[Show: Draft Saved]

    Action -->|Publish| ConfirmPublish{Phase 1 Confirmation?}
    ConfirmPublish -->|Yes Show Preview| PreviewJob[Show Job Preview]
    PreviewJob --> ConfirmOK{Looks Good?}
    ConfirmOK -->|No| EditJob[Edit Job]
    EditJob --> FillBasic

    ConfirmPublish -->|No Direct Publish| PublishJob
    ConfirmOK -->|Yes| PublishJob[Publish Job Posting]

    PublishJob --> GenerateID[Generate Job ID]
    GenerateID --> CreateRecord[Create Database Record]
    CreateRecord --> SetActive[Set Status: Active]
    SetActive --> IndexSearch[Index for Search]
    IndexSearch --> UpdateMetrics[Update Dashboard Metrics]
    UpdateMetrics --> ShowSuccess[Show: Job Published]

    ShowSuccess --> Options{Next Action?}
    Options -->|View Public| OpenPublic[Open Public Listing]
    Options -->|Share Link| CopyLink[Copy Job URL]
    Options -->|Post Another| Start
    Options -->|Dashboard| Dashboard[Return to Dashboard]

    OpenPublic --> End([Job Posted])
    CopyLink --> End
    Dashboard --> End
    ShowDraftSaved --> End

    style Start fill:#e1f5e1
    style End fill:#90EE90
    style ShowSuccess fill:#90EE90
    style ShowDraftSaved fill:#90EE90
    style ShowErrors fill:#FFB6C1
    style ShowDateError fill:#FFB6C1
    style ShowSalaryError fill:#FFB6C1
```

### 3.2 Job Edit Flow

```mermaid
flowchart TD
    Start([Select Job to Edit]) --> CheckStatus{Job Status?}

    CheckStatus -->|Archived| ShowArchived[Show: Must Un-archive First]
    ShowArchived --> End([Cannot Edit])

    CheckStatus -->|Active/Closed| LoadForm[Load Edit Form]
    LoadForm --> PreFill[Pre-fill with Current Data]
    PreFill --> UserEdits[User Makes Changes]

    UserEdits --> WhatChanged{What Changed?}

    WhatChanged -->|Deadline Extended| WarnReopen[Warn: Will Reopen Applications]
    WarnReopen --> ConfirmReopen{Confirm?}
    ConfirmReopen -->|No| UserEdits
    ConfirmReopen -->|Yes| ReactivateJob[Change Status to Active]
    ReactivateJob --> SaveChanges

    WhatChanged -->|Deadline Shortened| CheckPast{New Date in Past?}
    CheckPast -->|Yes| ShowError[Error: Cannot Shorten to Past]
    ShowError --> UserEdits
    CheckPast -->|No| SaveChanges[Save Changes]

    WhatChanged -->|Other Fields| SaveChanges

    SaveChanges --> UpdateDB[Update Database]
    UpdateDB --> UpdateSearch[Update Search Index]
    UpdateSearch --> LogEdit[Log Edit in Activity]

    LogEdit --> Phase{Which Phase?}
    Phase -->|Phase 1| ShowLastEdit[Show: Last Edited By User on Date]
    Phase -->|Phase 0| ShowSimple[Show: Saved]

    ShowLastEdit --> NotifyRecruiter
    ShowSimple --> NotifyRecruiter{Assigned Recruiter?}
    NotifyRecruiter -->|Yes| SendNotification[Send Notification]
    NotifyRecruiter -->|No| ShowSuccess
    SendNotification --> ShowSuccess[Show: Job Updated]

    ShowSuccess --> EndSuccess([Edit Complete])

    style Start fill:#e1f5e1
    style EndSuccess fill:#90EE90
    style ShowSuccess fill:#90EE90
    style End fill:#FFB6C1
    style ShowArchived fill:#FFB6C1
    style ShowError fill:#FFB6C1
```

### 3.3 Job Archive Flow (Manual & Auto)

```mermaid
flowchart TD
    Start([Archive Trigger]) --> TriggerType{Trigger Type?}

    TriggerType -->|Manual| ClickArchive[Staff Clicks Archive]
    ClickArchive --> ShowModal[Show Confirmation Modal]
    ShowModal --> SelectReason[Select Reason for Closing]
    SelectReason --> Options[Position Filled / No Longer Hiring / Other]
    Options --> ConfirmArchive[Click: Archive and Stop Applications]

    TriggerType -->|Automatic| CronJob[Daily Cron Job Midnight]
    CronJob --> CheckJobs[Check All Active Jobs]
    CheckJobs --> MatchDeadline{Deadline = Today?}
    MatchDeadline -->|No| SkipJob[Skip Job]
    MatchDeadline -->|Yes| AutoClose[Auto-close Job]

    ConfirmArchive --> ValidatePerm{Has Permission?}
    ValidatePerm -->|No| ShowPermError[Error: No Permission]
    ShowPermError --> EndCancel([Archive Cancelled])

    ValidatePerm -->|Yes| ProcessArchive[Process Archive]
    AutoClose --> ProcessArchive

    ProcessArchive --> UpdateStatus[Update Status to Archived/Closed]
    UpdateStatus --> RemovePublic[Remove from Public Listings]
    RemovePublic --> SetDate[Set Archived/Closed Date]
    SetDate --> LogReason[Log Archive Reason]
    LogReason --> MoveApps[Move New Apps to Under Review]

    MoveApps --> NotifyType{Trigger Type?}
    NotifyType -->|Manual| NotifyManual[Notify Assigned Recruiter]
    NotifyType -->|Auto| NotifyAuto[Send: Job Deadline Reached X Applications]

    NotifyManual --> ShowManualSuccess[Show: Job Archived]
    NotifyAuto --> DisableButton[Disable Apply Button on Public Page]

    ShowManualSuccess --> EndSuccess([Archive Complete])
    DisableButton --> EndSuccess

    SkipJob --> MoreJobs{More Jobs?}
    MoreJobs -->|Yes| CheckJobs
    MoreJobs -->|No| EndCron([Cron Complete])

    style Start fill:#e1f5e1
    style EndSuccess fill:#90EE90
    style EndCron fill:#90EE90
    style ShowManualSuccess fill:#90EE90
    style EndCancel fill:#FFB6C1
    style ShowPermError fill:#FFB6C1
```

### 3.4 Job Sharing Flow (Phase 1)

```mermaid
flowchart TD
    Start([Click Share Link]) --> ShowModal[Show Share Modal]
    ShowModal --> DisplayURL[Display Public Job URL]

    DisplayURL --> UserAction{Choose Action}

    UserAction -->|Copy URL| CopyClipboard[Copy to Clipboard]
    CopyClipboard --> ShowCopied[Show: URL Copied]
    ShowCopied --> End([Sharing Complete])

    UserAction -->|Email| OpenEmailClient[Open Default Email Client]
    OpenEmailClient --> PreFillTemplate[Pre-fill Email Template]
    PreFillTemplate --> UserSendsEmail[User Sends Email]
    UserSendsEmail --> End

    UserAction -->|QR Code| GenerateQR[Generate QR Code]
    GenerateQR --> ShowQR[Display QR Code]
    ShowQR --> DownloadQR{Download?}
    DownloadQR -->|Yes| SavePNG[Save as PNG]
    SavePNG --> End
    DownloadQR -->|No| End

    UserAction -->|LinkedIn Phase 2| ShareLinkedIn[Share to Company Page]
    ShareLinkedIn --> End

    UserAction -->|Embed Phase 2| ShowEmbed[Show Embed Code]
    ShowEmbed --> CopyEmbed[Copy HTML/iFrame]
    CopyEmbed --> End

    style Start fill:#e1f5e1
    style End fill:#90EE90
    style ShowCopied fill:#90EE90
```

---

## 4. Alternative Paths & Error Scenarios

### 4.1 Error Recovery Flow (Application)

```mermaid
flowchart TD
    Start([Error Occurs]) --> ErrorType{Error Type?}

    ErrorType -->|Network Timeout| Phase{Which Phase?}
    Phase -->|Phase 0| ShowSimpleError[Show: Connection Lost]
    ShowSimpleError --> UserRetry[User Clicks Retry]
    UserRetry --> Retry[Retry Request]

    Phase -->|Phase 1| CheckSaved{Data Saved?}
    CheckSaved -->|Yes| ShowSavedMsg[Show: Data Saved, Retry?]
    CheckSaved -->|No| DataLost[Data Lost]
    ShowSavedMsg --> UserRetry

    ErrorType -->|Validation Error| ShowInline[Show Inline Errors]
    ShowInline --> HighlightFields[Highlight Invalid Fields]
    HighlightFields --> UserFixes[User Fixes Errors]
    UserFixes --> Revalidate[Revalidate]
    Revalidate --> ValidNow{Valid Now?}
    ValidNow -->|No| ShowInline
    ValidNow -->|Yes| Success[Proceed]

    ErrorType -->|File Upload Failed| ShowUploadError[Show: Upload Failed]
    ShowUploadError --> RetryUpload{Retry?}
    RetryUpload -->|Yes| ReuploadFile[Re-upload File]
    RetryUpload -->|No| ChooseDifferent[Choose Different File]
    ReuploadFile --> UploadSuccess{Success?}
    UploadSuccess -->|No| ShowUploadError
    UploadSuccess -->|Yes| Success
    ChooseDifferent --> Success

    ErrorType -->|Session Expired| ShowExpired[Show: Session Expired]
    ShowExpired --> P1Check{Phase 1?}
    P1Check -->|Yes| RecoverData[Recover from localStorage]
    RecoverData --> RestoreSession[Restore Session]
    P1Check -->|No| LoginAgain[User Must Login Again]
    RestoreSession --> Success
    LoginAgain --> DataLost

    Retry --> Success
    Success --> End([Error Resolved])
    DataLost --> EndFail([Error Not Recovered])

    style Start fill:#FFE4B5
    style End fill:#90EE90
    style EndFail fill:#FFB6C1
    style Success fill:#90EE90
```

### 4.2 Duplicate Application Prevention

```mermaid
flowchart TD
    Start([User Submits Application]) --> CheckDupe{Check Email + JobID}

    CheckDupe -->|No Match| CreateNew[Create New Application]
    CreateNew --> Success[Application Submitted]
    Success --> End([Complete])

    CheckDupe -->|Match Found| ShowDupeMsg[Show: Already Applied on Date]
    ShowDupeMsg --> Options[Show Options]

    Options --> UserChoice{User Chooses?}
    UserChoice -->|View Status| ShowStatus[Display Application Status]
    ShowStatus --> EndView([View Status])

    UserChoice -->|Update Phase 1| AllowUpdate[Allow Profile Update]
    AllowUpdate --> UpdateApp[Update Application]
    UpdateApp --> EndUpdate([Application Updated])

    UserChoice -->|Apply Different| BackToJobs[Return to Job Listings]
    BackToJobs --> EndBack([Browse Other Jobs])

    style Start fill:#e1f5e1
    style End fill:#90EE90
    style EndView fill:#90EE90
    style EndUpdate fill:#90EE90
    style EndBack fill:#90EE90
```

---

## 4. Compliance & Workforce Management Flows (Phase 1 & 2)

### 4.1 Visa Expiry Monitoring Flow

```mermaid
flowchart TD
    Start([Staff Accesses Compliance Dashboard]) --> LoadDash[Load Visa Dashboard]
    LoadDash --> QueryDB[Query All Workers with Visas]

    QueryDB --> Calculate[Calculate Days Until Expiry]
    Calculate --> ApplyColor[Apply Color Coding]

    ApplyColor --> DisplayCards[Display Summary Cards]
    DisplayCards --> ShowCounts{Show Counts}

    ShowCounts --> Red[Red: < 30 Days]
    ShowCounts --> Amber[Amber: 30-60 Days]
    ShowCounts --> Yellow[Yellow: 60-90 Days]
    ShowCounts --> Green[Green: > 90 Days]

    Red --> WorkersList[Display Workers Table]
    Amber --> WorkersList
    Yellow --> WorkersList
    Green --> WorkersList

    WorkersList --> UserAction{User Action?}

    UserAction --> Filter[Apply Filters]
    Filter --> UpdateList[Update Worker List]
    UpdateList --> WorkersList

    UserAction --> ViewWorker[Click on Worker]
    ViewWorker --> WorkerProfile[Open Worker Profile]
    WorkerProfile --> ViewDocs[View Compliance Documents]
    ViewDocs --> CheckVisa{Visa Status?}

    CheckVisa --> Expiring[Expiring Soon]
    Expiring --> SendReminder[Send Renewal Reminder]
    SendReminder --> LogAction[Log Reminder Sent]

    CheckVisa --> Valid[Valid - No Action]
    Valid --> End([Dashboard Updated])

    CheckVisa --> Expired[Already Expired]
    Expired --> AlertOfficer[Alert Compliance Officer]
    AlertOfficer --> UrgentAction[Mark for Urgent Action]
    UrgentAction --> End

    UserAction --> ExportReport[Export Expiry Report]
    ExportReport --> GenerateCSV[Generate CSV/Excel]
    GenerateCSV --> Download[Auto-Download Report]
    Download --> End

    LogAction --> End

    style Start fill:#e1f5e1
    style End fill:#90EE90
    style Red fill:#FFB6C1
    style Amber fill:#FFE4B5
    style Yellow fill:#FFFACD
    style Green fill:#90EE90
    style Expired fill:#FF0000,color:#FFF
    style AlertOfficer fill:#FFB6C1
```

### 4.2 Automated Reminder System Flow (Phase 2)

```mermaid
flowchart TD
    Start([Daily Cron Job 9 AM]) --> QueryVisas[Query All Visa Expiry Dates]
    QueryVisas --> CheckThresholds{Check Against Thresholds}

    CheckThresholds --> Check90[90 Days Until Expiry?]
    Check90 --> |Yes| Generate90[Generate 90-Day Reminder]
    Generate90 --> Send90[Send Email to Staff]
    Send90 --> Log90[Log Reminder]

    CheckThresholds --> Check60[60 Days Until Expiry?]
    Check60 --> |Yes| Generate60[Generate 60-Day Reminder]
    Generate60 --> Send60[Send Email + Dashboard Alert]
    Send60 --> Log60[Log Reminder]

    CheckThresholds --> Check30[30 Days Until Expiry?]
    Check30 --> |Yes| Generate30[Generate 30-Day URGENT Reminder]
    Generate30 --> Send30[Email + SMS + Dashboard Alert]
    Send30 --> Log30[Log Reminder]

    CheckThresholds --> Check14[14 Days Until Expiry?]
    Check14 --> |Yes| GenerateDaily[Generate Daily Reminder]
    GenerateDaily --> SendDaily[Daily Emails Until Resolved]
    SendDaily --> Escalate[Escalate to Manager]
    Escalate --> LogDaily[Log Escalation]

    CheckThresholds --> CheckExpired[Expiry Date Reached?]
    CheckExpired --> |Yes| MarkExpired[Mark Visa as Expired]
    MarkExpired --> AlertCompliance[Alert Compliance Officer]
    AlertCompliance --> CreateTask[Create Urgent Action Task]
    CreateTask --> LogExpired[Log Expiry Event]

    Check90 --> |No| NextWorker
    Check60 --> |No| NextWorker
    Check30 --> |No| NextWorker
    Check14 --> |No| NextWorker
    CheckExpired --> |No| NextWorker

    NextWorker{More Workers?} --> |Yes| QueryVisas
    NextWorker --> |No| UpdateDash[Update Dashboard Counts]

    Log90 --> UpdateDash
    Log60 --> UpdateDash
    Log30 --> UpdateDash
    LogDaily --> UpdateDash
    LogExpired --> UpdateDash

    UpdateDash --> End([Reminders Complete])

    style Start fill:#e1f5e1
    style End fill:#90EE90
    style Generate30 fill:#FFB6C1
    style GenerateDaily fill:#FF0000,color:#FFF
    style MarkExpired fill:#FF0000,color:#FFF
    style AlertCompliance fill:#FFB6C1
```

### 4.3 Right-to-Work Document Upload Flow (Phase 1 & 2)

```mermaid
flowchart TD
    Start([Staff Opens Employee Profile]) --> NavDocs[Navigate to Documents Tab]
    NavDocs --> ViewExisting[View Existing Documents]

    ViewExisting --> Action{User Action?}

    Action --> ClickUpload[Click Upload Document]
    ClickUpload --> SelectCategory[Select Document Category]

    SelectCategory --> CategoryType{Category Type?}
    CategoryType --> RTW[Right-to-Work Evidence]
    CategoryType --> DBS[DBS Certificate]
    CategoryType --> Contract[Employment Contract]
    CategoryType --> Training[Training Certificate]

    RTW --> SelectRTWType{Document Type?}
    SelectRTWType --> Passport[Passport]
    SelectRTWType --> BRP[BRP Card]
    SelectRTWType --> ShareCode[Share Code Verification]

    Passport --> ChooseFile[Choose File from System]
    BRP --> ChooseFile

    ShareCode --> Phase2Check{Phase 2?}
    Phase2Check --> |Yes| EnterCode[Enter Share Code]
    EnterCode --> VerifyAPI[Connect to UKVI API]
    VerifyAPI --> APIResult{Verification Result?}
    APIResult --> |Success| AutoFill[Auto-fill Visa Details]
    APIResult --> |Failed| ManualEntry[Manual Entry Required]
    AutoFill --> SaveVerification
    ManualEntry --> SaveVerification[Save Verification Record]

    Phase2Check --> |No| ChooseFile

    DBS --> ChooseFile
    Contract --> ChooseFile
    Training --> ChooseFile

    ChooseFile --> ValidateFile{File Valid?}
    ValidateFile --> |No| ShowError[Error: Invalid File Type/Size]
    ShowError --> ClickUpload

    ValidateFile --> |Yes| AddDetails[Add Document Details Form]
    AddDetails --> FillDetails[Fill in Details]

    FillDetails --> DetailsForm[Document Details Form]
    DetailsForm --> IssueDate[Issue Date]
    DetailsForm --> ExpiryDate[Expiry Date if applicable]
    DetailsForm --> RefNumber[Reference Number]
    DetailsForm --> Notes[Notes optional]

    IssueDate --> ClickSave[Click Upload and Save]
    ClickSave --> ProcessUpload[Process Upload]

    ProcessUpload --> EncryptStore[Encrypt and Store Securely]
    EncryptStore --> CreateRecord[Create Database Record]
    CreateRecord --> Timestamp[Add Timestamp]
    Timestamp --> LinkProfile[Link to Employee Profile]
    LinkProfile --> AuditTrail[Add to Audit Trail]

    AuditTrail --> HasExpiry{Has Expiry Date?}
    HasExpiry --> |Yes| AddToReminder[Add to Reminder System]
    HasExpiry --> |No| SkipReminder[Skip Reminder]

    AddToReminder --> GenerateChecklist
    SkipReminder --> GenerateChecklist[Generate Document Verification Checklist]
    SaveVerification --> GenerateChecklist

    GenerateChecklist --> ShowSuccess[Show: Document Uploaded Successfully]
    ShowSuccess --> UpdateDocsList[Update Documents List]
    UpdateDocsList --> End([Upload Complete])

    Action --> ViewDoc[View Existing Document]
    ViewDoc --> Download[Download Document]
    Download --> End

    Action --> CheckVersion[Check Version History]
    CheckVersion --> ShowVersions[Display Document Versions]
    ShowVersions --> End

    style Start fill:#e1f5e1
    style End fill:#90EE90
    style ShowSuccess fill:#90EE90
    style ShowError fill:#FFB6C1
    style VerifyAPI fill:#ADD8E6
    style AutoFill fill:#90EE90
```

### 4.4 Reportable Events Tracking Flow (Phase 2)

```mermaid
flowchart TD
    Start([System Monitors Employee Records]) --> DetectChange{Change Detected?}

    DetectChange --> Absence[Employee Absent 10+ Days]
    DetectChange --> RoleSalary[Role/Salary Change]
    DetectChange --> Termination[Employment Terminated]
    DetectChange --> Location[Work Location Changed]

    Absence --> TriggerAlert[Trigger Reportable Event Alert]
    RoleSalary --> TriggerAlert
    Termination --> TriggerAlert
    Location --> TriggerAlert

    TriggerAlert --> CreateDraft[Create Draft UKVI Report]
    CreateDraft --> NotifyOfficer[Notify Compliance Officer]
    NotifyOfficer --> DashAlert[Add Alert to Dashboard]

    DashAlert --> StaffReview[Staff Reviews Event]
    StaffReview --> ConfirmAccuracy{Accurate?}

    ConfirmAccuracy --> |No| EditDetails[Edit Event Details]
    EditDetails --> StaffReview

    ConfirmAccuracy --> |Yes| AddContext[Add Additional Context/Notes]
    AddContext --> GenerateTemplate[Generate UKVI Report Template]

    GenerateTemplate --> PreFill[Pre-fill Template with Data]
    PreFill --> WorkerDetails[Worker Details: Name, CoS, Nationality]
    WorkerDetails --> EventDetails[Event Type and Date]
    EventDetails --> StatusInfo[Current Status]
    StatusInfo --> SupportingInfo[Supporting Information]

    SupportingInfo --> StaffReviewTemplate[Staff Reviews Template]
    StaffReviewTemplate --> NeedEdit{Needs Editing?}

    NeedEdit --> |Yes| EditTemplate[Edit Template Content]
    EditTemplate --> StaffReviewTemplate

    NeedEdit --> |No| ExportTemplate[Export for Submission]
    ExportTemplate --> DownloadPDF[Download Pre-filled PDF]
    DownloadPDF --> ManualSubmit[Staff Submits to UKVI SMS]

    ManualSubmit --> RecordSubmission[Record Submission Date in Platform]
    RecordSubmission --> MarkReported[Mark Event as Reported]

    MarkReported --> LogAudit[Create Audit Trail Entry]
    LogAudit --> LogDetails[Log All Event Details]

    LogDetails --> AuditEntries[Audit Trail Contains]
    AuditEntries --> OccurrenceDate[Event Occurrence Date]
    AuditEntries --> DetectionDate[Detection Date]
    AuditEntries --> ReportGenDate[Report Generation Date]
    AuditEntries --> SubmissionDate[Submission Date]
    AuditEntries --> HandlerName[Staff Member Who Handled]
    AuditEntries --> SupportingDocs[Supporting Documentation Links]

    OccurrenceDate --> UpdateCompliance[Update Compliance Dashboard]
    UpdateCompliance --> RemoveAlert[Remove from Active Alerts]
    RemoveAlert --> End([Event Reported])

    DetectChange --> |No Change| Continue[Continue Monitoring]
    Continue --> Start

    style Start fill:#e1f5e1
    style End fill:#90EE90
    style TriggerAlert fill:#FFB6C1
    style NotifyOfficer fill:#FFE4B5
    style MarkReported fill:#90EE90
```

---
