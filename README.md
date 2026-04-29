# Professional Job Portal

A sophisticated, full-stack Job Portal platform designed to bridge the gap between talented job seekers and industry-leading recruiters. This application provides a seamless, secure, and enhanced experience for managing the entire recruitment lifecycle.

## 🌟 Project Overview

The Professional Job Portal is a modern solution to the fragmented job market. It provides specialized environments for both candidates and employers, ensuring that job search and talent acquisition are efficient, transparent, and data-driven.

**Problem Solved:**

- Eliminates the chaos of email-based job applications.
- Provides real-time tracking for candidates.
- Offers powerful management tools for recruiters to handle hundreds of applications effortlessly.

**Target Users:**

- **Job Seekers:** Individuals looking for career opportunities and tracking their applications.
- **Employers / Recruiters:** Organizations posting jobs and managing candidate pipelines.

---

## 🚀 Tech Stack

### Frontend

- **Framework:** React 19 (Vite)
- **Styling:** Tailwind CSS 4 (Next-gen utility-first CSS)
- **Routing:** React Router 7 (Efficient navigation and layout management)
- **Animations:** Framer Motion (Smooth UI transitions and micro-interactions)
- **Icons:** Lucide React (Clean, scalable vector icons)
- **Notifications:** React Hot Toast (Elegant toast notifications)
- **State Management:** React Context API (Clean global auth and theme state)

### Backend

- **Runtime:** Node.js
- **Framework:** Express 5 (Robust API infrastructure)
- **Database:** MongoDB (Scalable document storage via Mongoose)
- **File Storage:** ImageKit (Optimized CDN for images and resumes)

### 🛡️ Security & Reliability

- **Authentication:** JWT (JSON Web Tokens) with Refresh Token rotation.
- **Token Storage:** HttpOnly, Secure, SameSite cookies to prevent XSS.
- **CSRF Protection:** Double Submit Cookie pattern via `csurf`.
- **Rate Limiting:** Protects against Brute Force attacks on auth endpoints.
- **Validation:** Strict schema validation for all inputs using custom logic.
- **Error Handling:** Centralized global error middleware for consistent API responses.

---

## ✨ Core Features

### 👤 For Job Seekers (Applicants)

- **Advanced Job Discovery:** Filter jobs by keyword, location and type.
- **Application Tracking:** Real-time status updates (Applied, Reviewed, Accepted, Rejected).
- **Saved Jobs:** Personal "wishlist" to save interesting roles for later.
- **Profile Hub:** Comprehensive profile with bio, skills, and resume management.

### 🏢 For Employers (Recruiters)

- **Job Lifecycle Management:** Full CRUD operations for job postings.
- **Applicant Pipeline:** View and manage all candidates for specific job listings.
- **Job Status Control:** Quickly open/close job postings to manage talent flow.
- **Dashboard Analytics:** High-level overview of recruitment activity.

### 🛡️ System Features

- **Role-Based Access Control (RBAC):** Strict separation between user types.
- **Smooth Navigation:** Seamless layouts with "Scroll to Top" and responsive design.
- **Persistent Sessions:** Secure login state maintained across page reloads.

---

## 🏗️ Architecture Overview

### Modular Structure

- **Frontend:** Follows a component-based architecture with dedicated `pages`, `components`, `context`, and `utils`. Layouts are used to separate Seeker and Employer views.
- **Backend:** Traditional MVC-inspired pattern:
  - `Routes` define endpoints and apply middleware.
  - `Controllers` contain business logic and interact with Models.
  - `Models` define the MongoDB schema.
  - `Middleware` handles auth, security, and file uploads.

### Authentication Flow

1. User logs in -> Server generates `accessToken` (Short-lived) and `refreshToken` (Long-lived).
2. Tokens are sent as **HttpOnly Cookies**.
3. Frontend includes `credentials: true` in requests.
4. `authMiddleware` verifies the `accessToken`.
5. If expired, the `/api/auth/refresh` endpoint rotates tokens automatically.

### CSRF Protection

A CSRF token is generated and sent to the client via a custom header (`X-CSRF-Token`). The client must send this token back in subsequent `POST/PUT/DELETE` requests, ensuring the request originated from our application.

---

## 📁 Folder Structure

```text
Job Portal/
├── frontend/               # React + Vite application
│   ├── src/
│   │   ├── components/     # Reusable UI components (Navbar, Button, etc.)
│   │   ├── context/        # Auth and global state
│   │   ├── pages/          # Page components grouped by role
│   │   ├── routes/         # Route definitions and ProtectedRoute
│   │   └── utils/          # API helpers and constants
│   ├── public/             # Static assets
│   └── index.html          # HTML entry point
│
└── backend/                # Node.js + Express API
    ├── config/             # Database and ImageKit configurations
    ├── controllers/        # Business logic for all endpoints
    ├── middleware/         # Auth, CSRF, Error, and Upload logic
    ├── models/             # Mongoose schemas (User, Job, etc.)
    ├── routes/             # Express route definitions
    ├── utils/              # Shared helpers and constants
    └── server.js           # Main Entry point
```

---

## 🔑 Environment Variables

### Backend (.env)

| Variable                | Description                                                                |
| :---------------------- | :------------------------------------------------------------------------- |
| `PORT`                  | The port your server will run on (default: 8,000).                         |
| `MONGO_URI`             | Your MongoDB Atlas connection string.                                      |
| `JWT_SECRET`            | Strong secret for signing Access Tokens.                                   |
| `REFRESH_TOKEN_SECRET`  | Strong secret for signing Refresh Tokens.                                  |
| `IMAGEKIT_PUBLIC_KEY`   | Public key from ImageKit dashboard.                                        |
| `IMAGEKIT_PRIVATE_KEY`  | Private key from ImageKit dashboard.                                       |
| `IMAGEKIT_URL_ENDPOINT` | URL Endpoint from ImageKit dashboard.                                      |

---

## 🛠️ Installation & Setup

### 1. Prerequisites

- **Node.js:** v18 or higher.
- **MongoDB:** An active cluster on MongoDB Atlas.
- **ImageKit:** Free account for image handling.

### 2. General Setup

```bash
git clone <your-repo-url>
cd "Job Portal"
```

### 3. Backend Setup

```bash
cd backend
npm install
# Create .env based on the table above
npm run dev
```

### 4. Frontend Setup

```bash
cd ../frontend
npm install
npm run dev
```

---

## 📡 API Reference (Major Routes)

### Authentication `/api/auth`

- `POST /register`: Create a new account (Avatar upload supported).
- `POST /login`: Authenticate and receive cookies.
- `POST /logout`: Clear session cookies.
- `GET /me`: Get current logged-in user details.

### Jobs `/api/jobs`

- `GET /`: List all open jobs with filters.
- `POST /`: Create a new job (Recruiter).
- `PUT /:id`: Update job details (Owner only).
- `DELETE /:id`: Remove job listing (Owner only).

### Applications `/api/applications`

- `POST /:jobId`: Apply for a job.
- `GET /my`: View my application history.
- `PUT /:id/status`: Update application status (Recruiter).

---

## 🌎 Deployment

### Backend

1. Use a platform like **Render** or **Railway**.
2. Connect your repository.
3. Configure Environment Variables in the platform dashboard.
4. Set build command: `npm install`
5. Set start command: `node server.js`

### Frontend

1. Deploy to **Vercel** or **Netlify**.
2. Use the `frontend` folder as the root.
3. Build Command: `npm run build`
4. Output Directory: `dist`

---

## 🔮 Future Improvements

- **Live Chat:** Real-time messaging between candidates and recruiters.
- **ATS Features:** Automated resume parsing and score matching.
- **Email Notifications:** Automatic alerts for application status changes.
- **Admin Panel:** Global management for users and reported jobs.

---

## 🤝 Contributing

1. Fork the Project.
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`).
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`).
4. Push to the Branch (`git push origin feature/AmazingFeature`).
5. Open a Pull Request.

---

*Designed & developed [Swagat Gharat](https://github.com/swagatgharat)*
