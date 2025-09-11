# IntelliCare Platform - Technical Implementation Guide

## Project Structure Migration

### Source Repository Structure
```
C:\Users\Eran Gross\IntelliCare\
├── apps/
│   ├── backend-api/
│   │   ├── services/          (200+ service files)
│   │   ├── routes/            (70+ route files)
│   │   ├── models/            (Database models)
│   │   ├── middleware/        (Express middleware)
│   │   ├── utils/             (Utility functions)
│   │   ├── config/            (Configuration files)
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

## Key Services to Migrate

### Priority 1: Core AI Services
```javascript
// Essential AI service files
- services/agentServiceV4.js (1.2MB) - Main AI orchestration
- services/agentServiceClaude.js - Claude Sonnet 4 integration
- services/claudeMedicalService.js - Document processing with Claude
- services/generatedMedicalFunctions.js - Medical operations
```

### Priority 2: Authentication & Security
```javascript
// Critical security services
- services/authAIService.js - AI-powered authentication
- services/secureDataAccess.js - Data access control
- services/hipaaComplianceService.js - HIPAA compliance
- services/secureSessionManager.js - Session management
```

### Priority 3: Medical Operations
```javascript
// Core medical functionality
- services/medicalDataService.js - Medical data management
- services/patientDataService.js - Patient operations
- services/clinicalDecisionSupport.js - Clinical support
- services/documentAnalysisService.js - Document processing
```

## API Routes Implementation

### Core Route Structure
```javascript
// Main API route files to implement
const routes = {
  '/api/agent': 'routes/agent.js',           // AI interactions
  '/api/auth': 'routes/auth.js',             // Authentication
  '/api/patients': 'routes/patients.js',     // Patient management
  '/api/documents': 'routes/documents.js',   // Document handling
  '/api/medical': 'routes/medical.js',       // Medical operations
  '/api/clinics': 'routes/clinics.js',       // Clinic management
  '/api/appointments': 'routes/appointments.js', // Scheduling
  '/api/billing': 'routes/billing.js'        // Billing operations
};
```

## Frontend Components Migration

### Essential Components
```javascript
// Priority components to migrate
const components = [
  'ChatAuthAI.js',          // AI chat interface
  'PatientDetail.js',       // Patient management
  'PatientList.js',         // Patient listing
  'Diagnosis.js',           // Diagnosis interface
  'DocumentViewer.js',      // Document viewing
  'MedicalHistoryModal.js', // Medical history
  'UserManagement.js',      // User administration
  'ClinicManagementDashboard.js' // Clinic dashboard
];
```

## Database Configuration

### MongoDB Schema Setup
```javascript
// Essential collections to create
const collections = {
  patients: {
    indexes: ['patientId', 'clinicId', 'email'],
    sharding: { key: 'clinicId' }
  },
  documents: {
    indexes: ['patientId', 'type', 'uploadDate'],
    ttl: { field: 'expiryDate' }
  },
  appointments: {
    indexes: ['patientId', 'providerId', 'date'],
    compound: [['clinicId', 'date']]
  },
  auditLogs: {
    indexes: ['userId', 'action', 'timestamp'],
    capped: { size: 10737418240 } // 10GB
  }
};
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
# Create .env file with required variables
NODE_ENV=development
PORT=3001
MONGODB_URI=mongodb://localhost:27017/intellicare
REDIS_URL=redis://localhost:6379
JWT_SECRET=development_secret_key
ENCRYPTION_KEY=32_byte_encryption_key_here
SESSION_SECRET=session_secret_key
ANTHROPIC_API_KEY=your_claude_sonnet_4_api_key
```

### Production Environment
```bash
# Production configuration
NODE_ENV=production
PORT=443
MONGODB_URI=mongodb+srv://cluster.mongodb.net/intellicare
REDIS_URL=redis://redis-cluster:6379
USE_SSL=true
SSL_CERT_PATH=/etc/ssl/certs/intellicare.crt
SSL_KEY_PATH=/etc/ssl/private/intellicare.key
```

## Migration Steps

### Step 1: Initialize Project Structure
```bash
# Create directory structure
mkdir -p IntelliCare-Platform/{backend,frontend,libs,docs,scripts}
mkdir -p IntelliCare-Platform/backend/{services,routes,models,middleware,utils}
mkdir -p IntelliCare-Platform/frontend/src/{components,services,hooks,pages}
```

### Step 2: Copy Core Files
```bash
# Copy backend services
cp -r C:/Users/Eran\ Gross/IntelliCare/apps/backend-api/services/* backend/services/
cp -r C:/Users/Eran\ Gross/IntelliCare/apps/backend-api/routes/* backend/routes/
cp -r C:/Users/Eran\ Gross/IntelliCare/apps/backend-api/models/* backend/models/

# Copy frontend components
cp -r C:/Users/Eran\ Gross/IntelliCare/apps/frontend-vite/src/* frontend/src/
```

### Step 3: Install Dependencies
```bash
# Backend dependencies
cd backend && npm install

# Frontend dependencies
cd ../frontend && npm install
```

### Step 4: Configure Database
```javascript
// backend/config/database.js
const { MongoClient } = require('mongodb');

const initializeDatabase = async () => {
  const client = new MongoClient(process.env.MONGODB_URI);
  await client.connect();
  
  const db = client.db(process.env.MONGODB_DB_NAME);
  
  // Create indexes
  await db.collection('patients').createIndex({ patientId: 1 });
  await db.collection('documents').createIndex({ patientId: 1, uploadDate: -1 });
  await db.collection('appointments').createIndex({ date: 1, clinicId: 1 });
  
  return db;
};
```

### Step 5: Setup Main Server
```javascript
// backend/server.js
const express = require('express');
const cors = require('cors');
const helmet = require('helmet');
const { initializeDatabase } = require('./config/database');
const { loadServices } = require('./services/masterServiceLoader');
const { setupRoutes } = require('./routes/routeLoaderService');

const app = express();

// Middleware
app.use(helmet());
app.use(cors());
app.use(express.json({ limit: '50mb' }));

// Initialize services
const startServer = async () => {
  const db = await initializeDatabase();
  await loadServices(db);
  setupRoutes(app, db);
  
  const port = process.env.PORT || 3001;
  app.listen(port, () => {
    console.log(`IntelliCare Platform running on port ${port}`);
  });
};

startServer();
```

## Testing Implementation

### Unit Test Example
```javascript
// tests/services/medicalDataService.test.js
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
// tests/integration/patient-flow.test.js
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
// Implement connection pooling
const mongoOptions = {
  maxPoolSize: 100,
  minPoolSize: 10,
  maxIdleTimeMS: 30000,
  serverSelectionTimeoutMS: 5000
};

// Use aggregation pipelines for complex queries
const getPatientStats = async (clinicId) => {
  return await db.collection('patients').aggregate([
    { $match: { clinicId } },
    { $group: {
      _id: '$status',
      count: { $sum: 1 }
    }}
  ]).toArray();
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
    max: 100 // limit each IP to 100 requests
  }),
  // CORS configuration
  cors({
    origin: process.env.ALLOWED_ORIGINS?.split(','),
    credentials: true
  })
];
```

### Data Encryption
```javascript
// utils/encryption.js
const crypto = require('crypto');

const encrypt = (text) => {
  const iv = crypto.randomBytes(16);
  const cipher = crypto.createCipheriv(
    'aes-256-cbc',
    Buffer.from(process.env.ENCRYPTION_KEY, 'hex'),
    iv
  );
  
  let encrypted = cipher.update(text, 'utf8', 'hex');
  encrypted += cipher.final('hex');
  
  return iv.toString('hex') + ':' + encrypted;
};
```

## Monitoring & Logging

### Application Monitoring
```javascript
// monitoring/metrics.js
const prometheus = require('prom-client');

const httpRequestDuration = new prometheus.Histogram({
  name: 'http_request_duration_seconds',
  help: 'Duration of HTTP requests in seconds',
  labelNames: ['method', 'route', 'status']
});

// Track API performance
app.use((req, res, next) => {
  const end = httpRequestDuration.startTimer();
  res.on('finish', () => {
    end({ method: req.method, route: req.path, status: res.statusCode });
  });
  next();
});
```

## Deployment Configuration

### Docker Setup
```dockerfile
# Dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
EXPOSE 3001
CMD ["node", "server.js"]
```

### Docker Compose
```yaml
# docker-compose.yml
version: '3.8'
services:
  backend:
    build: ./backend
    ports:
      - "3001:3001"
    environment:
      - NODE_ENV=production
    depends_on:
      - mongodb
      - redis
  
  frontend:
    build: ./frontend
    ports:
      - "3000:3000"
    depends_on:
      - backend
  
  mongodb:
    image: mongo:6
    volumes:
      - mongo-data:/data/db
  
  redis:
    image: redis:7-alpine
    
volumes:
  mongo-data:
```

## Next Steps

1. **Review source code** in `C:\Users\Eran Gross\IntelliCare`
2. **Set up development environment** with required tools
3. **Create database schema** and indexes
4. **Migrate core services** starting with AI and authentication
5. **Implement API routes** for essential operations
6. **Build frontend components** for user interface
7. **Configure security** and compliance features
8. **Set up testing** framework and write tests
9. **Deploy** to staging environment
10. **Performance testing** and optimization

This technical guide provides the foundation for migrating the IntelliCare codebase to the IntelliCare-Platform repository. Follow the implementation steps sequentially for best results.