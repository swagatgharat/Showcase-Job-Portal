# Professional Job Portal

An advanced recruitment platform designed to streamline hiring by directly connecting job seekers with recruiters. This platform features tailored seeker and recruiter dashboards, secure resume storage, and real-time application pipeline management.

## Links

- **Live Demo**: [https://job-portal-5pt9.onrender.com/](https://job-portal-5pt9.onrender.com/)

## Image

<img width="3150" height="2450" alt="Job Portal" src="https://github.com/user-attachments/assets/5422eba7-b0fa-47c6-843e-0c1f28f593ae" />


## 1. Project Overview

**Professional Job Portal** provides a central workspace that replaces disorganized email chains and static job boards. Candidates can discover roles, save positions, and trace application updates as they progress. Simultaneously, employers can manage active listings, analyze their applicant pool, and update statuses through an intuitive, role-based workflow.

### Problem Solved

Traditional recruitment platforms often rely on messy email threads and lack transparency. This project solves these issues by providing a centralized dashboard for real-time application tracking, direct pipeline status management, and automated visual metrics.

### Target Users

- **Job Seekers (Applicants)**: To find relevant roles, save interesting listings, apply, and monitor application progress.
- **Employers / Recruiters**: To post jobs, manage candidate application phases, and view metrics on hiring operations.

---

## 2. Core Features

- **Dual-Dashboard View**: Specialized panel layouts customized for Seekers (application tracker, saved jobs) and Employers (job poster, applicant pipelines).
- **Advanced Job Discovery**: Full-text searching and filtering by keywords, location, and job type (Full-time, Part-time, Contract, etc.).
- **Dynamic Application Pipeline**: Interactive status updates (Applied, Reviewed, Accepted, Rejected) allowing employers to manage candidates seamlessly.
- **Profile & Resume Management**: Complete digital resume hub featuring custom profiles, avatar updates, and secure resume uploads integrated with ImageKit.io.
- **Security & Integrity**: Complete token refresh rotation, HttpOnly cookie storage, rate-limiting, and Double-Submit CSRF cookie checking.

---

## 3. Tech Stack

- **Framework**: [React 19](https://react.dev/) / [Vite](https://vite.dev/)
- **Language**: [JavaScript (ES Modules)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)
- **Runtime**: [Node.js](https://nodejs.org/) / [Express.js 5](https://expressjs.com/)
- **Database**: [MongoDB](https://www.mongodb.com/) via [Mongoose](https://mongoosejs.com/)
- **Styling**: [Tailwind CSS 4](https://tailwindcss.com/)
- **Storage**: [ImageKit.io](https://imagekit.io/) for cloud media and resume storage
- **State & Routing**: [React Router 7](https://reactrouter.com/) & [React Context API](https://react.dev/)

---

## 4. Architecture Overview

### Frontend Structure

The client application follows a modular React SPA structure:

- **Pages**: Located in `src/pages`, separated into Seeker, Employer, and shared routes.
- **Components**: Reusable UI blocks located in `src/components`, handling navigation, layouts, modals, and tables.
- **Context API**: Global state providers in `src/context` managing authentication status and styling themes.
- **API Interceptors**: Unified Axios instance in `src/utils/axiosInstance.js` handling automatic CSRF token header embedding and automatic JWT token refresh.

### Backend Structure

The backend follows a Clean Controller-Service-Routing MVC architecture:

- **Routes**: Located in `src/routes`, separating Auth, User, Jobs, Applications, Saved Jobs, and Analytics endpoints.
- **Controllers**: Located in `src/controllers`, coordinating database query structures and request validations.
- **Models**: Located in `src/models`, defining schemas for User, Job, Application, and SavedJob collections.
- **Middleware**: Located in `src/middleware`, housing cookie parsers, image upload controllers, CSRF checkers, and global error boundaries.

### Authentication Flow

1. User authenticates via `POST /api/auth/login` → Server returns `accessToken` and `refreshToken` stored in secure, `httpOnly`, `Secure`, and `SameSite` cookies.
2. The browser manages token cookies automatically; client-side JS cannot access them, eliminating token leakage vectors.
3. Every API call automatically sends credentials (`withCredentials: true` configuration in Axios).
4. Upon receiving a `401 Unauthorized` response with an expired access token, the Axios interceptor transparently issues a `POST /api/auth/refresh` request to obtain a new token pair and retries the original request.
5. Logout revokes authorization by clearing cookies server-side via `POST /api/auth/logout`.

### CSRF Protection Flow

1. Every session generates a unique CSRF token, which is stored in a cookie.
2. The server sends this token back to the frontend in the `X-CSRF-Token` response header.
3. For state-changing requests (`POST`, `PUT`, `DELETE`), the Axios request interceptor injects the cached token into the request header.
4. The backend verifies the token using the Double Submit Cookie pattern via `csurf` middleware.

---

## 5. Folder Structure

```text
.
├── backend/                # Express Server
│   ├── config/             # DB and ImageKit configurations
│   ├── controllers/        # Request handlers & business logic
│   ├── middleware/         # Auth, CSRF, upload, & error logic
│   ├── models/             # Mongoose schemas (User, Job, etc.)
│   ├── routes/             # Express route definitions
│   ├── utils/              # Shared helpers and constants
│   └── server.js           # Express main entry file
├── frontend/               # React + Vite Application
│   ├── src/
│   │   ├── components/     # Reusable UI components (Navbar, Button, etc.)
│   │   ├── context/        # Auth and global state management
│   │   ├── pages/          # Page components grouped by role
│   │   ├── routes/         # Router definitions & ProtectedRoute
│   │   └── utils/          # Axios instance, apiPath configurations & helpers
│   ├── public/             # Static assets (Vite logo, etc.)
│   ├── index.html          # HTML entry point
│   ├── vite.config.js      # Vite configuration
│   └── package.json        # Frontend dependencies & scripts
└── README.md
```

---

## 6. Environment Variables

### Backend Configuration

Create a `.env` file in the `backend/` directory with the following variables:

```env
# Server Config
PORT=8000
NODE_ENV=production

# Database
MONGO_URI=your_mongodb_connection_uri

# Security
JWT_SECRET=your_jwt_signing_secret

# ImageKit (Media & Resumes)
IMAGEKIT_PUBLIC_KEY=your_imagekit_public_key
IMAGEKIT_PRIVATE_KEY=your_imagekit_private_key
IMAGEKIT_URL_ENDPOINT=your_imagekit_url_endpoint
```

### Frontend Configuration

The frontend references the backend API URL directly in `src/utils/apiPath.js`:

```javascript
export const BASE_URL = 'http://localhost:8000'
```

---

## 7. Installation & Setup

### Prerequisites

- Node.js (v18+)
- MongoDB Atlas cluster
- ImageKit account

### Step-by-Step Setup

1. **Clone the Repository**

   ```bash
   git clone https://github.com/swagatgharat/Job-Portal.git
   cd "Job Portal"
   ```

2. **Backend Setup**

   ```bash
   cd backend
   npm install
   # Create .env and configure variables
   npm run dev
   ```

3. **Frontend Setup**

   ```bash
   cd ../frontend
   npm install
   # Configure BASE_URL in src/utils/apiPath.js if different
   npm run dev
   ```

---

## 8. Security Implementation

- **Role-Based Access Control (RBAC)**: Enforces absolute security separation between Job Seekers and Employers.
- **Double Submit Cookie CSRF Protection**: Mitigates Cross-Site Request Forgery attacks using `csurf` and custom header verification.
- **HTTP-Only Cookies**: JWT tokens are kept out of reach of client-side scripts, protecting the application from Cross-Site Scripting (XSS) attacks.
- **Express Rate Limiting**: Employs brute-force request restrictions on authentication routes.
- **Robust Schema Verification**: Enforces clean input sanitization and verification on all incoming payloads before database persistence.

---

## 9. API Documentation

| Method   | Endpoint                          | Description                                             | Auth Required |
| :------- | :-------------------------------- | :------------------------------------------------------ | :------------ |
| **Authentication** |||
| `POST`   | `/api/auth/register`              | Create a new account (with avatar image upload)         | No            |
| `POST`   | `/api/auth/login`                 | Authenticate and receive cookies                        | No            |
| `POST`   | `/api/auth/refresh`               | Rotate session JWTs via cookies                         | No            |
| `POST`   | `/api/auth/logout`                | Clear session cookies and invalidate token              | Yes (Optional)|
| `GET`    | `/api/auth/me`                    | Retrieve logged-in user profile details                 | Yes           |
| `GET`    | `/api/auth/session`               | Check current login state                               | No            |
| `POST`   | `/api/auth/upload-image`          | Upload profile/general image to ImageKit                | Yes           |
| **User & Profiles** |||
| `PUT`    | `/api/user/profile`               | Update user details and experience                      | Yes           |
| `POST`   | `/api/user/resume`                | Remove current candidate resume from ImageKit           | Yes           |
| `DELETE` | `/api/user/avatar`                | Remove current avatar image from ImageKit               | Yes           |
| `GET`    | `/api/user/:id`                   | Get public user profile by ID                           | No            |
| **Jobs** |||
| `GET`    | `/api/jobs`                       | List all open jobs with keyword and location filters    | No            |
| `POST`   | `/api/jobs`                       | Post a new job opportunity                             | Yes           |
| `GET`    | `/api/jobs/get-jobs-employer`     | Fetch all jobs posted by the active employer            | Yes           |
| `GET`    | `/api/jobs/:id`                   | Fetch single job description by ID                      | No            |
| `PUT`    | `/api/jobs/:id`                   | Update job details (employer/owner only)                | Yes           |
| `DELETE` | `/api/jobs/:id`                   | Delete job post (employer/owner only)                   | Yes           |
| `PUT`    | `/api/jobs/:id/toggle-close`      | Toggle job status between Open and Closed               | Yes           |
| **Saved Jobs** |||
| `POST`   | `/api/save-jobs/:jobId`           | Bookmark job by ID                                      | Yes           |
| `DELETE` | `/api/save-jobs/:jobId`           | Remove bookmark of job by ID                            | Yes           |
| `GET`    | `/api/save-jobs/my`               | List all saved job posts of the active user             | Yes           |
| **Applications** |||
| `POST`   | `/api/applications/:jobId`        | Submit candidate application for a job                  | Yes           |
| `GET`    | `/api/applications/my`            | Fetch application history of the active candidate       | Yes           |
| `GET`    | `/api/applications/job/:jobId`    | Fetch all candidate applications for a job listing      | Yes           |
| `GET`    | `/api/applications/:id`           | Fetch details of a single job application               | Yes           |
| `PUT`    | `/api/applications/:id/status`    | Update application phase status                         | Yes           |
| **Analytics & System** |||
| `GET`    | `/api/analytics/overview`         | Retrieve recruitment and posting metrics                | Yes           |
| `GET`    | `/api/csrf`                       | Fetch active CSRF validation token                      | No            |

---

## 10. Deployment Instructions

### Backend Deployment

1. Deploy the backend to a cloud hosting platform such as **Render** or **Railway**.
2. Connect your Git repository.
3. Configure all environment variables listed in Section 6.
4. Set the build command to: `npm install`.
5. Set the start command to: `node server.js` or `npm start`.

### Frontend Deployment

1. Deploy the frontend folder to **Vercel** or **Netlify**.
2. Configure the build command: `npm run build`.
3. Set the output directory to: `dist`.
4. Point the base directory / root directory of the deployment project to the `frontend` folder.

---

## 11. Future Improvements

- **Live Chat Integration**: Enable real-time messaging between candidates and recruiters.
- **ATS Resume Parsing**: Automated parser matching resumes to job requirements.
- **Email Alerts**: Automatically email candidates on status changes.
- **Global Admin Panel**: A single interface for application-wide management of jobs, users, and reports.

---

## 12. Contributing Guidelines

1. **Fork** the repository.
2. Create a **Feature Branch** (`git checkout -b feature/AmazingFeature`).
3. **Commit** your changes (`git commit -m 'Add some AmazingFeature'`).
4. **Push** to the branch (`git push origin feature/AmazingFeature`).
5. Open a **Pull Request**.

**Code Style**: Please follow the camelCase naming convention for files and variables as established in the project guidelines.

---

_Designed & developed by [Swagat Gharat](https://github.com/swagatgharat)_
