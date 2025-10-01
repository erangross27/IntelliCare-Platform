# IntelliCare Platform - Feature Reference Guide

## AI Medical Assistant Features

### Natural Language Medical Conversations
- **Service**: `agentServiceV4.js` (1.2MB main orchestrator)
- **Route**: `/api/agent/chat`
- **Features**:
  - 2000+ medical functions (actively expanding)
  - Symptom analysis
  - Medical Q&A
  - Treatment suggestions
  - Drug information
  - Emergency detection
  - Semantic function selection (99.6% token reduction)

### Document Intelligence
- **Services**: `claudeBatchProcessor.js`, `documentAnalysisService.js`
- **Routes**: `/api/documents/analyze`, `/api/agent/batch-analyze`
- **Features**:
  - Batch document processing (50% cost savings)
  - Medical document OCR via Claude Sonnet 4.5
  - Lab result interpretation
  - Prescription reading
  - Medical image analysis
  - Report generation
  - Extraction of 245+ medical data types

## Patient Management Features

### Patient Records
- **Services**: `patientDataService.js`, `medicalDataService.js`
- **Routes**: `/api/patients/*`
- **Features**:
  - Complete medical history
  - Document storage (245+ collection types)
  - Timeline view
  - Family history
  - Allergy tracking
  - Multi-tenant isolation

### Appointment System
- **Service**: `appointments.js`
- **Route**: `/api/appointments`
- **Features**:
  - Online scheduling
  - Calendar integration
  - Reminder system via SMS/Email
  - Video consultations
  - Queue management

## Clinical Tools

### Diagnosis Support
- **Service**: `clinicalDecisionSupport.js`
- **Route**: `/api/diagnosis`
- **Features**:
  - ICD-10 coding
  - Differential diagnosis
  - Clinical guidelines
  - Evidence-based recommendations
  - Risk assessment via AI

### Prescription Management
- **Service**: `prescriptionGenerator.js`
- **Route**: `/api/prescriptions`
- **Features**:
  - E-prescribing
  - Drug interaction checking
  - Dosage calculations
  - Refill management
  - Pharmacy integration

### Lab Integration
- **Service**: `labResultInterpreter.js`
- **Component**: `LabResultsCard.js`
- **Features**:
  - Result interpretation via AI
  - Trend analysis
  - Abnormal value alerts
  - Historical comparison
  - Reference ranges

## Security & Compliance

### HIPAA Compliance
- **Service**: `hipaaComplianceService.js`
- **Features**:
  - PHI encryption (AES-256)
  - Access controls via SecureDataAccess
  - Audit logging (every operation tracked)
  - Data retention policies
  - Breach detection

### Authentication Systems
- **Services**:
  - `secureSessionManager.js` - Server-side sessions
  - `serviceAccountManager.js` - Service authentication
  - `secureDataAccess.js` - Data access control
- **Features**:
  - OTP-based authentication
  - Session management (httpOnly cookies)
  - Multi-tenant isolation (SACRED rule)
  - Service account authentication via KMS
  - Zero-trust architecture

## Healthcare Integrations

### Insurance Processing
- **Service**: `insuranceService.js`
- **Route**: `/api/insurance`
- **Features**:
  - Eligibility verification
  - Prior authorization
  - Claims processing
  - Copay calculation
  - Coverage checking

### Medicare/Medicaid
- **Services**:
  - `medicareQualityService.js`
  - `medicaidChipService.js`
- **Features**:
  - Beneficiary lookup via Blue Button
  - Coverage verification
  - Quality reporting
  - MIPS compliance
  - Cost estimates

### Pharmacy Integration
- **Service**: `formularyService.js`
- **Features**:
  - Drug formulary lookup
  - Generic substitution
  - Prior auth requirements
  - Mail-order pharmacy
  - Medication sync

## Analytics & Reporting

### Clinical Analytics
- **Service**: `clinicalAnalyticsService.js`
- **Component**: `DashboardComponents/*`
- **Features**:
  - Population health metrics
  - Quality measures
  - Outcome tracking
  - Risk stratification
  - Predictive modeling

### Business Intelligence
- **Service**: `businessIntelligenceDashboardService.js`
- **Features**:
  - Revenue cycle analytics
  - Operational metrics
  - Provider productivity
  - Patient satisfaction
  - Financial reporting

## Communication Tools

### Patient Portal
- **Service**: `patientPortalMessagingService.js`
- **Component**: `ChatAuthAI.js`
- **Features**:
  - Secure messaging
  - Video consultations
  - Document sharing
  - Appointment requests
  - Prescription refills

### Notifications
- **Services**:
  - `smsService.js` - SMS notifications (Twilio)
  - `emailService.js` - Email alerts (SendGrid)
  - `reminderService.js` - Appointment reminders
- **Features**:
  - Multi-channel delivery
  - Template management
  - Preference settings
  - Delivery tracking
  - Unsubscribe handling

## Advanced AI Features

### Semantic Function Selection
- **Service**: `semanticFunctionSelector.js`, `claudeTwoStageSelector.js`
- **Features**:
  - 2000+ functions (actively expanding)
  - 99.6% token reduction (1,352→10 functions)
  - Two-stage selection process
  - O(1) registry lookup
  - Context-aware function selection

### Self-Improving Memory
- **Service**: `selfImprovingMemory.js`
- **Features**:
  - Learning from interactions
  - Pattern recognition
  - Preference tracking
  - Context retention
  - Personalization

### Predictive Analytics
- **Service**: `predictiveAnalyticsAIService.js`
- **Features**:
  - Risk prediction
  - Readmission prevention
  - Disease progression
  - Treatment outcomes
  - Cost forecasting

### Emergency Detection
- **Service**: `emergencyProtocolDetector.js`
- **Features**:
  - Symptom severity assessment
  - Automatic escalation
  - Emergency contact alerts
  - Location services
  - Protocol activation

## Workflow Automation

### Clinical Workflows
- **Service**: `workflowEngine.js`
- **Features**:
  - Care pathways
  - Protocol automation
  - Task management
  - Alert routing
  - Escalation rules

### Batch Document Processing
- **Service**: `claudeBatchProcessor.js`
- **Features**:
  - Bulk upload processing
  - 50% cost savings vs single document
  - Auto-classification
  - Data extraction to 245+ collections
  - Quality checking
  - Archive management

## Medical Data Collections (245+)

### Primary Medical Collections
```
diagnoses, medications, lab_results, vital_signs,
allergies, immunizations, procedures, hospitalizations,
radiology_results, pathology_results, discharge_plans,
surgical_history, medication_history, problem_list,
care_plans, advance_directives, social_history,
family_history, preventive_care, referrals,
consultations, progress_notes, operative_reports,
[... 225+ more specialized collections]
```

### Managed by
- **Service**: `medicalCollectionsService.js`
- **Database**: Multi-tenant MongoDB (separate DB per practice)

## API Endpoints Summary

### Core Medical APIs
```
POST   /api/agent/chat                    - AI medical chat
POST   /api/agent/batch-analyze           - Batch document analysis
GET    /api/agent/batch-progress          - Batch progress status
POST   /api/agent/analyze                 - Single document analysis
GET    /api/patients                      - List patients
POST   /api/patients                      - Create patient
GET    /api/patients/:id                  - Get patient details
PUT    /api/patients/:id                  - Update patient
POST   /api/documents/upload              - Upload document
POST   /api/documents/analyze             - Analyze document
GET    /api/appointments                  - List appointments
POST   /api/appointments                  - Schedule appointment
POST   /api/diagnosis/suggest             - Diagnosis suggestions
POST   /api/prescriptions/generate        - Generate prescription
```

### Authentication APIs
```
POST   /api/auth/send-otp                 - Send OTP code
POST   /api/auth/verify-otp               - Verify OTP
POST   /api/auth/refresh                  - Refresh session
POST   /api/auth/logout                   - User logout
```

### Administrative APIs
```
GET    /api/clinics                       - List clinics (practice tenants)
POST   /api/clinics                       - Create clinic
GET    /api/users                         - List users
POST   /api/users                         - Create user
GET    /api/analytics/dashboard           - Analytics data
GET    /api/reports/generate              - Generate reports
GET    /api/audit-logs                    - Audit trail access
```

## Component Library (100+)

### Essential React Components
1. **ChatAuthAI.js** - AI-powered chat interface
2. **PatientDetail.js** - Comprehensive patient view
3. **PatientList.js** - Patient listing with search
4. **Diagnosis.js** - Diagnosis interface
5. **DocumentViewer.js** - Document display
6. **MedicalHistoryModal.js** - Medical history
7. **VitalSignsCard.js** - Vital signs display
8. **LabResultsCard.js** - Lab results
9. **MedicationsCard.js** - Medications list
10. **AppointmentsCard.js** - Appointments
11. **UserManagement.js** - User admin
12. **ClinicManagementDashboard.js** - Clinic admin
13. **NotificationCenter.js** - Real-time notifications

## Database Structure

### Primary Collections
- **Global Database** (`intellicare_practice_global`):
  - `ServiceAccounts` - Service authentication
  - `Practices` - Multi-tenant practice records
  - Global configuration

- **Practice Databases** (`intellicare_practice_{subdomain}`):
  - `users` - User accounts (per practice)
  - `patients` - Patient records
  - `appointments` - Scheduled appointments
  - `documents` - Medical documents
  - `chat_messages` - AI chat history
  - `prescriptions` - Prescription records
  - `diagnoses` - Diagnosis history
  - `auditLogs` - System audit trail
  - 245+ medical data collections

## Quick Start Commands

```bash
# Backend development
cd apps/backend-api
npm run dev                # Port 5000 (auto-restart via nodemon)

# Frontend development
cd apps/frontend-vite
npm run dev                # Port 3000 (Vite HMR)

# Log monitoring
tail -f apps/backend-api/logs/server-errors.log

# Database backup
node scripts/backupDatabase.js

# Verify data extraction
node scripts/verifyDataExtractionAutoWithCache.js --no-cache
```

## Critical Rules

### Development Rules
1. **NEVER kill NPM processes** - Destroys user sessions
2. **Backend auto-restarts** - Via nodemon, don't restart manually
3. **Frontend auto-refreshes** - Via Vite HMR
4. **Database access** - ALL operations through SecureDataAccess
5. **Multi-tenant isolation** - SACRED rule, never bypass

### Security Rules
```javascript
// ONLY these methods exist:
await SecureDataAccess.query(collection, filter, options, context)
await SecureDataAccess.insert(collection, document, context)
await SecureDataAccess.update(collection, filter, updates, context)
await SecureDataAccess.delete(collection, filter, context, options)
await SecureDataAccess.aggregate(collection, pipeline, context)

// Context REQUIRED:
const context = {
  serviceId: 'service-name',
  operation: 'operation-name',
  practiceId: req.practice?.id || 'global'
};
```

## Support Resources

- **Project Location**: `/home/erangross/Development/IntelliCare`
- **Backend**: `apps/backend-api/`
- **Frontend**: `apps/frontend-vite/`
- **Documentation**: `docs/` directory
- **Scripts**: `scripts/` directory
- **Production Domain**: `intellicare.health`

## AI Model Configuration

### Claude Sonnet 4.5
- **Model ID**: `claude-sonnet-4-5-20250929`
- **Use Case**: Medical document extraction and analysis
- **Batch Processing**: 50% cost savings
- **Pricing**: $3/1M input, $15/1M output tokens

### Claude Haiku
- **Use Case**: Real-time function execution
- **Performance**: <1s response time
- **Use Case**: Interactive chat operations

This reference guide provides quick access to all major features and components of the IntelliCare platform based on the actual implementation.
