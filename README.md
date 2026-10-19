# Skillora — Skill-Based Learning & Certification Platform

Skillora is a MERN stack e-learning and certification platform. Instructors can create and manage courses, while learners can enroll, track their progress, and earn certificates.

## Features

- Role-based dashboards and workflows for instructors and learners
- Course creation, curriculum management, and video content
- Razorpay payment integration
- PDF certificate generation and verification

## Technology Stack

| Layer | Technology |
|:---|:---|
| Frontend | React, Vite |
| Backend | Node.js, Express |
| Database | MongoDB, Mongoose |
| Payments | Razorpay |
| Email | Resend |

## Local Development

### Requirements

- Node.js 22 or newer
- MongoDB running locally or a MongoDB connection URI
- npm

### Backend

1. Open a terminal in `backend` and install dependencies:
   ```bash
   cd backend
   npm install
   ```
2. Create `backend/.env` from `backend/.env.example` and set `MONGODB_URI` to a running MongoDB instance. Add real payment and email credentials to use those integrations.
3. Start the API:
   ```bash
   npm run dev
   ```

The backend listens on `http://localhost:3000`; its health endpoint is `http://localhost:3000/health`.

### Frontend

In a second terminal:

```bash
cd frontend
npm install
npm run dev
```

Open the local URL printed by Vite (normally `http://localhost:5173`). The frontend uses `http://localhost:3000/api` by default; set `VITE_API_BASE_URL` if the backend uses a different URL.

## Production Builds

Build the frontend with:

```bash
cd frontend
npm run build
```
