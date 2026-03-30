# 🤖 AI Mock Interview Platform

An AI-powered mock interview platform that conducts realistic, personalized technical and behavioral interviews. Upload your resume, pick a role, and get grilled by **Natalie** — your virtual AI interviewer — then receive detailed performance feedback.

---

## ✨ Features

- **Resume-Aware Questions** — Uploads and parses your PDF resume; generates personalized interview questions based on your skills, projects, and experience.
- **Conversational AI Interviewer** — A virtual interviewer named Natalie (powered by Google Gemini) conducts the interview turn-by-turn, acknowledging your answers and transitioning naturally between questions.
- **Voice Input via Speech-to-Text** — Answer questions by speaking; AssemblyAI transcribes your audio responses in real time.
- **Text-to-Speech Responses** — Natalie's responses are spoken aloud using the Murf TTS API for a more immersive experience.
- **Live Code Editor** — Integrated Monaco Editor for coding questions (write, fix a bug, or explain a snippet).
- **Detailed Feedback Report** — At the end of each interview, Gemini evaluates your performance across 5 categories: Communication, Technical Knowledge, Problem Solving, Code Quality, and Confidence — with an overall score and specific improvement tips.
- **Interview History** — Browse all past interviews with scores, roles, and dates.
- **JWT Authentication** — Secure login/signup with token-based session management.

---

## 🛠️ Tech Stack

### Frontend (`/client`)
| Technology | Purpose |
|---|---|
| React 19 + Vite | UI framework & build tool |
| React Router v7 | Client-side routing |
| Axios | HTTP requests to backend API |
| Monaco Editor | In-browser code editor for coding questions |
| React Hot Toast | Toast notifications |
| React Icons | Icon library |

### Backend (`/server`)
| Technology | Purpose |
|---|---|
| Node.js + Express 5 | REST API server |
| MongoDB + Mongoose | Database & ODM |
| Google Gemini (`@google/genai`) | Question generation, follow-ups, and feedback |
| AssemblyAI | Speech-to-text transcription |
| Murf API | Text-to-speech for interviewer voice |
| Multer + pdfjs-dist | PDF resume upload & parsing |
| bcryptjs | Password hashing |
| jsonwebtoken | JWT authentication |

---

## 📁 Project Structure

```
ai-mock-interview-platform/
├── client/                   # React frontend
│   ├── src/
│   │   ├── pages/
│   │   │   ├── LoginPage/        # Login & signup
│   │   │   ├── HomePage/         # Dashboard with recent interviews
│   │   │   ├── InterviewSetupPage/ # Role selection & resume upload
│   │   │   ├── InterviewPage/    # Live interview interface
│   │   │   ├── FeedbackPage/     # Post-interview feedback report
│   │   │   └── HistoryPage/      # All past interviews
│   │   ├── components/           # Shared UI components (Navbar, ProtectedRoute, etc.)
│   │   ├── services/             # Axios API service functions
│   │   ├── context/              # React context (auth state)
│   │   └── constants/            # App-wide constants
│   ├── index.html
│   └── vite.config.js
│
└── server/                   # Express backend
    ├── server.js             # Entry point (loads env, connects DB, starts server)
    └── src/
        ├── app.js            # Express app config (CORS, middleware, routes)
        ├── config/
        │   └── db.config.js  # MongoDB connection
        ├── routes/
        │   ├── auth.routes.js      # POST /api/auth/register, /login
        │   ├── resume.routes.js    # POST /api/resume/upload, GET /api/resume
        │   ├── interview.routes.js # POST /api/interview/start, /answer, /feedback
        │   ├── history.routes.js   # GET /api/history
        │   └── index.js           # Route aggregator
        ├── controllers/      # Request handlers
        ├── models/
        │   ├── User.model.js
        │   └── Interview.model.js
        ├── services/         # AI integrations (Gemini, AssemblyAI, Murf)
        ├── middleware/       # Auth, error handling, file upload
        ├── constants/
        │   └── prompts.js    # All Gemini prompts
        └── utils/
```

---

## 🚀 Getting Started

### Prerequisites

- Node.js v18+
- MongoDB Atlas account (or local MongoDB)
- Google Gemini API key
- AssemblyAI API key
- Murf API key

### 1. Clone the Repository

```bash
git clone <repo-url>
cd ai-mock-interview-platform
```

### 2. Backend Setup

```bash
cd server
npm install
```

Create a `.env` file inside `server/`:

```env
PORT=5000
NODE_ENV=development
MONGODB_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret_key
JWT_EXPIRES_IN=7d
GEMINI_API_KEY=your_gemini_api_key
ASSEMBLYAI_API_KEY=your_assemblyai_api_key
MURF_API_KEY=your_murf_api_key
CLIENT_URL=http://localhost:5173
```

Start the backend:

```bash
# Development (with auto-reload)
npm run dev

# Production
npm start
```

The server runs on `http://localhost:5000`.

### 3. Frontend Setup

```bash
cd client
npm install
```

Create a `.env` file inside `client/`:

```env
VITE_API_URL=http://localhost:5000
```

Start the frontend:

```bash
npm run dev
```

The app runs on `http://localhost:5173`.

---

## 🔌 API Reference

All routes are prefixed with `/api`. Protected routes require a `Authorization: Bearer <token>` header.

### Auth `/api/auth`
| Method | Endpoint | Description |
|---|---|---|
| POST | `/register` | Register a new user |
| POST | `/login` | Login and receive JWT |

### Resume `/api/resume` *(protected)*
| Method | Endpoint | Description |
|---|---|---|
| POST | `/upload` | Upload a PDF resume (multipart/form-data) |
| GET | `/` | Retrieve the current user's parsed resume text |

### Interview `/api/interview` *(protected)*
| Method | Endpoint | Description |
|---|---|---|
| POST | `/start` | Start a new interview session (returns questions + greeting) |
| POST | `/answer` | Submit an answer and get Natalie's follow-up response |
| POST | `/feedback` | Finish interview and generate AI feedback report |

### History `/api/history` *(protected)*
| Method | Endpoint | Description |
|---|---|---|
| GET | `/` | List all past interviews for the current user |
| GET | `/:id` | Get full details of a specific interview |

---

## 🎯 Interview Flow

```
1. Login / Sign Up
       ↓
2. Upload Resume (PDF) → parsed to text
       ↓
3. Select Role (e.g. "Frontend Developer", "Data Scientist")
       ↓
4. AI generates personalized questions:
   • Behavioral questions (from resume experience)
   • Technical knowledge questions
   • 1 Coding question (write / fix bug / explain code)
       ↓
5. Live interview with Natalie:
   • Natalie greets you and begins with "Tell me about yourself"
   • You answer via voice (speech-to-text) or text
   • Natalie acknowledges your answer and moves to the next question
   • Coding questions open the Monaco Editor
       ↓
6. Feedback Report:
   • Overall score (0–100)
   • Category scores: Communication, Technical, Problem Solving,
     Code Quality, Confidence
   • Strengths & Areas for Improvement
   • Final Assessment
       ↓
7. View history of all past interviews
```

---

## 🔐 Environment Variables Summary

| Variable | Required In | Description |
|---|---|---|
| `MONGODB_URI` | Server | MongoDB connection string |
| `JWT_SECRET` | Server | Secret for signing JWTs |
| `JWT_EXPIRES_IN` | Server | Token expiry (e.g. `7d`) |
| `GEMINI_API_KEY` | Server | Google Gemini AI API key |
| `ASSEMBLYAI_API_KEY` | Server | AssemblyAI speech-to-text key |
| `MURF_API_KEY` | Server | Murf text-to-speech key |
| `CLIENT_URL` | Server | Frontend URL for CORS (e.g. `http://localhost:5173`) |
| `PORT` | Server | Server port (default: `5000`) |
| `VITE_API_URL` | Client | Backend API base URL |

---

## 📄 License

MIT
