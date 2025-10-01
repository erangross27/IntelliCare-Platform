# IntelliCare Platform Architecture

<div align="center">
  <img src="https://img.shields.io/badge/Enterprise%20Architecture-blue?style=for-the-badge" alt="Enterprise Architecture"/>
  <img src="https://img.shields.io/badge/HIPAA%20Compliant-green?style=for-the-badge" alt="HIPAA Compliant"/>
  <img src="https://img.shields.io/badge/High%20Performance-orange?style=for-the-badge" alt="High Performance"/>
</div>

---

## **Architecture Overview**

IntelliCare employs an **enterprise medical platform architecture** combining traditional healthcare management with advanced AI capabilities. The system uses Claude Sonnet 4.5 for medical document analysis and Claude Haiku for real-time function execution, ensuring both accuracy and performance.

## **System Architecture**

### **Core Components**
1. **Frontend Layer** - React (Vite) with 100+ components
2. **Backend API** - Node.js + Express with 200+ microservices
3. **AI Processing** - Claude Sonnet 4.5 + Haiku integration
4. **Database Layer** - MongoDB with multi-tenant isolation
5. **Security Layer** - Zero-trust architecture with KMS

---

## **Architecture Principles**

### **Design Philosophy**
- **Multi-Tenant Isolation** - Complete data separation per clinic
- **AI-Enhanced Operations** - 2000+ medical functions (actively expanding)
- **Security First** - HIPAA compliance and zero-trust architecture
- **Scalable Design** - Microservices architecture for horizontal scaling
- **Performance Optimized** - Redis caching with 95% hit rate

### **Security Principles**
- **Zero Trust Architecture** - Verify every request, trust nothing by default
- **Defense in Depth** - Multiple security layers throughout the system
- **Least Privilege Access** - Minimal required permissions for each component
- **Data Encryption** - Encryption at rest and in transit
- **Audit Trail** - Comprehensive logging of all system activities

---

## **Frontend Architecture**

### **Client Applications**

#### **Web Application (React + Vite)**
- **Framework:** React 18+ with Vite
- **UI Library:** 100+ custom medical components
- **State Management:** React Context API
- **Styling:** Tailwind CSS
- **Real-time:** WebSocket client for live updates
- **Internationalization:** Hebrew + English with RTL support

#### **Progressive Web App (PWA)**
- **Service Workers** - Offline functionality and caching
- **Responsive Design** - Mobile-first approach
- **Push Notifications** - Real-time diagnostic alerts
- **Local Storage** - Secure client-side data persistence

### **Application Structure**
```
frontend-vite/src/
├── components/           # React UI components (100+)
├── services/            # API service layer
├── context/             # React context providers
├── hooks/               # Custom React hooks
├── pages/               # Page components
└── utils/               # Frontend utilities
```

---

## **Backend Architecture**

### **API Gateway & Load Balancing**

#### **Load Balancer (Nginx)**
- **SSL Termination** - Handles HTTPS certificates
- **Request Routing** - Intelligent request distribution
- **Rate Limiting** - DDoS protection and API throttling
- **Health Checks** - Automatic unhealthy instance removal

#### **WebSocket Gateway**
- **Real-time Updates** - Batch processing progress notifications
- **Database Polling** - Progress tracking via MongoDB
- **Session Management** - Secure session persistence
- **Notification System** - Real-time alerts and updates

### **Core Services** (200+)

#### **AI Services**
```
- agentServiceV4.js (1.2MB) - Main AI orchestration
- agentServiceClaude.js - Claude API integration
- claudeBatchProcessor.js - Batch document processing
- documentAnalysisService.js - Document analysis
- semanticFunctionSelector.js - Function selection (99.6% token reduction)
```

#### **Authentication & Security**
```
- secureDataAccess.js - All database operations MUST use this
- secureSessionManager.js - Server-side session management
- serviceAccountManager.js - Service authentication
- hipaaComplianceService.js - HIPAA compliance enforcement
```

#### **Medical Data Services**
```
- medicalDataService.js - Medical data management
- medicalCollectionsService.js - 245+ medical collections
- patientDataService.js - Patient operations
- clinicalDecisionSupport.js - Clinical support
```

---

## **AI Processing Architecture**

### **AI Model Configuration**

#### **Claude Sonnet 4.5** (`claude-sonnet-4-5-20250929`)
- **Primary Use:** Medical document extraction and analysis
- **Batch Processing:** 50% cost savings via batch API
- **Pricing:** $3 per 1M input tokens, $15 per 1M output tokens
- **Features:** High accuracy medical document analysis

#### **Claude Haiku**
- **Primary Use:** Real-time function execution
- **Features:** Fast response times for interactive operations
- **Performance:** <1s response time for most operations

### **Function Selection System**

#### **Semantic Selection**
- **Total Functions:** 2000+ (actively expanding)
- **Selection Method:** AI-powered semantic matching
- **Token Reduction:** 99.6% (1,352→10 functions)
- **Performance:** <1ms lookup time (O(1) registry)

#### **Two-Stage Selection** (`claudeTwoStageSelector.js`)
1. **Stage 1:** Semantic filtering to relevant subset
2. **Stage 2:** AI selection of specific functions
3. **Result:** Minimal token usage with maximum relevance

---

## **Data Architecture**

### **Database Design**

#### **MongoDB Multi-Tenant Architecture**
```
Global Database: intellicare_practice_global
├── ServiceAccounts
├── Practices
└── Global Configuration

Practice Databases: intellicare_practice_{subdomain}
├── Users
├── Patients
├── Appointments
├── Documents
├── Chat Messages
├── Medical Data (245+ collections)
└── Audit Logs
```

#### **Security Configuration**
- **Authentication:** Required for ALL connections
- **Localhost Bypass:** DISABLED (`enableLocalhostAuthBypass: 0`)
- **Credentials:** Stored in KMS only
- **Connection String:** `mongodb://intellicare_app:[PASSWORD]@localhost:27017/`

### **Medical Data Collections** (245+)
```
diagnoses, medications, lab_results, vital_signs,
allergies, immunizations, procedures, hospitalizations,
radiology_results, pathology_results, discharge_plans,
[... 235+ more collections]
```

### **Caching Strategy**

#### **Redis Implementation**
- **Active Sessions** - User session data
- **Function Registry** - O(1) instant lookup
- **Medical Data** - Frequently accessed records
- **Performance:** 95% cache hit rate target
- **Auto-Invalidation** - Smart cache invalidation on updates

---

## **Security Architecture**

### **Multi-Layer Security**

#### **Network Security**
```yaml
Security Layers:
  1. WAF (Web Application Firewall)
  2. DDoS Protection
  3. SSL/TLS Encryption (TLS 1.3)
  4. VPN Access for Admin Functions
  5. Network Segmentation
```

#### **Application Security**
```yaml
Application Security:
  Authentication:
    - Server-side sessions (httpOnly cookies)
    - OTP-based authentication
    - No magic links (same tab only)
    - Service account authentication via KMS

  Authorization:
    - Multi-tenant isolation (SACRED rule)
    - SecureDataAccess for ALL database operations
    - Service-level authentication
    - Practice-specific data access

  Data Protection:
    - AES-256 encryption at rest
    - TLS 1.3 encryption in transit
    - Field-level encryption for PII
    - Data anonymization for analytics
```

### **HIPAA Compliance Architecture**
- **Administrative Safeguards** - Security officer, training, access management
- **Physical Safeguards** - Facility controls, workstation security
- **Technical Safeguards** - Access control, audit controls, integrity, transmission security
- **Audit Trails** - Complete logging via `auditLogService.js`

#### **Critical Security Rules**
```javascript
// ONLY these methods exist for database access:
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

---

## **Performance Architecture**

### **Scalability Design**

#### **Horizontal Scaling**
```yaml
Scaling Strategy:
  Load Balancing:
    - Nginx reverse proxy
    - Round-robin distribution
    - Health check monitoring
    - Automatic failover

  Microservices:
    - 200+ independent services
    - Service auto-registration
    - Container orchestration ready
    - Independent scaling per service

  Database:
    - MongoDB replica sets
    - Read/write separation
    - Sharding for large datasets
```

#### **Performance Optimization**
- **Semantic Function Selection** - 99.6% token reduction
- **Redis Caching** - 95% hit rate target
- **Batch Document Processing** - 50% cost savings
- **Database Indexing** - Optimized query performance
- **Connection Pooling** - Efficient database connection management

### **Monitoring & Observability**

#### **System Monitoring**
```yaml
Monitoring Stack:
  Logs:
    - Centralized logging (lnav for analysis)
    - Structured logging (JSON format)
    - Error tracking and alerting

  Metrics:
    - Application performance metrics
    - Database performance monitoring
    - AI model performance tracking

  Tools:
    - lnav (log analysis)
    - ripgrep (code search)
    - htop (system monitoring)
    - MongoDB MCP tools (database inspection)
```

---

## **Deployment Architecture**

### **Production Environment**

#### **Infrastructure**
```yaml
Infrastructure:
  Domain: intellicare.health

  Services:
    - Backend API (Port 5000)
    - Frontend (Port 3000)
    - MongoDB (Port 27017)
    - Redis (Port 6379)

  Security:
    - SSL/TLS certificates
    - KMS key management
    - Service account authentication
    - Multi-tenant database isolation
```

#### **Service Management**
- **Backend:** Auto-restarts via nodemon
- **Frontend:** Auto-refreshes via Vite HMR
- **MongoDB:** Authenticated connections only
- **Redis:** Session and cache management

---

## **Integration Architecture**

### **External Integrations**

#### **AI Services**
- **Anthropic Claude** - Primary AI provider
- **Batch API** - Document processing
- **Streaming API** - Real-time chat responses

#### **Third-Party Services**
```yaml
Integrations:
  Communication:
    - SendGrid (email)
    - Twilio (SMS)

  Healthcare:
    - Blue Button (Medicare data)
    - FHIR-compliant APIs
    - Lab integration systems

  Security:
    - Google KMS (key management)
    - Audit trail services
```

---

## **Development Architecture**

### **Development Workflow**
```bash
# Backend development
cd apps/backend-api && npm run dev      # Port 5000

# Frontend development
cd apps/frontend-vite && npm run dev    # Port 3000

# Log monitoring
tail -f apps/backend-api/logs/server-errors.log
```

### **Critical Development Rules**
1. **NPM processes** - NEVER KILL (destroys user sessions)
2. **Backend** - Auto-restarts via nodemon
3. **Frontend** - Auto-refreshes via Vite HMR
4. **Testing** - Through GUI only, not scripts
5. **Database Access** - ALL operations through SecureDataAccess

---

## **Future Architecture Considerations**

### **Scalability Roadmap**
- **Edge Computing** - Regional AI processing for reduced latency
- **Advanced Analytics** - Machine learning for population health insights
- **IoT Device Integration** - Real-time patient monitoring data
- **Function Expansion** - Target 5000+ medical functions

### **Global Expansion Architecture**
- **Multi-Region Deployment** - Global infrastructure presence
- **Localization Framework** - Additional language support
- **Regulatory Compliance** - Region-specific healthcare regulations
- **Data Residency** - Local data storage requirements compliance

---

<div align="center">

  **IntelliCare Architecture - Enterprise Medical AI Platform**

  *Built on Claude Sonnet 4.5 for intelligent healthcare management*

</div>
