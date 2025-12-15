# TriageX

**AI-Powered Emergency Medical Triage & Hospital Alert System**

TriageX is a comprehensive emergency medical triage platform that connects clinicians with hospitals in real-time. The system uses AI-powered assessment (Google Gemini API with intelligent fallback) to evaluate patient criticality and automatically notifies nearby hospitals of incoming emergency cases.

## Key Features

### For Clinicians
- **AI-Powered Triage Assessment** - Google Gemini API integration for intelligent patient evaluation with rule-based fallback
- **Comprehensive Patient Data Entry** - Capture vitals, symptoms, and medical history
- **Criticality Scoring** - Automated 1-10 severity assessment with visual indicators
- **Smart Hospital Selection** - Find and notify nearest hospitals within customizable radius
- **Real-time Dispatch** - Send en-route alerts with patient snapshots to receiving hospitals
- **Interactive Maps** - Geolocation-based hospital discovery with distance calculation

### For Hospitals  
- **Real-Time Patient Alerts** - WebSocket-powered instant notifications of incoming patients
- **Emergency Audio Notifications** - Automatic sound alerts for critical cases
- **Patient Queue Management** - Visual dashboard with severity-based color coding
- **Detailed Patient Previews** - Access full patient data before arrival
- **Status Tracking** - Monitor pending, acknowledged, and arrived patients
- **Secure Authentication** - Hospital-specific login with bcrypt password hashing

### Technical Highlights
- **Role-Based Access Control** - Separate interfaces for clinicians and hospitals
- **WebSocket Communication** - Real-time bidirectional updates via Socket.io
- **MongoDB Integration** - Persistent storage for patients, vitals, assessments, and alerts
- **RESTful API** - Express.js backend with passport.js authentication
- **Responsive Design** - Mobile-friendly interface with dark/light theme support
- **Form Validation** - React Hook Form with Zod schema validation

## Project Structure

```
TriageX/
├── src/                          # Frontend React application
│   ├── components/               # Reusable UI components
│   │   ├── TriageForm.tsx       # Patient data entry form
│   │   ├── PatientCard.tsx      # Hospital dashboard patient cards
│   │   ├── PatientDetails.tsx   # Detailed patient modal
│   │   ├── HospitalSelector.tsx # Nearest hospital finder
│   │   ├── LoginForm.tsx        # Authentication interface
│   │   └── ui/                  # shadcn/ui components
│   ├── pages/                   # Route pages
│   │   ├── Index.tsx            # Clinician triage interface
│   │   ├── HospitalDashboard.tsx # Hospital alert dashboard
│   │   ├── NearestHospital.tsx  # Hospital selection & dispatch
│   │   └── PatientStatistics.tsx # Analytics dashboard
│   ├── lib/                     
│   │   ├── gemini.ts            # Gemini API integration with fallback
│   │   └── utils.ts             # Utility functions
│   └── types/                   # TypeScript interfaces
│
├── server/                       # Backend Node.js/Express server
│   ├── auth.js                  # Main server with auth & API routes
│   ├── gemini-proxy.mjs         # Gemini API proxy server
│   ├── models/                  # MongoDB Mongoose schemas
│   │   ├── User.js              # Clinician accounts
│   │   ├── Hospital.js          # Hospital accounts & data
│   │   ├── Patient.js           # Patient records
│   │   ├── Vitals.js            # Vital signs
│   │   ├── Symptom.js           # Symptom tracking
│   │   ├── TriageAssessment.js  # Triage evaluations
│   │   └── EnRouteAlert.js      # Hospital notification records
│   └── scripts/                 # Database utilities
│       ├── populateHospitals.js # Seed hospital data
│       └── testHospitalLogin.js # Authentication testing
│
└── public/                       # Static assets
```

## Technology Stack

### Frontend
- **Framework**: React 18 with TypeScript
- **Build Tool**: Vite (SWC plugin for fast refresh)
- **Routing**: React Router v6
- **UI Library**: shadcn/ui (Radix UI primitives)
- **Styling**: Tailwind CSS with custom animations
- **State Management**: React Query (@tanstack/react-query)
- **Forms**: React Hook Form + Zod validation
- **Charts**: Recharts + Chart.js
- **Maps**: Mapbox GL JS
- **Icons**: Lucide React
- **Animations**: Framer Motion

### Backend
- **Runtime**: Node.js with ES Modules
- **Framework**: Express.js 5.1
- **Database**: MongoDB with Mongoose ODM
- **Authentication**: Passport.js (Local + Google OAuth)
- **Real-time**: Socket.io for WebSocket communication
- **Password Hashing**: bcrypt
- **Session Management**: express-session
- **AI Integration**: Google Gemini 1.5 Flash API

### Development Tools
- **Package Manager**: npm (Bun lockfile support)
- **Linting**: ESLint 9 with React plugins
- **TypeScript**: Strict type checking
- **CSS Processing**: PostCSS + Autoprefixer
- **Dev Concurrency**: Concurrently for multi-server startup

## Getting Started

### Prerequisites
- Node.js 18+ installed
- MongoDB instance running (local or MongoDB Atlas)
- Google Gemini API key (optional - falls back to rule-based triage)

### Environment Variables
Create a `.env` file in the root directory:

```env
MONGO_URI=mongodb://localhost:27017/triagex
SESSION_SECRET=your_secure_random_secret_here
GEMINI_API_KEY=your_gemini_api_key_here
```

### Installation

1. **Install dependencies**
   ```bash
   npm install
   ```

2. **Populate hospital data** (first-time setup)
   ```bash
   node server/scripts/populateHospitals.js
   ```

3. **Start all services** (frontend + backend + Gemini proxy)
   ```bash
   npm run dev:all
   ```

   Or start individually:
   ```bash
   # Terminal 1 - Frontend (port 8080)
   npm run dev
   
   # Terminal 2 - Auth/API Server (port 5001)
   node server/auth.js
   
   # Terminal 3 - Gemini Proxy (port 5002)
   node server/gemini-proxy.mjs
   ```

4. **Access the application**
   - Frontend: http://localhost:8080
   - API Server: http://localhost:5001
   - Gemini Proxy: http://localhost:5002

## Hospital Login Credentials

For testing, use these pre-configured hospitals:

**All hospitals use password**: `12345`

### Available Hospitals:
- Manipal Hospital Whitefield (Bangalore)
- Fortis Hospital Bannerghatta Road (Bangalore)
- Apollo Hospital Sheshadripuram (Bangalore)
- Narayana Health City (Bangalore)
- NIMHANS (Bangalore)
- Lilavati Hospital and Research Centre (Mumbai)
- Kokilaben Dhirubhai Ambani Hospital (Mumbai)
- All India Institute of Medical Sciences - AIIMS (Delhi)
- Fortis Escorts Heart Institute (Delhi)
- Apollo Main Hospital (Chennai)

**Login Format:**
1. Select "Hospital" role on login page
2. Enter exact hospital name (case-sensitive)
3. Enter password: `12345`

See [HOSPITAL_LOGIN_CREDENTIALS.md](HOSPITAL_LOGIN_CREDENTIALS.md) for complete list.

## User Workflows

### Clinician Workflow
1. Log in as clinician (or create account)
2. Fill patient triage form with vitals and symptoms
3. AI generates criticality score and medical instructions
4. Review patient data preview
5. Select "Find Nearest Hospital"
6. Choose hospital from map-based list
7. Dispatch en-route alert to selected hospital

### Hospital Workflow
1. Log in with hospital credentials
2. Dashboard displays all incoming en-route alerts
3. Receive real-time notifications with audio alert
4. Review patient details including vitals, symptoms, AI assessment
5. Acknowledge patient arrival
6. Access patient medical history and instructions

## Available Scripts

```bash
npm run dev           # Start Vite dev server (port 8080)
npm run dev:all       # Start all services concurrently
npm run build         # Production build
npm run build:dev     # Development build
npm run preview       # Preview production build
npm run lint          # Run ESLint
```

## Database Schema

### Collections
- **users** - Clinician accounts (email, password, OAuth profiles)
- **hospitals** - Hospital accounts and metadata (name, location, services)
- **patients** - Patient records
- **vitals** - Vital sign measurements
- **symptoms** - Symptom reports
- **triageassessments** - Complete triage evaluations with AI scores
- **enroutealerts** - Hospital notification records with patient snapshots

## Security Features

- Bcrypt password hashing with salt rounds
- HTTP-only session cookies
- CORS configuration for frontend/backend separation
- Passport.js authentication strategies
- Role-based route protection (RequireRole component)
- MongoDB injection protection via Mongoose

## UI/UX Features

- Dark/light theme toggle with ThemeContext
- Responsive mobile-first design
- Toast notifications for user feedback
- Loading states and skeleton screens
- Modal dialogs for detailed views
- Color-coded severity indicators (critical=red, serious=orange, moderate=yellow)
- Animated transitions with Framer Motion
- Accessible components (Radix UI primitives)

## AI Assessment System

The triage AI uses **Google Gemini 1.5 Flash** to generate:
- **Criticality Score**: 1-10 severity rating
- **Clinical Instructions**: 10 step-by-step medical actions

**Intelligent Fallback**: When Gemini API is unavailable (quota exceeded, network issues), the system automatically uses rule-based triage logic evaluating:
- Symptom severity (chest pain, unconsciousness, bleeding, breathing difficulty)
- Vital sign abnormalities (heart rate, blood pressure, temperature, oxygen saturation)
- Multi-symptom combinations

## Real-Time Features

- **WebSocket Rooms**: Hospitals join specific rooms for targeted alerts
- **Instant Notifications**: Sub-second alert delivery
- **Audio Alerts**: Loop until acknowledged for critical patients
- **Live Dashboard Updates**: Automatic patient list refresh
- **Status Synchronization**: Real-time acknowledgment across all clients

## Geolocation Features

- **Haversine Distance Calculation**: Accurate km-based hospital proximity
- **Customizable Search Radius**: 5-50 km range selection
- **Interactive Hospital Map**: Click-to-select interface
- **Address Geocoding**: Convert locations to coordinates
- **Service Filtering**: Filter by hospital capabilities

## Implementation Notes

- Frontend uses path alias `@/` for src directory imports
- MongoDB requires manual connection string configuration
- Gemini proxy separates API key from frontend bundle
- Session store uses in-memory (production should use Redis/MongoDB store)
- Hospital passwords are pre-hashed in database scripts
- Socket.io requires CORS configuration for multiple origins

## Future Enhancements

- [ ] Patient outcome tracking and analytics
- [ ] EMS/Ambulance integration
- [ ] Hospital bed availability tracking
- [ ] Multi-language support
- [ ] Mobile apps (React Native)
- [ ] Telemedicine video consultation
- [ ] Integration with hospital EMR systems
- [ ] Advanced analytics dashboard
- [ ] SMS/Push notifications
- [ ] Barcode/QR patient identification

## License

This project is for educational and demonstration purposes.

## Disclaimer

**This is a demonstration system and should NOT be used for actual medical emergencies without proper certification, testing, and regulatory approval. Always consult qualified healthcare professionals for medical decisions.**

---

*Built for emergency healthcare professionals*
