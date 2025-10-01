# IntelliCare Platform - Technical Implementation Guide

## Project Structure

### Actual Repository Structure
```
/home/erangross/Development/IntelliCare/
├── apps/
│   ├── backend-api/
│   │   ├── services/          (200+ service files)
│   │   ├── routes/            (70+ route files)
│   │   ├── models/            (Database models)
│   │   ├── middleware/        (Express middleware)
│   │   ├── utils/             (Utility functions)
│   │   ├── config/            (Configuration files)
│   │   ├── scripts/           (Utility scripts)
│   │   ├── logs/              (Application logs)
│   │   └── server.js          (Main server file)
│   └── frontend-vite/
│       └── src/
│           ├── components/    (100+ React components)
│           ├── services/      (Frontend services)
│           ├── context/       (React contexts)
│           ├── hooks/         (Custom hooks)
│           ├── pages/         (Page components)
│           └── utils/         (Frontend utilities)
├── libs/                      (Shared libraries)
├── docs/                      (Documentation)
└── scripts/                   (Build & deployment scripts)
```

## Key Services Implementation

### Priority 1: Core AI Services
```javascript
// Essential AI service files
apps/backend-api/services/
├── agentServiceV4.js (1.2MB)          - Main AI orchestration
├── agentServiceClaude.js              - Claude Sonnet 4.5 integration
├── claudeBatchProcessor.js            - Batch document processing
├── documentAnalysisService.js         - Document analysis
├── semanticFunctionSelector.js        - Function selection (99.6% reduction)
├── claudeTwoStageSelector.js          - Two-stage selection
└── generatedMedicalFunctions.js       - 2000+ medical operations
```

### Priority 2: Authentication & Security
```javascript
// Critical security services
apps/backend-api/services/
├── secureDataAccess.js                - ALL database operations
├── secureSessionManager.js            - Server-side sessions
├── serviceAccountManager.js           - Service authentication
├── hipaaComplianceService.js          - HIPAA compliance
└── auditLogService.js                 - Complete audit trails
```

### Priority 3: Medical Operations
```javascript
// Core medical functionality
apps/backend-api/services/
├── medicalDataService.js              - Medical data management
├── medicalCollectionsService.js       - 245+ collections
├── patientDataService.js              - Patient operations
├── clinicalDecisionSupport.js         - Clinical support
└── prescriptionGenerator.js           - Prescription management
```

## API Routes Implementation

### Core Route Structure
```javascript
// Main API route files
apps/backend-api/routes/
├── agent.js                 - AI interactions (/api/agent/*)
├── auth.js                  - Authentication (/api/auth/*)
├── patients.js              - Patient management (/api/patients/*)
├── documents.js             - Document handling (/api/documents/*)
├── appointments.js          - Scheduling (/api/appointments/*)
├── clinics.js               - Clinic management (/api/clinics/*)
└── medical.js               - Medical operations (/api/medical/*)
```

## Frontend Components Implementation

### Essential Components
```javascript
// Priority components
apps/frontend-vite/src/components/
├── ChatAuthAI.js                      - AI chat interface
├── PatientDetail.js                   - Patient management
├── PatientList.js                     - Patient listing
├── Diagnosis.js                       - Diagnosis interface
├── DocumentViewer.js                  - Document viewing
├── MedicalHistoryModal.js             - Medical history
├── UserManagement.js                  - User administration
├── ClinicManagementDashboard.js       - Clinic dashboard
└── NotificationCenter.js              - Real-time notifications
```

## Database Configuration

### MongoDB Multi-Tenant Setup
```javascript
// Database structure
Global Database: intellicare_practice_global
├── ServiceAccounts                     - Service authentication
├── Practices                           - Tenant records
└── Global Configuration

Practice Databases: intellicare_practice_{subdomain}
├── users                               - User accounts (per practice)
├── patients                            - Patient records
├── appointments                        - Scheduled appointments
├── documents                           - Medical documents
├── chat_messages                       - AI chat history
├── prescriptions                       - Prescription records
├── diagnoses                           - Diagnosis history
├── auditLogs                           - System audit trail
└── [245+ medical data collections]

// Essential collections
const collections = {
  patients: {
    indexes: ['patientId', 'practiceId', 'email'],
    sharding: { key: 'practiceId' }
  },
  documents: {
    indexes: ['patientId', 'type', 'uploadDate'],
    ttl: { field: 'expiryDate' }
  },
  appointments: {
    indexes: ['patientId', 'providerId', 'date'],
    compound: [['practiceId', 'date']]
  },
  auditLogs: {
    indexes: ['userId', 'action', 'timestamp'],
    capped: { size: 10737418240 } // 10GB
  }
};
```

### Database Security Configuration
```javascript
// MongoDB authentication configuration
{
  security: {
    authorization: "enabled",
    enableLocalhostAuthBypass: 0  // CRITICAL: Localhost bypass DISABLED
  },
  net: {
    port: 27017,
    bindIp: "127.0.0.1"
  }
}

// Application connection
const connectionString =
  "mongodb://intellicare_app:[PASSWORD]@localhost:27017/intellicare_practice_global?authSource=admin";
```

## Service Dependencies

### NPM Packages Required
```json
{
  "dependencies": {
    "@anthropic-ai/sdk": "^0.57.0",
    "mongodb": "^6.11.0",
    "express": "^5.0.1",
    "jsonwebtoken": "^9.0.2",
    "bcrypt": "^6.0.0",
    "multer": "^1.4.5-lts.1",
    "socket.io": "^4.8.1",
    "redis": "^4.7.0",
    "node-cron": "^3.0.3",
    "winston": "^3.17.0",
    "helmet": "^8.0.0",
    "cors": "^2.8.5",
    "express-rate-limit": "^8.0.0",
    "express-validator": "^7.2.0"
  }
}
```

## Environment Setup

### Development Environment
```bash
# Backend .env file (apps/backend-api/.env)
NODE_ENV=development
PORT=5000
MONGODB_URI=mongodb://intellicare_app:[PASSWORD]@localhost:27017/intellicare_practice_global?authSource=admin
REDIS_URL=redis://localhost:6379
ENCRYPTION_KEY=32_byte_encryption_key_here
SESSION_SECRET=session_secret_key

# API keys stored in KMS only (never in .env):
# - ANTHROPIC_API_KEY
# - SENDGRID_API_KEY
# - TWILIO_ACCOUNT_SID
# - TWILIO_AUTH_TOKEN
```

### Production Environment
```bash
# Production configuration
NODE_ENV=production
PORT=5000
DOMAIN=intellicare.health
MONGODB_URI=mongodb://intellicare_app:[PASSWORD]@localhost:27017/
REDIS_URL=redis://localhost:6379
USE_SSL=true
```

## Implementation Guide

### Step 1: Clone and Setup
```bash
# Clone repository (private)
git clone [private-repo-url] IntelliCare
cd IntelliCare

# Install backend dependencies
cd apps/backend-api
npm install

# Install frontend dependencies
cd ../frontend-vite
npm install
```

### Step 2: Configure Database
```bash
# Ensure MongoDB is running with authentication
sudo systemctl status mongod

# Database credentials are in KMS
# Connection managed by secureDataAccess.js
```

### Step 3: Start Development Servers
```bash
# Terminal 1: Backend (auto-restart via nodemon)
cd apps/backend-api
npm run dev                # Port 5000

# Terminal 2: Frontend (Vite HMR)
cd apps/frontend-vite
npm run dev                # Port 3000

# Terminal 3: Log monitoring
tail -f apps/backend-api/logs/server-errors.log
```

### Step 4: Main Server Implementation
```javascript
// apps/backend-api/server.js
const express = require('express');
const cors = require('cors');
const helmet = require('helmet');
const { initializeDatabase } = require('./config/database');
const { loadServices } = require('./services/masterServiceLoader');
const { setupRoutes } = require('./routes/routeLoaderService');

const app = express();

// Middleware
app.use(helmet());
app.use(cors({
  origin: process.env.ALLOWED_ORIGINS?.split(','),
  credentials: true
}));
app.use(express.json({ limit: '50mb' }));

// Initialize services
const startServer = async () => {
  const db = await initializeDatabase();
  await loadServices(db);
  setupRoutes(app, db);

  const port = process.env.PORT || 5000;
  app.listen(port, () => {
    console.log(`IntelliCare Platform running on port ${port}`);
  });
};

startServer();
```

## Testing Implementation

### Unit Test Example
```javascript
// apps/backend-api/tests/services/medicalDataService.test.js
const { expect } = require('chai');
const medicalDataService = require('../../services/medicalDataService');

describe('Medical Data Service', () => {
  it('should create patient record', async () => {
    const patient = await medicalDataService.createPatient({
      name: 'Test Patient',
      dateOfBirth: '1990-01-01'
    });
    expect(patient).to.have.property('patientId');
  });
});
```

### Integration Test Example
```javascript
// apps/backend-api/tests/integration/patient-flow.test.js
describe('Patient Management Flow', () => {
  it('should complete patient registration flow', async () => {
    // Create patient
    const patient = await api.post('/api/patients', patientData);

    // Upload document
    const document = await api.post(`/api/documents/${patient.id}`, documentData);

    // Schedule appointment
    const appointment = await api.post('/api/appointments', {
      patientId: patient.id,
      date: '2024-01-15'
    });

    expect(appointment.status).to.equal('scheduled');
  });
});
```

## Performance Optimization

### Caching Strategy
```javascript
// services/cacheService.js
const redis = require('redis');
const client = redis.createClient({ url: process.env.REDIS_URL });

const cacheService = {
  async get(key) {
    return await client.get(key);
  },

  async set(key, value, ttl = 3600) {
    await client.setEx(key, ttl, JSON.stringify(value));
  },

  async invalidate(pattern) {
    const keys = await client.keys(pattern);
    if (keys.length) await client.del(keys);
  }
};
```

### Database Optimization
```javascript
// Connection pooling
const mongoOptions = {
  maxPoolSize: 100,
  minPoolSize: 10,
  maxIdleTimeMS: 30000,
  serverSelectionTimeoutMS: 5000
};

// Use aggregation pipelines for complex queries
const getPatientStats = async (practiceId) => {
  return await SecureDataAccess.aggregate('patients', [
    { $match: { practiceId } },
    { $group: {
      _id: '$status',
      count: { $sum: 1 }
    }}
  ], context);
};
```

## Security Implementation

### API Security Middleware
```javascript
// middleware/security.js
const rateLimit = require('express-rate-limit');
const helmet = require('helmet');

const securityMiddleware = [
  helmet(),
  rateLimit({
    windowMs: 15 * 60 * 1000, // 15 minutes
    max: 100 // limit each IP to 100 requests per windowMs
  }),
  // CORS configuration
  cors({
    origin: process.env.ALLOWED_ORIGINS?.split(','),
    credentials: true
  })
];
```

### Critical Security Rules
```javascript
// ALL database operations MUST use SecureDataAccess
// NEVER use direct MongoDB operations

// CORRECT:
await SecureDataAccess.query('patients', { id: patientId }, {}, context);

// FORBIDDEN:
await Patient.findById(patientId);  // NEVER DO THIS
```

## Monitoring & Logging

### Application Monitoring
```javascript
// services/loggingService.js
const winston = require('winston');

const logger = winston.createLogger({
  level: 'info',
  format: winston.format.json(),
  transports: [
    new winston.transports.File({ filename: 'logs/error.log', level: 'error' }),
    new winston.transports.File({ filename: 'logs/combined.log' })
  ]
});

// Track API performance
app.use((req, res, next) => {
  const start = Date.now();
  res.on('finish', () => {
    const duration = Date.now() - start;
    logger.info({
      method: req.method,
      path: req.path,
      status: res.statusCode,
      duration
    });
  });
  next();
});
```

### Log Analysis Tools
```bash
# Use lnav for intelligent log analysis
lnav apps/backend-api/logs/

# Inside lnav:
:filter-in error|warn
;SELECT log_msg, COUNT(*) FROM logs WHERE log_level='ERROR' GROUP BY log_msg

# Use ripgrep for code search
rg "SecureDataAccess" --type js

# Use htop for system monitoring
htop
```

## Deployment

### Development Deployment
```bash
# Start all services
cd /home/erangross/Development/IntelliCare

# Backend (Terminal 1)
cd apps/backend-api && npm run dev

# Frontend (Terminal 2)
cd apps/frontend-vite && npm run dev
```

### Production Deployment
```bash
# Production deployment on intellicare.health
# Services managed via systemd or PM2
pm2 start apps/backend-api/server.js --name intellicare-backend
pm2 startup
pm2 save
```

## Critical Implementation Rules

### Development Rules
1. **NEVER kill NPM processes** - Destroys user sessions
2. **Backend auto-restarts** - Via nodemon, don't restart manually
3. **Frontend auto-refreshes** - Via Vite HMR
4. **Test through GUI** - Not just API scripts
5. **Research before modifying** - Understand full context

### Security Rules
1. **ALL database operations** - Use SecureDataAccess ONLY
2. **Multi-tenant isolation** - SACRED rule, never bypass
3. **No direct queries** - Always include context
4. **KMS for secrets** - Never plain text
5. **Audit everything** - Complete logging required

### Code Quality Rules
1. **Fix root causes** - No workarounds
2. **Security first** - All operations through SecureDataAccess
3. **Document changes** - Update memory.md
4. **Test thoroughly** - Use verification scripts
5. **Monitor logs** - Use lnav for analysis

## AI Features Implementation

### Function Selection System
```javascript
// Semantic function selection (99.6% token reduction)
const semanticFunctionSelector = require('./services/semanticFunctionSelector');

// Two-stage selection process
const selectedFunctions = await semanticFunctionSelector.select({
  userQuery: "Show me diabetic patients",
  availableFunctions: 2000,  // Total functions
  context: conversationContext
});
// Returns: ~10 relevant functions (99.6% reduction)
```

### Batch Document Processing
```javascript
// Batch processing with Claude Sonnet 4.5
const claudeBatchProcessor = require('./services/claudeBatchProcessor');

// Process multiple documents (50% cost savings)
const result = await claudeBatchProcessor.processBatch({
  documents: documentArray,
  patientId: patientId,
  extractionSchema: medicalSchema
});
// Extracts to 245+ medical collections
```

## Database Backup & Restore

### Backup Procedure
```bash
# Automated backup script
node apps/backend-api/scripts/backupDatabase.js

# Output location:
# /home/erangross/Development/IntelliCare/apps/backups/mongodb-backup-YYYY-MM-DD
```

### Restore Procedure
```bash
# Restore from backup
mongorestore \
  --uri="mongodb://intellicare_admin:[PASSWORD]@localhost:27017/?authSource=admin" \
  --gzip \
  "/home/erangross/Development/IntelliCare/apps/backups/mongodb-backup-2025-10-01"
```

## Troubleshooting

### Common Issues

**Issue: MongoDB authentication failed**
```bash
# Check MongoDB is running
sudo systemctl status mongod

# Verify credentials in KMS
# Never use plain text credentials
```

**Issue: Port already in use**
```bash
# Find process using port
sudo netstat -tulpn | grep :5000

# Don't kill NPM processes - they manage sessions
```

**Issue: Frontend not updating**
```bash
# Vite HMR handles auto-refresh
# If needed, clear browser cache
# Don't restart dev server unnecessarily
```

## Support Resources

- **Project Location**: `/home/erangross/Development/IntelliCare`
- **Backend**: `apps/backend-api/`
- **Frontend**: `apps/frontend-vite/`
- **Documentation**: `docs/` directory
- **Scripts**: `apps/backend-api/scripts/`
- **Production Domain**: `intellicare.health`
- **Memory File**: `/home/erangross/Development/IntelliCare/CLAUDE.md`

This technical guide provides implementation details based on the actual IntelliCare codebase structure and architecture.
