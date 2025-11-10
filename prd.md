# Product Requirements Document: Recruitment Agency Talent Pool Platform

## Document Information

- **Version**: 1.0  
- **Date**: October 22, 2025  
- **Status**: Draft  
- **Author**: Product Team

---

## 1\. Executive Summary

### 1.1 Purpose

This document outlines the requirements for a web-based recruitment platform designed specifically for recruitment agencies to manage job postings and build a curated pool of eligible candidates. The platform streamlines the candidate application and screening process by generating standardized one-page profiles.

### 1.2 Product Vision

Create an efficient, user-friendly platform that enables recruitment agencies to:

- Post and manage job opportunities
- Build and maintain a pool of eligible candidates
- Streamline candidate screening through standardized profile generation

### 1.3 Delivery Strategy and Timeline Approach

The project delivery has been restructured to prioritize early investor engagement and measurable progress:

**Two-Phase MVP Approach:**

1. **Phase 0: Demo MVP (4 weeks)** - Focused on investor presentation deliverables with core end-to-end workflows
2. **Phase 1: Extended MVP (4 weeks)** - Enhanced features for production readiness

This approach enables:
- Early visibility into progress and product validation
- Investor momentum through working demonstrations
- Stakeholder feedback collection at critical junctures
- Measurable milestones with tangible outputs

**Development Methodology:**
- Bi-weekly sprint model with visible progress demonstrations
- Incremental delivery of usable, testable functionality
- Running demo environment for continuous stakeholder validation
- Feature prioritization tied to investor demo goals and user value

---

## 2\. Product Overview

### 2.1 Target Users

1. **Primary Users (Recruitment Agency Staff)**  
     
   - Post job listings  
   - Review candidate profiles  
   - Manage candidate pool  
   - Screen applications efficiently

   

2. **Secondary Users (Job Candidates)**  
     
   - Browse available positions  
   - Submit applications  
   - Upload required documents  
   - Create professional profiles

### 2.2 Core Value Proposition

- **For Agencies**: Simplified candidate screening with standardized one-page profiles  
- **For Candidates**: Streamlined application process with reusable profile information

---

## 3\. Functional Requirements

### 3.1 Job Management System

#### 3.1.1 Job Posting (Agency Portal)

**Requirements:**

- Create new job postings with the following fields:  
  - Job title  
  - Job description  
  - Required qualifications  
  - Experience level  
  - Location (remote/hybrid/on-site)  
  - Employment type (full-time/part-time/contract)  
  - Salary range (optional)  
  - Application deadline  
  - Required documents list  
- Edit existing job postings  
- Activate/deactivate job listings  
- Archive closed positions  
- Duplicate job postings for similar roles

#### 3.1.2 Job Display (Public-Facing)

**Requirements:**

- Display all active job listings  
- Filter jobs by:  
  - Location  
  - Employment type  
  - Experience level  
  - Date posted  
- Search functionality for job titles and keywords  
- Job detail page with full description and application CTA

### 3.2 Candidate Application System

#### 3.2.1 Application Form

**Requirements:**

- Multi-step application form including:  
    
  **Personal Information:**  
    
  - Full name  
  - Email address  
  - Phone number  
  - Current location  
  - LinkedIn profile (optional)


  **Professional Information:**


  - Current job title  
  - Years of experience  
  - Education level and institution  
  - Professional certifications/licenses  
  - Areas of expertise/specialization  
  - Key skills (taggable/searchable)


  **Work History:**


  - Previous positions (title, company, duration)  
  - Brief description of responsibilities


  **Additional Information:**


  - Cover letter or summary statement  
  - Availability/notice period  
  - Salary expectations (optional)

#### 3.2.2 Document Upload

**Requirements:**

- Upload multiple documents:  
  - Resume/CV (required) \- PDF, DOC, DOCX  
  - Cover letter (optional) \- PDF, DOC, DOCX  
  - Certifications (optional) \- PDF, JPG, PNG  
  - References (optional) \- PDF, DOC, DOCX  
  - Portfolio/work samples (optional) \- PDF, DOC, DOCX  
- File size limit: 10MB per file  
- Maximum 5 documents per application  
- Drag-and-drop upload interface  
- Preview uploaded documents before submission  
- Ability to replace uploaded documents

#### 3.2.3 Form Validation

**Requirements:**

- Real-time field validation  
- Required field indicators  
- Email format validation  
- Phone number format validation  
- File type and size validation  
- Error messaging for invalid inputs

### 3.3 One-Page Profile Generation

#### 3.3.1 Profile Content

**Requirements:**

- Auto-generate a standardized one-page profile containing:  
  - Candidate photo placeholder/initials  
  - Full name and contact information  
  - Professional headline (current role)  
  - Years of experience  
  - Education summary  
  - Top 5-6 key skills  
  - Brief work history (last 2-3 positions)  
  - Certifications/licenses  
  - Availability and location  
  - Links to uploaded documents

#### 3.3.2 Profile Design

**Requirements:**

- Clean, professional layout  
- Consistent formatting across all profiles  
- Easy-to-scan sections  
- Print-friendly format (PDF export)  
- Responsive design for different screen sizes

### 3.4 Candidate Pool Management

#### 3.4.1 Candidate Database (Agency Portal)

**Requirements:**

- View all candidates in the pool  
- Advanced filtering by:  
  - Job applied for  
  - Experience level  
  - Skills/expertise  
  - Location  
  - Availability  
  - Application date  
  - Education level  
- Search functionality (name, skills, keywords)  
- Sort by relevance, date, experience  
- Bulk selection for actions

#### 3.4.2 Candidate Profiles

**Requirements:**

- View the generated one-page profile  
- Access all uploaded documents  
- Add internal notes/comments (agency staff only)  
- Tag candidates with custom labels  
- Mark candidates with status:  
  - New  
  - Under review  
  - Shortlisted  
  - Interviewed  
  - Offered  
  - Rejected  
  - Hired  
  - In pool (not for a specific role)

#### 3.4.3 Profile Export

**Requirements:**

- Export individual profiles as PDF  
- Bulk export selected candidate profiles  
- Generate comparison reports for shortlisted candidates

### 3.5 Authentication & Authorization

#### 3.5.1 Agency Portal Access

**Requirements:**

- Secure login system  
- Role-based access control:  
  - Admin (full access)  
  - Recruiter (manages jobs and candidates)  
  - Viewer (read-only access)  
- Password reset functionality  
- Two-factor authentication (optional)

#### 3.5.2 Candidate Access

**Requirements:**

- No account required for initial application  
- Optional candidate portal for:  
  - Updating profile information  
  - Tracking application status  
  - Applying to multiple positions

---

## 4\. Non-Functional Requirements

### 4.1 Performance

Performance targets are separated into Demo and Production environments to allow flexibility for early demo readiness without over-engineering initial builds.

**Demo MVP Targets:**
- Page load time < 5 seconds
- Document upload processing < 10 seconds for 10MB files
- Profile generation < 3 seconds
- Support up to 50 concurrent users
- Acceptable for controlled demo environments and investor presentations

**Full Production Targets:**
- Page load time < 3 seconds
- Document upload processing < 5 seconds for 10MB files
- Profile generation < 2 seconds
- Support up to 1,000 concurrent users
- Optimized for real-world usage and scale

### 4.2 Security

- HTTPS encryption for all data transmission  
- Secure document storage with encryption at rest  
- GDPR/data protection compliance  
- Regular security audits  
- Data retention policies  
- Secure file upload validation (virus scanning)

### 4.3 Scalability

- Architecture to support growth to 10,000+ candidates  
- Database optimization for large datasets  
- CDN for document delivery  
- Cloud-based infrastructure

### 4.4 Usability

- Intuitive interface requiring minimal training  
- Mobile-responsive design  
- Accessibility compliance (WCAG 2.1 AA)  
- Multi-browser support (Chrome, Firefox, Safari, Edge)

### 4.5 Reliability

- 99.5% uptime SLA  
- Automated backups (daily)  
- Disaster recovery plan  
- Error logging and monitoring

---

## 5\. User Workflows

**Note:** High-level flow diagrams and clickable prototypes will be developed during Phase 0 (Demo MVP) to visualize user journeys. These artifacts will support early investor demos, user feedback sessions, and alignment between business and technical teams.

### 5.1 Candidate Application Flow

1. Candidate browses job listings  
2. Selects the desired position  
3. Clicks "Apply Now"  
4. Fills out the multi-step application form  
5. Upload required documents  
6. Reviews the application summary  
7. Submits application  
8. Receives confirmation email with application ID  
9. The system generates a one-page profile automatically

### 5.2 Agency Review Flow

1. Agency staff logs into the portal  
2. View new applications/candidates  
3. Filters candidates by criteria  
4. Opens candidate one-page profile  
5. Reviews profile and documents  
6. Adds internal notes  
7. Updates the candidate status  
8. Shortlists candidates for interview  
9. Exports selected profiles for review meeting

---

## 6\. Technical Specifications

### 6.1 Codebase Audit and Reuse Strategy

**Important Note:** While backend access to a previous platform instance has been obtained, preliminary assessment indicates that the existing codebase utilizes outdated technology stacks and architectural patterns that are incompatible with modern development practices.

**Assessment Finding:**
- The legacy codebase uses deprecated frameworks and libraries
- Migration or adaptation would require significant refactoring effort
- Time investment to modernize legacy code would exceed the cost of building fresh
- Risk of technical debt and maintenance issues if legacy code is incorporated

**Recommendation:** Build the platform from scratch using current best practices and modern technology stack. This approach will:
- Reduce long-term technical debt
- Enable faster development velocity
- Ensure maintainability and scalability
- Leverage modern tooling and ecosystem support
- Provide cleaner architecture aligned with current requirements

A brief technical discovery phase (1-2 days) may still be conducted to identify any reusable business logic or data models, but code-level reuse is not recommended.

### 6.2 Technology Stack (Recommendations)

**Frontend:**

- React.js or Next.js for a web application  
- Tailwind CSS for styling  
- React Hook Form for form management

**Backend:**

- Node.js with Express.js or Next.js API routes  
- PostgreSQL or MongoDB for the database  
- AWS S3 or similar for document storage

**Profile Generation:**

- PDF generation library (PDFKit, Puppeteer, or React-PDF)  
- Template engine for consistent formatting

**Infrastructure:**

- Cloud hosting (AWS or Digital Ocean)  
- CDN for static assets  
- Email service (SendGrid, AWS SES)

### 6.3 API Requirements

- RESTful API or GraphQL
- API documentation (Swagger/OpenAPI)
- Rate limiting
- API versioning

### 6.4 Database Schema (Key Entities)

- **Jobs**: Job postings and metadata  
- **Candidates**: Candidate's personal and professional information  
- **Applications**: Link between candidates and jobs  
- **Documents**: File metadata and storage references  
- **Users**: Agency staff accounts and permissions  
- **Notes**: Internal comments on candidates

---

## 7\. Design Requirements

### 7.1 Branding

- Professional, trustworthy design aesthetic  
- Agency color scheme and logo integration  
- Consistent typography and spacing

### 7.2 Key Pages

1. **Public Site:**  
     
   - Homepage with job search  
   - Job listings page  
   - Job detail page  
   - Application form pages  
   - Confirmation page

   

2. **Agency Portal:**  
     
   - Dashboard with metrics  
   - Job management interface  
   - Candidate pool view  
   - Candidate detail view  
   - Settings/configuration

### 7.3 One-Page Profile Template

- Professional header with candidate name  
- Compact information hierarchy  
- Visual separation of sections  
- Consistent spacing and alignment  
- Print-optimized layout

---

## 8\. Data Requirements

### 8.1 Data Collection

- Only collect necessary information  
- Clear privacy policy  
- Consent for data storage  
- GDPR compliance (right to deletion, data export)

### 8.2 Data Retention

- Active candidates: Indefinite (with periodic confirmation)  
- Rejected candidates: Configurable retention period  
- Job postings: Archive after closing  
- Documents: Retained with candidate profile

---

## 9\. Integration Requirements

### 9.1 Email Integration

- Application confirmation emails  
- Status update notifications (optional)  
- New application alerts for agency staff  
- Weekly digest of new candidates

### 9.2 Calendar Integration (Future)

- Interview scheduling  
- Availability management

### 9.3 ATS Integration (Future)

- Export to standard ATS formats  
- API for third-party integrations

---

---

## 10\. Project Phases

The delivery structure has been reframed into a phased approach that prioritizes investor readiness and incremental value delivery.

### Phase 0: Demo MVP (Investor Presentation Build)

**Timeline: 4 weeks**

**Objectives:**
- Demonstrate platform value through working end-to-end workflows
- Enable investor interaction with functional prototype
- Validate core user experience and product concept
- Gather stakeholder feedback for refinement

**Scope:**
- Basic job posting creation and display
- Candidate application form (essential fields only)
- Document upload (CV/Resume required)
- One-page profile generation (standardized template)
- Simple candidate list view in agency portal
- Basic authentication for agency staff
- Minimal but functional UI/UX

**Success Metrics:**
- Complete job-to-application-to-profile workflow operational
- Demo-ready environment deployed and accessible
- Positive investor feedback on concept and usability
- No critical bugs during presentation scenarios

**Deliverables:**
- Working demo environment with sample data
- High-level flow diagrams and user journey visualizations
- Brief feature prioritization matrix
- Weekly progress demonstrations

### Phase 1: Extended MVP (Production-Ready Build)

**Timeline: 4 weeks**

**Objectives:**
- Enhance demo features for real-world usage
- Add operational functionality for agency staff
- Improve user experience and interface polish
- Prepare for early adopter onboarding

**Scope:**
- Advanced job posting management (edit, duplicate, archive)
- Enhanced application form with full field set
- Document management (multiple file types, preview)
- Advanced filtering and search in candidate pool
- Candidate status management workflow
- Internal notes and tagging system
- Bulk selection and export functionality
- Email notifications (application confirmation, new candidate alerts)
- Improved UI/UX with responsive design

**Success Metrics:**
- Platform ready for pilot users or early adopters
- Key workflows tested and validated through UAT
- Performance meets demo MVP targets
- Technical handover documentation complete

### Phase 2: Enhanced Features

**Timeline: 4-6 weeks**

**Objectives:**
- Scale platform for broader usage
- Add analytics and reporting capabilities
- Optimize performance for production targets

**Scope:**
- Analytics dashboard with key metrics
- Advanced candidate comparison and reporting
- Candidate portal with login (optional profile updates)
- Application tracking for candidates
- Enhanced search with keyword indexing
- Performance optimizations (caching, database indexing)
- Security hardening and compliance review

### Phase 3: Optimization and Integration

**Timeline: 4-6 weeks**

**Scope:**
- API for third-party integrations
- Advanced reporting and data export options
- Calendar integration for interview scheduling
- Mobile-responsive optimizations
- ATS export formats
- Mobile app (optional)

---

## 11\. Risk Assessment

### 11.1 Potential Risks

1. **Data Security**: Sensitive candidate information requires robust security
   - *Mitigation*: Implement industry-standard encryption and security practices
   - Conduct security audits at each phase completion
   - Use secure file upload validation with virus scanning

2. **Data Privacy Compliance**: GDPR and other regulations
   - *Mitigation*: Build compliance into design from the start
   - Implement data retention policies and right to deletion
   - Clear privacy policy and consent mechanisms

3. **Scalability**: Rapid growth in candidate pool
   - *Mitigation*: Cloud-based infrastructure with auto-scaling
   - Database optimization and indexing strategy
   - CDN for document delivery

4. **Document Storage Costs**: Large file volumes
   - *Mitigation*: File size limits, compression, storage optimization
   - Implement tiered storage strategy (active vs archived)

5. **Timeline Risk**: Aggressive schedule for investor demos may impact quality
   - *Mitigation*: Adopt phased delivery with clear scope boundaries
   - Use feature toggles to enable/disable incomplete features
   - Prioritize "Must-Have" features for Phase 0
   - Build buffer time for UAT and bug fixes
   - Maintain weekly progress checkpoints with stakeholders

6. **Dependency Risk**: Reliance on third-party services and tools
   - *Mitigation*: Identify critical dependencies early (email service, document storage, PDF generation)
   - Have backup providers or alternatives documented
   - Use abstraction layers for easy switching
   - Set up early access environments to validate integrations
   - Avoid vendor lock-in through standard APIs and formats

7. **Backend Readiness**: Development pace depends on API and infrastructure availability
   - *Mitigation*: Use mock APIs and data during frontend development
   - Parallel development tracks for frontend and backend
   - Define API contracts early with clear documentation
   - Incremental integration testing as components become available

---

## 12\. Open Questions

1. Should candidates be able to apply to multiple jobs with the same profile?
2. What is the preferred design aesthetic/brand guidelines?
3. Are there specific certifications or licenses commonly required?
4. What level of customization do agency staff need for the one-page profile template?
5. Should candidates be notified when their status changes?
6. What are the data retention and deletion requirements?

---

## 13\. Development and Project Cycle

### 13.1 Discovery & Planning (Pre-Development)

**Duration:** 1-2 days

**Activities:**
- Short alignment catch-up sessions (1-2 meetings) to finalize MVP priorities
- Define "demo-ready" acceptance criteria for Phase 0
- Produce feature prioritization matrix:
  - **Must-Have**: Critical for investor demo (job posting, application, profile generation)
  - **Should-Have**: Enhances experience but not blocking (advanced filters, bulk actions)
  - **Can-Wait**: Future phases (analytics, integrations, candidate portal)
- Brief technical discovery to assess any reusable business logic from legacy system
- Define demo objectives and success metrics
- Set up project tracking and communication channels

**Deliverables:**
- Feature prioritization matrix
- Phase 0 acceptance criteria document
- Technical discovery summary (if applicable)
- Sprint planning calendar

### 13.2 Incremental Delivery & Demos

**Approach:** Bi-weekly sprint model

**Sprint Structure:**
- **Sprint Duration:** 2 weeks
- **Sprint Planning:** Define specific deliverables and acceptance criteria
- **Daily Standups:** Brief progress updates and blocker identification
- **Mid-Sprint Check-in:** Review progress and adjust scope if needed
- **Sprint Demo:** Present completed, testable functionality to stakeholders
- **Sprint Retrospective:** Identify improvements for next sprint

**Demo Environment:**
- Maintain a running demo environment separate from development
- Populate with realistic sample data for presentations
- Deploy tested features at end of each sprint
- Ensure environment is always demo-ready for unexpected investor meetings

**Weekly Deliverables Breakdown (Phase 0 - 4 weeks):**

**Week 1:**
- Basic authentication and user management
- Job posting creation interface
- Database schema setup
- Initial UI framework

**Week 2:**
- Job listing display (public-facing)
- Candidate application form (Part 1: Personal & Professional info)
- Document upload functionality
- Form validation

**Week 3:**
- Complete application workflow
- One-page profile generation (basic template)
- Agency portal candidate list view
- Application submission and confirmation

**Week 4:**
- UI/UX polish and refinements
- Bug fixes and testing
- Demo environment setup with sample data
- UAT and stakeholder review
- Investor presentation preparation

### 13.3 Quality Assurance & User Acceptance Testing

**Approach:** Parallel testing during development

**Testing Strategy:**
- **Unit Testing:** Automated tests for critical functions (profile generation, file upload)
- **Integration Testing:** API and database interactions
- **UAT Cycle:** End of Phase 0 focused on investor demo scenarios
- **Test Scenarios:**
  - Complete job-to-application workflow
  - Profile generation accuracy and formatting
  - Document upload and retrieval
  - Agency portal navigation and usability
  - Performance under limited load (50 concurrent users)

**UAT Focus Areas:**
- Clean navigation and intuitive interface
- No critical bugs in demo scenarios
- Smooth performance during presentations
- Professional appearance and branding
- Data accuracy in generated profiles

**Bug Classification:**
- **Critical:** Blocks demo scenarios, must fix immediately
- **High:** Impacts user experience, fix before UAT completion
- **Medium:** Minor issues, document for Phase 1
- **Low:** Cosmetic issues, address in future phases

### 13.4 Documentation & Handover

**Technical Documentation (End of Each Phase):**
- Deployment notes and environment configuration
- API documentation (Swagger/OpenAPI format)
- Database schema and migrations
- Third-party service integration guides
- Known issues and workarounds
- Future enhancement recommendations

**Handover Package Contents:**
- Source code repository access and structure
- Environment setup instructions
- Admin credentials and access management guide
- Demo data seeding scripts
- Monitoring and logging setup
- Backup and recovery procedures
- Scalability considerations for next phases

**Knowledge Transfer:**
- Technical walkthrough session with stakeholders
- Documentation of architectural decisions
- Code commenting and README files
- Troubleshooting guide for common issues

### 13.5 Communication and Reporting

**Weekly Status Reports:**
- Completed deliverables vs. planned
- Current sprint progress
- Blockers and risks
- Upcoming milestones
- Resource needs

**Stakeholder Communication:**
- Weekly progress demonstrations (recorded for asynchronous viewing)
- Bi-weekly sprint reviews with live demos
- Slack/Teams channel for real-time updates
- Monthly executive summary for business stakeholders

**Progress Tracking:**
- Jira/Trello board with sprint backlog
- Burndown charts for sprint velocity
- Feature completion percentage
- Demo environment uptime and readiness status

---
