# Detailed User Workflows

**Document Information**
- **Version**: 1.0
- **Date**: November 10, 2025
- **Status**: Draft
- **Related Document**: Product Requirements Document (prd.md)

---

## Important: Phase 0 vs Phase 1 Philosophy

### Phase 0 (Demo MVP - 4 weeks)
**Goal**: Create a functional, demo-ready prototype for investor presentations

**Characteristics**:
- **Lean & Fast**: Prioritize speed of delivery over perfection
- **Core Functionality Only**: Essential features that demonstrate value
- **Minimal Validation**: Basic client-side checks, no complex business logic
- **Simple Error Handling**: Generic error messages, no retry mechanisms
- **No Advanced Features**: No auto-save, real-time updates, or background jobs
- **Synchronous Operations**: Direct API calls, no queues or workers
- **Basic Security**: File extension checks, no virus scanning
- **Static UI**: Manual refresh, no live updates
- **Limited Scale**: Works for demo scenarios (50 concurrent users max)

### Phase 1 (Extended MVP - 4 weeks)
**Goal**: Production-ready platform for early adopters

**Enhancements**:
- Real-time validation and auto-save
- Advanced error handling with recovery
- Background job processing
- Virus scanning and security hardening
- Real-time updates via WebSockets
- Bulk operations and advanced filters
- Email notifications
- Performance optimizations

**Important Note**: Throughout this document, features are clearly marked as Phase 0 or Phase 1. If not explicitly marked as Phase 0, assume it's Phase 1 or later.

---

## Table of Contents

1. [Candidate Application Flow](#1-candidate-application-flow)
2. [Agency Review Flow](#2-agency-review-flow)
3. [Job Posting Management Flow](#3-job-posting-management-flow)

---

## 1. Candidate Application Flow

### Overview
This workflow describes the complete journey of a candidate from discovering a job posting to successfully submitting an application, including all decision points, error scenarios, and success states.

### 1.1 Job Discovery Phase

**Step 1: Landing on Platform**
- **Entry Point**: Candidate arrives at the public job portal
- **Options Available**:
  - Browse all active job listings
  - Use search functionality
  - Apply filters (location, employment type, experience level, date posted)

**User Actions**:
- Candidate can scroll through job listings
- View job cards with summary information (title, location, type, posted date)
- Click on any job card to view full details

**System Behavior**:
- Display all active (non-archived, non-expired) job postings
- Show job count and filter indicators
- Provide responsive grid/list layout
- Load more jobs on scroll (pagination)

**Edge Cases**:
- **No jobs available**: Display "No positions currently available. Check back soon!" message
- **Slow connection**: Show loading skeleton while fetching jobs
- **Filter returns no results**: Display "No jobs match your criteria" with option to clear filters

---

### 1.2 Job Exploration Phase

**Step 2: Viewing Job Details**
- **Trigger**: Candidate clicks on a specific job posting
- **Page Displays**:
  - Full job title and company information
  - Complete job description
  - Required qualifications and experience
  - Employment details (type, location, salary range if provided)
  - Application deadline
  - List of required documents
  - Clear "Apply Now" button

**User Actions**:
- Read through job description
- Check if they meet qualifications
- Note required documents
- Decide whether to apply
- Options:
  - Click "Apply Now" → Proceed to application
  - Click "Back to Jobs" → Return to job listings

**System Behavior**:
- Track job view analytics (for agency reporting)
- Ensure job is still active and accepting applications
- Display warning if application deadline is approaching (within 3 days)

**Edge Cases**:
- **Job closed mid-viewing**: Show modal "This position is no longer accepting applications"
- **Application deadline passed**: Disable "Apply Now" button, show "Application period has ended"
- **Required documents unclear**: Provide tooltips or help text

---

### 1.3 Application Initiation Phase

**Step 3: Starting the Application**
- **Trigger**: Candidate clicks "Apply Now"
- **System Action**:
  - Navigate to multi-step application form
  - Display progress indicator (Step 1 of 4, for example)
  - Show job title at top as context
  - Provide "Save Draft" option (Phase 1 feature)

**Form Structure Overview**:
- **Step 1**: Personal Information
- **Step 2**: Professional Information
- **Step 3**: Document Upload
- **Step 4**: Review & Submit

**User Actions**:
- Begin filling out Step 1
- Can navigate back to job details if needed
- Can save draft and return later (Phase 1)

---

### 1.4 Personal Information Collection (Step 1)

**Step 4: Collecting Personal Details**

**Required Fields**:
- Full name (First name, Last name)
- Email address
- Phone number (with country code selector)
- Current location (City, Country)

**Optional Fields**:
- LinkedIn profile URL

**User Actions**:
- Fill in text fields
- Select country code from dropdown
- Click "Next" to proceed to Step 2
- Click "Back" to return to job details (with confirmation if form has data)

**System Behavior - Validation**:

**Phase 0 (Demo MVP) - Basic Validation**:
- **Email validation**:
  - Simple format check using HTML5 input type="email"
  - Basic regex pattern (contains @ and .)
  - Show error on form submit if invalid

- **Phone validation**:
  - Accept any format (no strict validation)
  - Minimum 7 characters
  - No automatic formatting in Phase 0

- **Name validation**:
  - Minimum 2 characters
  - No special character restrictions in Phase 0

**Phase 1 (Extended MVP) - Enhanced Validation**:
- Real-time validation on field blur
- Check if email already exists (suggest profile recovery)
- International phone format validation
- Automatic phone number formatting
- Unicode character support for international names
- Network timeout handling with data recovery

**Error Scenarios (Phase 0)**:
- **Empty required field**: Show "This field is required" on submit
- **Invalid email format**: Show "Please enter a valid email"
- **Form submission with errors**: Show inline error messages

**Phase 1 Additional Errors**:
- Real-time field-by-field validation
- Network timeout recovery
- Scroll to first error functionality

**Success State**:
- All validations pass
- "Next" button enabled
- Smooth transition to Step 2

---

### 1.5 Professional Information Collection (Step 2)

**Step 5: Collecting Professional Details**

**Required Fields**:
- Current job title / Most recent position
- Years of professional experience (dropdown: 0-1, 1-3, 3-5, 5-10, 10+ years)
- Education level (dropdown: High School, Associate, Bachelor's, Master's, PhD, Other)
- Institution name
- Key skills (tag input - add up to 10 skills)

**Optional Fields**:
- Professional certifications/licenses (text area)
- Areas of expertise/specialization (text area)

**Work History Section** (Phase 0: Simplified / Phase 1: Detailed):

**Phase 0 (Demo MVP)**:
- Most recent position title
- Company name
- Employment duration (from month/year to month/year or "Present")
- Brief description (200 character limit)

**Phase 1 (Extended MVP)**:
- Add multiple previous positions (up to 5)
- Same fields as Phase 0 for each position
- Drag to reorder
- Delete positions

**User Actions**:
- Fill in required fields
- Add skills by typing and pressing Enter (tag creation)
- Remove skills by clicking X on tag
- Add work history details
- Click "Next" to proceed to Step 3
- Click "Back" to return to Step 1 (data is preserved)

**System Behavior - Validation**:

**Phase 0 (Demo MVP) - Simplified Validation**:
- **Skills validation**:
  - Minimum 3 skills required
  - Maximum 10 skills (simple counter)
  - Basic duplicate prevention
  - No auto-save in Phase 0

- **Experience validation**:
  - Years of experience selection required
  - Job title minimum 2 characters
  - Company name minimum 2 characters

- **Date validation**:
  - Basic check: "From" date before "To" date
  - Simple validation on form submit

**Phase 1 (Extended MVP) - Enhanced Features**:
- Auto-save to localStorage every 30 seconds
- "Draft saved" indicator
- Data recovery on page reload
- Real-time character counters
- Date overlap warnings for multiple positions
- Advanced duplicate skill detection

**Error Scenarios (Phase 0)**:
- **Missing required skills**: Show "Please add at least 3 skills" on submit
- **Invalid date range**: Show "End date must be after start date" on submit
- **Description too long**: HTML maxlength attribute (hard limit)

**Phase 1 Additional Features**:
- Real-time skill limit warnings
- Character count indicators
- Proactive validation messages

**Success State**:
- All validations pass
- Progress indicator shows Step 2 complete
- Transition to Step 3

---

### 1.6 Document Upload Phase (Step 3)

**Step 6: Uploading Required Documents**

**Required Documents**:
- Resume/CV (mandatory)

**Optional Documents**:
- Cover letter
- Certifications
- Portfolio/work samples
- References

**File Requirements**:
- Accepted formats: PDF, DOC, DOCX (Phase 1: JPG, PNG for certifications)
- Maximum file size: 10MB per file
- Maximum 5 documents total

**User Actions**:
- Click "Upload" button or drag-and-drop files
- Select file from system file browser
- View uploaded file list with file names and sizes
- Preview uploaded documents (Phase 1)
- Replace/delete uploaded files
- Click "Next" to proceed to Step 4
- Click "Back" to return to Step 2 (uploads are preserved)

**System Behavior - Upload Process**:

**Phase 0 (Demo MVP)**:
1. User selects file
2. Basic client-side validation (file extension only: .pdf, .doc, .docx)
3. Simple file size check (< 10MB)
4. Upload to server (simple POST request)
5. Store file in storage (AWS S3 or similar)
6. Display success message with file name
7. No virus scanning (security measures in Phase 1)

**Phase 1 (Extended MVP)**:
- Add virus scanning (ClamAV or cloud service)
- File preview/thumbnail generation
- Upload progress percentage indicator
- Multiple file type support (JPG, PNG for certificates)
- Pause/resume for large files
- MIME type validation (not just extension)
- Document thumbnails

**Phase 0 Validation (Minimal)**:
- **File extension check**: Accept only .pdf, .doc, .docx
- **File size check**: Maximum 10MB (client-side only)
- **Document count**: Maximum 5 files (simple counter)

**Error Scenarios (Phase 0)**:
- **File too large**: Show "File exceeds 10MB limit"
- **Invalid file type**: Show "Only PDF, DOC, and DOCX files accepted"
- **Upload failed**: Show "Upload failed. Please try again"
- **No resume uploaded**: Disable "Next" button

**Phase 1 Additional Errors**:
- Virus detected scenarios
- Network interruption recovery
- Advanced retry mechanisms

**Success State**:
- At least resume uploaded successfully
- All files displayed in uploaded list
- File icons show correct document type
- "Next" button enabled

---

### 1.7 Review & Submit Phase (Step 4)

**Step 7: Final Review Before Submission**

**Page Display**:
- **Section 1: Personal Information Summary**
  - Name, Email, Phone, Location
  - Edit button → Returns to Step 1

- **Section 2: Professional Information Summary**
  - Current role, Years of experience
  - Education, Skills (displayed as tags)
  - Work history summary
  - Edit button → Returns to Step 2

- **Section 3: Uploaded Documents**
  - List of all uploaded files with icons
  - File sizes displayed
  - Edit button → Returns to Step 3

- **Section 4: Consent & Agreements** (Phase 1)
  - Checkbox: "I confirm the information provided is accurate"
  - Checkbox: "I agree to the privacy policy and terms" (with links)
  - Required to submit

**User Actions**:
- Review all entered information
- Click "Edit" on any section to make changes
- Check consent checkboxes (Phase 1)
- Click "Submit Application"
- Click "Back" to return to Step 3

**System Behavior - Pre-Submit Validation**:
- Verify all required fields still valid
- Confirm all files still in temporary storage
- Check job posting still active
- Validate consent checkboxes checked (Phase 1)

**Error Scenarios**:
- **Session expired**: Show "Your session has expired. Don't worry, your data is saved. Click to refresh."
- **Job closed**: Show "This position has been closed. Your application cannot be submitted. Would you like to apply to similar roles?"
- **Consent not provided**: Show "Please confirm your agreement to proceed"
- **File missing**: Show "One or more documents failed to upload. Please re-upload."

---

### 1.8 Application Submission Phase

**Step 8: Submitting the Application**

**User Action**:
- Click "Submit Application" button

**System Process**:
1. Show loading spinner on button ("Submitting...")
2. Disable submit button to prevent double-submission
3. Bundle all form data into application payload
4. Move uploaded files from temporary to permanent storage
5. Generate unique application ID
6. Create candidate record in database
7. Link application to job posting
8. Generate one-page profile automatically (background job)
9. Send confirmation email to candidate
10. Redirect to confirmation page

**Database Operations**:
```
CREATE Candidate Record:
- Personal info
- Professional info
- Application timestamp
- Application ID
- Status: "New"

CREATE Application Record:
- Link to Candidate ID
- Link to Job ID
- Application date
- Status: "Submitted"

CREATE Document Records:
- Link to Application ID
- File storage paths
- File metadata

TRIGGER Profile Generation:
- Queue background job
- Generate PDF from candidate data
- Store profile file reference
```

**Error Scenarios**:
- **Database error**: Show "Submission failed. Please try again in a moment." Log error for debugging
- **Email send failure**: Application still saved, show warning "Application submitted but confirmation email failed. Please check your spam folder."
- **Storage error**: Show "There was an issue saving your documents. Please contact support@agency.com with application ID: [ID]"
- **Timeout**: Show "Submission is taking longer than expected. We're still processing. Please don't close this window."

**Success Triggers**:
- Database records created successfully
- Files stored permanently
- Email queued for sending
- Application ID generated

---

### 1.9 Confirmation & Next Steps Phase

**Step 9: Application Confirmation**

**Confirmation Page Display**:
- Success message: "Application Submitted Successfully!"
- Application ID prominently displayed
- Job title and company reminder
- Summary of next steps:
  - "Our team will review your application"
  - "You'll receive updates at [candidate_email]"
  - "Typical response time: 5-7 business days"
- Call-to-action buttons:
  - "Apply to Another Position"
  - "Download Application Receipt" (PDF)
  - "Return to Job Listings"

**Email Confirmation Content**:
```
Subject: Application Received - [Job Title] at [Company]

Dear [Candidate Name],

Thank you for applying to the [Job Title] position!

Application ID: [ID]
Submitted: [Date and Time]

What's Next:
- Our recruitment team will review your application
- We'll contact qualified candidates within 5-7 business days
- Please keep this email for your records

Documents Submitted:
- [List of uploaded documents]

If you have questions, reply to this email or contact us at [agency_email].

Best regards,
[Agency Name] Recruitment Team
```

**User Actions**:
- Download application receipt (optional)
- Apply to another position
- Leave the platform
- Save application ID for reference

**System Behavior - Post-Submission**:
- **Notification to Agency**:
  - Send internal notification to agency staff
  - Email alert with candidate name, job title, and application ID
  - Update agency dashboard with new application count

- **Profile Generation** (Background):
  - Queue job to generate one-page profile
  - Extract key information from application
  - Apply standardized template
  - Generate PDF
  - Link PDF to candidate record in database
  - Update profile generation status: "Completed"

**Edge Cases**:
- **Candidate tries to apply again to same job**: Show "You've already applied to this position. Track your application status with ID: [ID]"
- **Email bounces**: Flag candidate record for admin follow-up
- **Profile generation fails**: Log error, retry automatically, notify admin if multiple failures

---

### 1.10 Alternative Paths & Edge Cases

#### Path A: Candidate Abandons Application

**Scenario**: Candidate starts application but doesn't complete it

**Phase 0 (Demo MVP)**:
- Application data lost on page close
- No recovery mechanism

**Phase 1 (Extended MVP)**:
- Form data saved to localStorage
- On return, show "You have an incomplete application. Would you like to continue?"
- Recover data and resume from last step
- Optional: Send reminder email after 24 hours (if email was provided)

#### Path B: Duplicate Application Prevention

**Scenario**: Candidate tries to apply twice to the same job

**System Check**:
- On form submission, check if email+jobID combination exists
- If match found:
  - Show message: "You've already applied to this position on [date]"
  - Offer options:
    - "View Application Status"
    - "Update Application" (Phase 1)
    - "Apply to Different Position"

#### Path C: Application Deadline Handling

**Scenario**: Candidate starts application, but deadline passes during process

**System Behavior**:
- Check job status at Step 4 (Review & Submit)
- If job closed:
  - Prevent submission
  - Show: "Application deadline has passed while you were completing the form"
  - Offer to save data for future applications (Phase 1)
  - Suggest similar open positions

#### Path D: Technical Error Recovery

**Scenario**: Browser crash, tab closed accidentally, system error

**Recovery Mechanism** (Phase 1):
- Auto-save every 30 seconds to localStorage
- Store session token in cookie
- On page reload:
  - Check for saved session
  - Restore form data
  - Prompt: "Welcome back! Would you like to continue your application?"

---

## 2. Agency Review Flow

### Overview
This workflow describes how recruitment agency staff review applications, manage candidates, and make hiring decisions through the agency portal.

### 2.1 Agency Portal Access Phase

**Step 1: Login to Agency Portal**

**Entry Point**: Staff navigates to agency portal URL

**User Actions**:
- Enter email address
- Enter password
- Click "Sign In"
- Optional: Check "Remember me" (Phase 1)

**System Behavior - Authentication**:
- Validate email format
- Check credentials against database
- Verify account is active (not suspended/deactivated)
- Check user role (Admin, Recruiter, Viewer)
- Create session token
- Redirect to dashboard based on role

**Error Scenarios**:
- **Invalid credentials**: Show "Email or password is incorrect"
- **Account locked**: Show "Account temporarily locked due to multiple failed attempts. Reset password or contact admin."
- **Inactive account**: Show "Account is inactive. Please contact system administrator."
- **Network error**: Show "Cannot connect to server. Check your internet connection."

**Success State**:
- User authenticated successfully
- Session established
- Redirect to appropriate dashboard
- Display welcome message with user name

---

### 2.2 Dashboard Overview Phase

**Step 2: Landing on Dashboard**

**Page Display - Admin/Recruiter View**:
- **Header**: Welcome message, user profile menu, notifications icon
- **Quick Stats Cards**:
  - Total Active Jobs
  - New Applications (today/this week)
  - Candidates Pending Review
  - Interviews Scheduled (Phase 1)

- **Recent Activity Feed**:
  - Latest applications
  - Status changes
  - Notes added by team members

- **Quick Actions**:
  - "Post New Job"
  - "View All Applications"
  - "Search Candidates"
  - "Generate Reports" (Phase 2)

**Navigation Menu**:
- Dashboard
- Jobs (Manage postings)
- Applications (Review new submissions)
- Candidate Pool (All candidates)
- Reports & Analytics (Phase 2)
- Settings

**User Actions**:
- View overview metrics
- Click on any stat card to drill down
- Navigate to specific section via menu
- Check notifications
- Search for candidates or jobs

**System Behavior**:

**Phase 0 (Demo MVP)**:
- Load dashboard data from API (simple GET request)
- Display basic metrics (static counts)
- Show most recent 10 activities
- Manual refresh only (no auto-refresh)

**Phase 1 (Extended MVP)**:
- Real-time metrics calculation
- Auto-refresh notifications every 60 seconds
- WebSocket connections for live updates
- Advanced analytics

---

### 2.3 Viewing New Applications Phase

**Step 3: Accessing New Applications**

**Trigger**: Staff clicks "Applications" in menu or "New Applications" card

**Applications List Page Display**:
- **Filter Panel** (left sidebar):
  - Job posting (dropdown - all jobs)
  - Application date (date range picker)
  - Status (New, Under Review, Shortlisted, etc.)
  - Experience level
  - Skills (multi-select)

- **Applications Table**:
  - Columns:
    - Candidate Name (sortable)
    - Applied For (job title)
    - Application Date (sortable)
    - Experience
    - Top Skills (tags)
    - Status (badge)
    - Actions (View, Quick View, More)

- **Pagination**: 10/25/50 per page
- **Bulk Actions**: Select multiple → Change status, Export, Delete

**User Actions**:
- Apply filters to narrow down list
- Sort by column headers
- Select individual applications
- Click "View" to open full candidate profile
- Use quick actions dropdown for status changes
- Export selected applications

**System Behavior**:

**Phase 0 (Demo MVP)**:
- Load applications with default filter: Status = "New"
- Sort by application date (most recent first)
- Display total count above table
- Basic pagination (10 per page, hard-coded)
- Manual refresh only

**Phase 1 (Extended MVP)**:
- Real-time updates (new applications appear automatically)
- Advanced filters with multi-select
- Bulk actions (status change, export, delete)
- Saved filter presets
- Customizable pagination (10/25/50 per page)
- WebSocket for live updates

---

### 2.4 Reviewing Candidate Profile Phase

**Step 4: Opening Candidate Detail View**

**Trigger**: Staff clicks "View" button on an application

**Candidate Profile Page Layout**:

**Left Panel - Candidate Information**:
- Profile photo placeholder/initials
- Full name (large heading)
- Contact information (email, phone) with click-to-copy icons
- Current location
- LinkedIn profile link (if provided)
- Application ID (small text, for reference)

**Center Panel - Detailed Information**:

**Tab 1: Overview**
- Professional headline (current role)
- Years of experience badge
- Education summary
- Key skills (displayed as colored tags)
- Work history cards:
  - Job title, Company
  - Duration
  - Responsibilities (expandable)
- Certifications section
- Availability & location preferences

**Tab 2: Documents**
- List of all uploaded documents
- Document previewer (PDF/DOC inline viewer)
- Download buttons for each file
- Document upload dates

**Tab 3: Application Details** (Phase 1)
- Application timeline (when applied, when viewed, status changes)
- Cover letter or summary statement
- Salary expectations (if provided)
- Custom application questions answers (Phase 2)

**Tab 4: Internal Notes** (Phase 1)
- Notes feed (reverse chronological)
- Each note shows:
  - Staff member who added it
  - Date and time
  - Note content
- Add note section at top:
  - Rich text editor
  - Private note checkbox
  - Save button

**Right Panel - Actions & Status**:
- **Current Status**: Badge with dropdown to change
  - New
  - Under Review
  - Shortlisted
  - Interviewed
  - Offered
  - Rejected
  - Hired
  - In Pool (not for specific role)

- **Quick Actions Buttons**:
  - Download One-Page Profile (PDF)
  - Email Candidate (Phase 1)
  - Schedule Interview (Phase 1)
  - Add to Different Job (Phase 1)
  - Move to Pool
  - Reject Application

- **Tags Section** (Phase 1):
  - Add custom tags
  - Color-coded labels
  - Remove tags

- **Activity Log**:
  - Profile viewed by [Staff Name] on [Date]
  - Status changed to [Status] by [Staff Name]
  - Note added by [Staff Name]
  - Document downloaded by [Staff Name]

---

### 2.5 Accessing One-Page Profile Phase

**Step 5: Viewing/Downloading Generated Profile**

**Trigger**: Staff clicks "Download One-Page Profile" button

**System Behavior**:

**Phase 0 (Demo MVP)**:
- Check if profile PDF exists in database
- If not generated, show "Generating profile..." message
- Profile generated on-demand (synchronous, may take 2-3 seconds)
- Download PDF directly to user's system
- Basic PDF template (no customization)

**Phase 1 (Extended MVP)**:
- Background job queue for profile generation
- Async generation with progress indicator
- Retry mechanism if generation fails
- Multiple template options
- Agency branding customization

**One-Page Profile PDF Contents**:

**Header Section**:
- Candidate initials or photo in circle
- Full name (large, bold)
- Professional headline
- Contact information (email, phone, location)
- LinkedIn icon with URL

**Professional Summary Section** (1-2 lines):
- Auto-generated from application data
- Years of experience, key expertise areas

**Key Skills Section**:
- Top 5-6 skills displayed as styled badges
- Organized by category if applicable

**Experience Section**:
- Last 2-3 positions
- Each showing:
  - Job title (bold)
  - Company name
  - Duration
  - 2-3 key responsibilities bullets

**Education Section**:
- Highest degree
- Institution name
- Certifications listed

**Footer**:
- "Documents Available: Resume, [Other docs]"
- Application ID
- Applied for: [Job Title]
- Application date

**Design Specifications**:
- Single page layout (no overflow)
- Professional color scheme (agency branding)
- Print-friendly (black text, minimal graphics)
- Consistent typography hierarchy
- Clean section dividers

**User Actions with Profile**:
- View in browser PDF viewer
- Download to local system
- Print directly
- Share with team members (Phase 1)
- Email to hiring manager (Phase 1)

**Error Scenarios**:
- **Profile generation failed**: Show "Profile generation in progress. Refresh in a moment." with retry button
- **Missing data**: Profile still generates but shows placeholders for missing sections
- **Download blocked by browser**: Show instructions to allow downloads from this site

---

### 2.6 Candidate Status Management Phase

**Step 6: Updating Candidate Status**

**Trigger**: Staff clicks status dropdown in right panel

**Available Statuses & Meanings**:
- **New**: Just submitted, not yet reviewed
- **Under Review**: Being actively evaluated
- **Shortlisted**: Passed initial screening, candidate for interview
- **Interviewed**: Interview completed, awaiting decision
- **Offered**: Job offer extended
- **Rejected**: Not selected for this position
- **Hired**: Accepted offer, hired
- **In Pool**: Not for current job, but good for future opportunities

**User Actions**:
- Select new status from dropdown
- Add reason/note (required for "Rejected" status)
- Confirm status change

**System Behavior - Status Change Process**:

**Phase 0 (Demo MVP)**:
1. Validate user has permission to change status
2. Update candidate status in database
3. Log status change in activity feed
4. Update applications list in real-time
5. Show success toast: "Status updated to [Status]"

**Phase 1 (Extended MVP)**:
1. All Phase 0 steps, plus:
2. If status = "Rejected":
   - Show modal: "Reason for rejection" (required field)
   - Options: Not qualified, Position filled, No response, Other
   - Optional: Send rejection email to candidate
3. If status = "Shortlisted" or "Interviewed":
   - Optional: Send email update to candidate
   - Show "Schedule Interview" quick action
4. If status = "Hired":
   - Trigger congratulations email
   - Update job posting candidate count
   - Archive application from active queue

**Status Change Permissions**:
- **Viewer**: Can view status, cannot change
- **Recruiter**: Can change to any status except "Hired"
- **Admin**: Full permission

**Error Scenarios**:
- **Insufficient permissions**: Show "You don't have permission to change this status. Contact your administrator."
- **Network error**: Show "Status update failed. Your changes were not saved. Please try again."
- **Concurrent edit**: Show "Another user modified this candidate. Refresh to see latest status."

---

### 2.7 Adding Internal Notes Phase (Phase 1)

**Step 7: Adding Notes to Candidate Profile**

**Trigger**: Staff navigates to "Internal Notes" tab and clicks "Add Note"

**Add Note Interface**:
- Rich text editor with formatting options:
  - Bold, italic, underline
  - Bulleted/numbered lists
  - @ mentions (tag team members)
- Character limit: 1000 characters (with counter)
- "Mark as Private" checkbox (visible only to note author and admins)
- "Save Note" and "Cancel" buttons

**User Actions**:
- Type note content
- Format text if needed
- @ mention colleague to notify them
- Toggle private checkbox if sensitive
- Click "Save Note"

**System Behavior**:
- Save note to database with:
  - Candidate ID
  - Author ID
  - Timestamp
  - Note content
  - Private flag
- If @ mentions present:
  - Send notifications to mentioned users
  - Email notification (optional setting)
- Add note to activity log
- Display note in notes feed immediately

**Note Display**:
- Author name and avatar
- Timestamp (relative: "2 hours ago")
- Note content (with formatted text)
- Edit/Delete options (only for author or admin)
- Private indicator (lock icon) if applicable

**Use Cases for Notes**:
- "Phone screened on [date]. Good communication skills. Schedule technical interview."
- "Concerned about salary expectations being too high for this role."
- "Strong portfolio. Let's fast-track to final round."
- "@JohnDoe can you review the technical assessment?"

**Error Scenarios**:
- **Empty note**: Disable save button, show "Note cannot be empty"
- **Character limit exceeded**: Show warning, prevent typing beyond limit
- **Save failed**: Show "Note could not be saved. Please try again."

---

### 2.8 Candidate Pool Management Phase

**Step 8: Managing the Candidate Pool**

**Trigger**: Staff clicks "Candidate Pool" in navigation menu

**Candidate Pool Page Display**:

**Filter & Search Panel**:
- **Search Bar**: Search by name, email, skills, keywords
- **Advanced Filters**:
  - Skills (multi-select with autocomplete)
  - Experience level (0-1, 1-3, 3-5, 5-10, 10+ years)
  - Education level
  - Location (city, country)
  - Availability (Immediate, 2 weeks, 1 month, etc.)
  - Application date range
  - Status (all statuses including "In Pool")
  - Jobs applied for
- **Save Filter Preset** (Phase 1): Save commonly used filter combinations

**Candidates Display**:
- **View Options**: Grid view or List view toggle
- **Sort Options**:
  - Relevance (default for keyword searches)
  - Date applied (newest first)
  - Experience (highest first)
  - Name (A-Z)

**Grid View**:
- Candidate cards showing:
  - Initials/photo
  - Name
  - Current role
  - Years of experience
  - Top 3 skills
  - Applied for (job title)
  - Status badge
  - Quick actions icon

**List View**:
- Table format similar to Applications list
- More details visible without clicking

**Bulk Actions** (Phase 1):
- Select multiple candidates
- Actions:
  - Export profiles (PDF bundle)
  - Change status
  - Add tags
  - Send email
  - Add to job consideration

**User Actions**:
- Search for specific candidates or skills
- Apply filters to narrow pool
- Browse candidates
- Click to view full profile
- Use bulk actions for efficiency
- Export filtered candidate list

**System Behavior**:
- Load all candidates (paginated, 24 per page)
- Apply search and filters in real-time
- Highlight keyword matches in search results
- Show total candidate count
- Display "Showing X of Y candidates"

**Search Examples**:
- "React developer" → Returns candidates with React in skills or job titles
- "Senior product manager Toronto" → Returns senior PMs in Toronto area
- "5+ years experience Python" → Returns candidates matching criteria

**Phase 2 Features**:
- AI-powered candidate matching
- Suggested candidates for open jobs
- Candidate comparison tool
- Advanced analytics on candidate pool demographics

---

### 2.9 Exporting Candidate Profiles Phase

**Step 9: Bulk Exporting Profiles**

**Trigger**: Staff selects multiple candidates and clicks "Export Profiles"

**Export Options Modal**:

**Phase 0 (Demo MVP) - Simplified Export**:
- **Format**: PDF only (one-page profiles)
- **Limit**: Maximum 10 candidates at once
- **Options**: No customization in Phase 0
- **Output**: Individual PDFs in ZIP file

**Phase 1 (Extended MVP) - Advanced Export**:
- Multiple format options (PDF, Excel, CSV)
- Combined PDF option
- Include full resumes/CVs
- Include cover letters and certifications
- Internal notes export (admin only)
- Sort options
- Cover sheet with summary
- Maximum 50 candidates
- Email download link for large files

**User Actions**:
- Select export format
- Choose what to include
- Click "Generate Export"
- Wait for processing
- Download generated file

**System Behavior - Export Process**:

**Phase 0 (Demo MVP) - Simple Export**:
1. Validate user has permission to export
2. Show loading spinner: "Generating export..."
3. For each selected candidate (max 10):
   - Retrieve one-page profile PDF from database
4. Create simple ZIP file with all PDFs
5. Name files: `[CandidateName]_Profile.pdf`
6. Auto-download ZIP file
7. Show success message

**Phase 1 (Extended MVP) - Advanced Export**:
1. All Phase 0 steps, plus:
2. Show progress modal with percentage
3. Include additional documents (resumes, cover letters)
4. Create folder structure in ZIP
5. Combined PDF option with table of contents
6. Email download link for large files (>50MB)
7. Export to Excel/CSV formats
8. Add cover sheet with summary statistics

**Export Naming Convention**:
- Individual: `[CandidateName]_[ApplicationID]_Profile.pdf`
- Bulk: `Candidate_Export_[Date]_[JobTitle].zip`
- Spreadsheet: `Candidates_Export_[Date].xlsx`

**Error Scenarios**:

**Phase 0 (Demo MVP)**:
- **Too many candidates**: Show "Maximum 10 candidates can be exported at once"
- **Export failed**: Show "Export failed. Please try again"
- **Missing profiles**: Show "Some profiles unavailable. Export may be incomplete"

**Phase 1 (Extended MVP) - Additional Errors**:
- Export timeout with email fallback
- Storage quota exceeded warnings
- Detailed error messages for missing files
- Automatic retry mechanisms

**Success State**:
- Export completes successfully
- File downloads automatically
- Show success message: "Export complete. X profiles exported."
- Log export action in audit trail (admin feature)

---

### 2.10 Alternative Paths & Edge Cases

#### Path A: Shortlisting for Interview

**Scenario**: Recruiter wants to move candidate to interview stage

**User Actions**:
1. Change status to "Shortlisted"
2. System shows "Schedule Interview" quick action
3. Click "Schedule Interview" (Phase 1)
4. Modal opens with:
   - Date/time picker
   - Interview type (Phone, Video, In-person)
   - Interviewer selection (team members)
   - Location/meeting link field
   - Send calendar invite checkbox
5. Click "Schedule"
6. System sends:
   - Calendar invite to candidate
   - Reminder to interviewer(s)
   - Updates candidate status to "Interview Scheduled"

**Phase 0 Workaround**:
- Status changed to "Shortlisted"
- Note added: "Schedule interview"
- Manual email sent to candidate outside platform

#### Path B: Rejecting Candidate

**Scenario**: Candidate doesn't meet requirements

**User Actions**:
1. Select status "Rejected"
2. Required modal appears: "Reason for rejection"
3. Options:
   - Not qualified for this role
   - Position filled
   - Candidate withdrew
   - Other (text field)
4. Checkbox: "Add to talent pool for future opportunities"
5. Checkbox: "Send rejection email to candidate" (Phase 1)
6. Click "Confirm Rejection"

**System Behavior**:
- Update status to "Rejected"
- If "Add to pool" checked: Change status to "In Pool" instead
- Log rejection reason (internal only, not visible to candidate)
- If email opted:
  - Send templated rejection email
  - Professional and courteous tone
  - Encourage future applications
- Remove from active job applicants list
- Keep in candidate pool with "Rejected for [Job Title]" note

**Email Template** (Phase 1):
```
Subject: Update on Your Application - [Job Title]

Dear [Candidate Name],

Thank you for your interest in the [Job Title] position at [Company Name].

After careful consideration, we've decided to move forward with other candidates whose experience more closely aligns with the role requirements at this time.

We were impressed by your background and encourage you to apply for future openings that match your qualifications.

Best regards,
[Agency Name] Recruitment Team
```

#### Path C: Multiple Recruiters Reviewing Same Candidate

**Scenario**: Concurrent editing and status conflicts

**System Behavior - Conflict Prevention**:
- **Real-time status updates**: When Recruiter A changes status, Recruiter B sees update immediately
- **Edit indicators**: Show "Currently being viewed by [Name]" banner
- **Optimistic locking**: Last write wins, but warn users
- **Activity feed**: All changes logged with timestamp and user

**Conflict Resolution**:
1. Recruiter A opens candidate profile at 10:00 AM
2. Recruiter B opens same profile at 10:02 AM
3. Recruiter A changes status to "Shortlisted" at 10:05 AM
4. Recruiter B tries to change status to "Under Review" at 10:06 AM
5. System shows warning: "This candidate was recently updated by [Recruiter A]. Current status: Shortlisted. Continue with your change?"
6. Recruiter B options:
   - "Refresh and Review" - See latest changes first
   - "Proceed Anyway" - Override previous change
   - "Cancel" - Keep Recruiter A's change

**Phase 1 Enhancement**:
- Real-time collaboration indicators
- Presence awareness (who's viewing profile)
- Comment threads on profiles
- @ mentions to loop in colleagues

#### Path D: Candidate Requests Profile Deletion (GDPR)

**Scenario**: Candidate exercises right to be forgotten

**User (Admin) Actions**:
1. Receive deletion request via email/form
2. Navigate to candidate profile
3. Click "More Actions" → "Delete Candidate Data"
4. Confirmation modal:
   - Warning: "This action cannot be undone"
   - List of data to be deleted:
     - Personal information
     - Application history
     - Uploaded documents
     - Generated profiles
   - Checkbox: "I confirm this is a legitimate deletion request"
   - Reason dropdown: GDPR request, Duplicate entry, Test data, etc.
5. Click "Delete Permanently"

**System Behavior - Data Deletion**:
1. Create deletion audit log entry
2. Anonymize candidate record:
   - Replace name with "Deleted User [ID]"
   - Remove email, phone, location
   - Clear all personal identifiers
3. Delete all uploaded documents from storage
4. Delete generated profile PDFs
5. Keep anonymized application statistics (for job posting analytics)
6. Send deletion confirmation email to candidate's original email (before deletion)
7. Log compliance action in admin audit trail

**Retention Rules**:
- Financial records: Keep anonymized for 7 years (if applicable)
- Anonymized stats: Keep for analytics (no PII)
- Notes/comments: Delete personal references, keep generalized insights for improvement

---

## 3. Job Posting Management Flow

### Overview
This workflow describes how recruitment agency staff create, edit, publish, and manage job postings on the platform.

### 3.1 Creating New Job Posting Phase

**Step 1: Initiating Job Creation**

**Entry Point**: Agency staff clicks "Post New Job" button from dashboard or jobs page

**User Arrives At**: Create Job Posting form

**Form Structure**:

**Section 1: Basic Information**
- Job title (required, text field)
- Company/Organization name (auto-filled from agency settings or manual entry)
- Job description (required, rich text editor with formatting)
- Short summary (optional, 200 characters - for job card display)

**Section 2: Job Requirements**
- Required qualifications (rich text editor, bullets recommended)
- Preferred qualifications (optional, rich text editor)
- Experience level required (dropdown):
  - Entry Level (0-2 years)
  - Mid Level (2-5 years)
  - Senior Level (5-10 years)
  - Executive (10+ years)
- Required skills (tag input, add up to 10)

**Section 3: Job Details**
- Employment type (required, radio buttons):
  - Full-time
  - Part-time
  - Contract
  - Temporary
  - Internship
- Work location (required, dropdown/autocomplete):
  - Remote
  - Hybrid
  - On-site
- If on-site/hybrid: City, State/Province, Country (required)
- Salary range (optional):
  - Minimum (number)
  - Maximum (number)
  - Currency (dropdown)
  - Frequency (Annual, Hourly)
  - "Hide salary from candidates" checkbox

**Section 4: Application Settings**
- Application deadline (required, date picker)
  - Minimum: Today + 7 days
  - Maximum: Today + 90 days
  - Or "Rolling basis" checkbox (no specific deadline)
- Required documents checklist:
  - Resume/CV (always required, disabled checkbox)
  - Cover Letter (optional checkbox)
  - Portfolio/Work Samples (optional checkbox)
  - References (optional checkbox)
  - Certifications (optional checkbox)
  - Other (custom text field) - Phase 1
- Custom application questions (Phase 2):
  - Add up to 5 custom questions
  - Question types: Text, Multiple choice, Yes/No

**Section 5: Internal Settings**
- Job category/department (dropdown, agency-specific) - Phase 1
- Assigned recruiter (dropdown, select team member) - Phase 1
- Internal job reference number (text field) - Phase 1
- Private notes (text area, visible only to agency staff)

**User Actions**:
- Fill in all required fields
- Use rich text editor to format description
- Add tags for skills and qualifications
- Set application deadline
- Preview job posting (Phase 1)
- Save as draft (Phase 1)
- Click "Publish Job Posting"

**System Behavior - Validation**:
- **Required field checks**:
  - Job title (min 5 characters)
  - Job description (min 100 characters)
  - Experience level selected
  - Employment type selected
  - Location details (if not remote)
  - Application deadline valid date

- **Date validation**:
  - Deadline must be in future
  - Warn if deadline is less than 14 days (may not get enough applicants)

- **Salary validation**:
  - Maximum must be greater than minimum
  - Reasonable ranges (no obvious typos)

- **Skills validation**:
  - Minimum 3 required skills recommended
  - Maximum 10 skills

**Error Scenarios**:
- **Missing required field**: Highlight field, scroll to first error, show "Please complete all required fields"
- **Invalid date**: Show "Application deadline must be at least 7 days from today"
- **Invalid salary range**: Show "Maximum salary must be greater than minimum"
- **Network timeout**: Show "Connection lost. Your draft is saved. Please try again."

---

### 3.2 Publishing Job Posting Phase

**Step 2: Publishing the Job**

**User Action**: Click "Publish Job Posting" button after completing form

**Confirmation Modal** (Phase 1):
- Preview of job posting as candidates will see it
- Key details summary
- "Everything looks good?" confirmation
- "Publish Now" or "Edit" buttons

**System Behavior - Publication Process**:
1. Validate all required fields complete
2. Generate unique job ID
3. Create job posting record in database
4. Set status: "Active"
5. Set publication date: Current timestamp
6. Calculate closing date based on deadline
7. Index job for search (add to search engine)
8. Send confirmation to assigned recruiter (if any)
9. Update agency dashboard metrics
10. Redirect to job detail page with success message

**Database Record Structure**:
```
Job Posting Record:
- Job ID
- Title
- Description
- Requirements
- Details (type, location, salary)
- Application settings
- Status: "Active"
- Created by: [Staff User ID]
- Created date: [Timestamp]
- Closing date: [Deadline]
- View count: 0
- Application count: 0
```

**Success State**:
- Job posting published successfully
- Visible on public job listings immediately
- Success message: "Job posted successfully! It's now live and accepting applications."
- Options:
  - "View Public Listing"
  - "Share Job Link"
  - "Post Another Job"
  - "Return to Dashboard"

**Error Scenarios**:
- **Database error**: Show "Job could not be published. Please try again or contact support."
- **Duplicate job**: Warn "A similar job posting already exists. Are you sure you want to create another?"
- **Permission issue**: Show "You don't have permission to publish jobs. Contact your administrator."

---

### 3.3 Managing Published Jobs Phase

**Step 3: Viewing All Jobs**

**Entry Point**: Staff clicks "Jobs" in navigation menu

**Jobs Management Page Display**:

**Filter Panel**:
- Status (dropdown):
  - Active (currently accepting applications)
  - Closed (deadline passed)
  - Archived (manually closed)
  - Draft (unpublished) - Phase 1
- Date posted (date range)
- Employment type
- Location
- Assigned recruiter (Phase 1)
- Search by job title or ID

**Jobs Table**:
- Columns:
  - Job Title (sortable, link to detail)
  - Posted Date (sortable)
  - Closing Date (sortable)
  - Applications Count (sortable, link to applications)
  - Views Count (Phase 1)
  - Status (badge: Active, Closed, Archived)
  - Actions (dropdown)

**Actions Dropdown**:
- View Public Listing
- Edit Job Posting
- View Applications
- Duplicate Job (Phase 1)
- Archive Job
- Delete Job (admin only)
- Share Link
- View Analytics (Phase 2)

**User Actions**:
- Browse all job postings
- Filter by status or criteria
- Sort by columns
- Click job title to view details
- Use quick actions
- Bulk select and archive (Phase 1)

**System Behavior**:
- Load all jobs created by agency
- Default filter: Active jobs
- Sort by posted date (newest first)
- Show total count per status
- Real-time updates for application counts

---

### 3.4 Editing Job Posting Phase

**Step 4: Modifying Existing Job**

**Trigger**: Staff clicks "Edit" in actions dropdown

**Edit Job Form**:
- Same form as creation
- Pre-filled with existing data
- Changes tracked (Phase 1 - show "Last edited by [Name] on [Date]")

**User Actions**:
- Modify any field except:
  - Job ID (read-only)
  - Created date (read-only)
  - Application count (read-only)
- Save changes
- Cancel (discard changes)

**Important Notes Display** (Phase 1):
- "X candidates have already applied to this job"
- "Significant changes will not notify existing applicants"
- "Changing deadline: New deadline [date]"

**System Behavior - Update Process**:
1. Validate changes
2. If deadline extended:
   - Show warning: "Extending deadline will reopen applications"
   - Re-activate job if it was closed
3. Update database record
4. Update search index
5. Log edit in activity feed
6. Show success message
7. If job was closed and deadline extended:
   - Change status back to "Active"
   - Send notification to assigned recruiter

**Restrictions**:
- Cannot edit job if status = "Archived" (must un-archive first)
- Cannot shorten deadline to past date
- Cannot change job to significantly different role (suggest creating new posting)

**Error Scenarios**:
- **Invalid changes**: Show specific field errors
- **Concurrent edit**: Show "Another user is editing this job. Please refresh."
- **Permission denied**: Show "You don't have permission to edit this job."

---

### 3.5 Closing/Archiving Job Phase

**Step 5: Closing Job Posting**

**Scenarios**:
- **Auto-close**: Deadline reached, system automatically closes job
- **Manual close**: Position filled, staff archives job early

**Manual Archive Process**:

**Trigger**: Staff clicks "Archive Job" in actions dropdown

**Confirmation Modal**:
- "Are you sure you want to archive this job?"
- Reason for closing (dropdown):
  - Position filled
  - No longer hiring
  - Incorrect posting
  - Duplicate posting
  - Other (text field)
- "Archive and stop accepting applications" button
- "Cancel" button

**User Actions**:
- Select reason
- Confirm archive
- Or cancel action

**System Behavior - Archive Process**:
1. Validate user permission
2. Update job status to "Archived"
3. Remove from public listings immediately
4. Set archived date
5. Log archive action and reason
6. Move all "New" applications to "Under Review" (prevent loss)
7. Send notification to assigned recruiter
8. Show success message: "Job archived. It's no longer visible to candidates."

**Impact of Archiving**:
- Job removed from public job listings
- Existing applications preserved and accessible
- Job detail page shows "Position no longer accepting applications"
- Can be un-archived if needed (Phase 1)

**Auto-Close Process** (Deadline Reached):
1. Scheduled task runs daily at midnight
2. Check all active jobs for deadline = today
3. For each expired job:
   - Change status to "Closed"
   - Send notification to assigned recruiter: "Job [Title] deadline reached. X applications received."
   - Keep on public listing but mark as "No longer accepting applications"
   - Disable "Apply" button

**Phase 1 Feature - Reopen Job**:
- If closed by deadline, allow reopen
- Extend deadline
- Reactivate job posting
- Notify previous applicants of reopening (optional)

---

### 3.6 Sharing Job Posting Phase (Phase 1)

**Step 6: Sharing Job Link**

**Trigger**: Staff clicks "Share Link" in actions dropdown

**Share Modal Display**:
- **Public Job URL**: `https://platform.com/jobs/[job-id]`
  - Copy button
  - QR code generator (Phase 1)

- **Share via**:
  - Email (opens default email client with pre-filled template)
  - LinkedIn (share to company page) - Phase 2
  - Twitter/X - Phase 2
  - Copy to clipboard

- **Embed Code** (Phase 2):
  - HTML snippet to embed on company website
  - iFrame code with responsive design

**Email Template** (when sharing via email):
```
Subject: Job Opportunity - [Job Title]

Hello,

I wanted to share this opportunity that might be of interest:

[Job Title] at [Company Name]
Location: [Location]
Type: [Employment Type]

[Short description or summary]

Apply here: [Job URL]

Application deadline: [Date]
```

**User Actions**:
- Copy URL to clipboard
- Share via email
- Download QR code (PNG) - Phase 1
- Copy embed code - Phase 2

**System Behavior**:
- Generate shareable URL (always same URL per job)
- Track clicks on shared links (UTM parameters) - Phase 2
- Show share analytics in job dashboard - Phase 2

---

### 3.7 Duplicating Job Posting Phase (Phase 1)

**Step 7: Creating Similar Job from Existing**

**Trigger**: Staff clicks "Duplicate Job" in actions dropdown

**Duplication Process**:
1. Copy all job data except:
   - Job ID (new ID generated)
   - Posted date (set to current date)
   - Application count (reset to 0)
   - Applications (not copied)
2. Append "(Copy)" to job title
3. Reset deadline to +30 days from today
4. Set status to "Draft" (not published yet)
5. Open edit form with duplicated data

**User Actions**:
- Review duplicated job details
- Modify as needed (change title, update description, adjust deadline)
- Save as draft or publish immediately
- Or cancel duplication

**Use Cases**:
- Creating similar roles in different locations
- Reopening position after previous round closed
- Creating seasonal recurring positions
- A/B testing different job descriptions

**System Behavior**:
- Create new database record
- Do NOT copy over applications
- Link to original job (internal reference) - for analytics
- Show "Duplicated from: [Original Job Title]" note in admin view

---

### 3.8 Job Analytics & Reporting Phase (Phase 2)

**Step 8: Viewing Job Performance**

**Trigger**: Staff clicks "View Analytics" for a specific job

**Analytics Dashboard Display**:

**Overview Cards**:
- Total Views
- Total Applications
- Conversion Rate (Applications / Views %)
- Average Time to Apply
- Application Completion Rate
- Source Breakdown (where candidates found job)

**Charts & Graphs**:
- **Application Timeline**: Line chart showing applications over time
- **Traffic Sources**: Pie chart (Direct link, Search, Social, Referral)
- **Candidate Demographics**: Bar charts
  - Experience levels
  - Education levels
  - Geographic distribution
- **Top Skills**: Word cloud of most common skills in applications

**Insights Section**:
- "Applications per day: X average"
- "Peak application days: [Days of week]"
- "Most applications from: [City/Country]"
- "Average candidate experience: X years"

**Export Options**:
- Export analytics as PDF report
- Export application data as CSV
- Schedule weekly email reports (Phase 3)

**User Actions**:
- View charts and insights
- Compare with other jobs (Phase 2)
- Filter date ranges
- Export data
- Use insights to optimize future postings

---

### 3.9 Alternative Paths & Edge Cases

#### Path A: Draft Jobs (Phase 1)

**Scenario**: Staff wants to prepare job posting but not publish yet

**Process**:
1. Fill out job posting form
2. Click "Save as Draft" instead of "Publish"
3. Job saved with status: "Draft"
4. Not visible on public listings
5. Accessible from "Drafts" tab in Jobs page
6. Can resume editing anytime
7. Publish when ready

**Benefits**:
- Prepare job postings in advance
- Get team approval before publishing
- Test different versions
- Schedule publication (Phase 2)

#### Path B: Job Expiring Soon

**Scenario**: Job deadline approaching, few applications

**System Behavior** (7 days before deadline):
1. Send notification to assigned recruiter
2. Email: "Job [Title] expiring in 7 days. X applications received."
3. Suggest actions:
   - Extend deadline
   - Share job more widely
   - Review and adjust job description
   - Lower requirements

**Dashboard Alert**:
- Show warning badge on job listing
- "Expiring soon" tag
- Quick action to extend deadline

**Recruiter Actions**:
- Extend deadline with one click
- Share job via multiple channels
- Review and edit job posting
- Archive if no longer needed

#### Path C: High Volume Applications

**Scenario**: Job posting receives overwhelming applications

**System Behavior**:
- When application count > 50 (threshold configurable):
  - Send notification: "Job [Title] received 50+ applications"
  - Suggest: "Consider closing applications early or updating job requirements"

- When application count > 100:
  - Auto-enable advanced filters for that job's applications
  - Provide AI-assisted screening suggestions (Phase 2)
  - Offer bulk review tools

**Recruiter Actions**:
- Close applications early
- Add more specific requirements
- Enable auto-screening based on must-have criteria (Phase 2)
- Archive job if position filled

#### Path D: No Applications Received

**Scenario**: Job posted 14+ days ago, 0 applications

**System Behavior**:
- Send notification after 14 days with 0 applications
- Email: "Job [Title] has received no applications. Here's how to improve visibility:"
  - Suggestions:
    - Expand location to remote
    - Lower experience requirements
    - Increase salary range visibility
    - Simplify job title
    - Improve job description
  - Link to job posting best practices guide

**Dashboard Alert**:
- Show info badge on job listing
- "Needs attention" tag
- Quick link to analytics/insights

**Recruiter Actions**:
- Review and edit job description
- Adjust requirements or salary
- Share more widely
- Consider if role is still needed

---

## Summary & Key Takeaways

### Critical Success Factors

**For Candidate Applications**:
1. Simple, intuitive multi-step form
2. Real-time validation to prevent errors
3. Auto-save functionality to prevent data loss
4. Clear communication at every step
5. Fast document uploads with progress indication
6. Professional confirmation experience

**For Agency Review**:
1. Quick access to new applications
2. One-page profile for fast screening
3. Efficient filtering and search
4. Streamlined status management
5. Collaborative features (notes, @mentions)
6. Bulk actions for productivity

**For Job Management**:
1. Easy job creation with helpful guidance
2. Flexible editing while protecting applicant data
3. Automatic deadline management
4. Analytics to improve posting effectiveness
5. Sharing tools to increase reach
6. Draft capability for preparation

### Phase 0 vs Phase 1 Feature Matrix

| Feature | Phase 0 (Demo MVP) | Phase 1 (Extended MVP) |
|---------|-------------------|----------------------|
| **Validation** | Basic HTML5, on-submit | Real-time, field-by-field |
| **Error handling** | Generic messages | Detailed with recovery |
| **Application form** | Essential fields only | Full field set, auto-save |
| **Document upload** | Basic single file, extension check | Multiple files, virus scan, preview |
| **Profile generation** | Synchronous, basic template | Background job, customizable |
| **Status management** | Basic status changes | Email notifications, rejection reasons |
| **Internal notes** | Not included | Full rich text editor with @mentions |
| **Job editing** | Basic form editing | Version history, drafts, collaboration |
| **Candidate export** | Max 10, PDF only | Max 50, multiple formats |
| **Search & filters** | Basic filters, manual refresh | Advanced filters, real-time updates |
| **Sharing** | Copy URL only | Email, QR codes, social, embeds |
| **Analytics** | Not included | Full dashboard with charts |
| **Security** | File extension checks | Virus scanning, MIME validation |
| **Performance** | <5s page load, 50 users | <3s page load, 1000+ users |

---
