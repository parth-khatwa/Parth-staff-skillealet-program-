 # Parth-staff-skillealet-program-
It Nizam modern human resource management system
README.md
package.json
.gitignore
client/.env.example
client/index.html
client/package.json
client/src/App.jsx
client/src/api.js
client/src/components/Layout.jsx
client/src/components/ProtectedRoute.jsx
client/src/components/ui.jsx
client/src/context/AuthContext.jsx
client/src/context/ToastContext.jsx
client/src/index.css
client/src/main.jsx
client/src/pages/Announcements.jsx
client/src/pages/Attendance.jsx
client/src/pages/Dashboard.jsx
client/src/pages/Employees.jsx
client/src/pages/LeaveRequests.jsx
client/src/pages/Leaves.jsx
client/src/pages/Login.jsx
client/src/pages/Profile.jsx
client/src/pages/Register.jsx
client/src/pages/Trainings.jsx
client/src/utils.js
client/vite.config.js
server/.env.example
server/config/db.js
server/middleware/auth.js
server/middleware/error.js
server/models/Announcement.js
server/models/Attendance.js
server/models/Leave.js
server/models/Training.js
server/models/User.js
server/package.json
server/routes/announcements.js
server/routes/attendance.js
server/routes/auth.js
server/routes/dashboard.js
server/routes/employees.js
server/routes/leaves.js
server/routes/trainings.js
server/seed.js
server/server.js
server/utils/asyncHandler.js
server/utils/dates.js
server/utils/helpers.js
# StaffSync — Modern Human Resource Management Solution

StaffSync is a web-based HRM application built with the **MERN stack** (MongoDB, Express.js, React.js, Node.js).
It digitises the core HR processes of a company and gives **HR**, **managers** and **employees** their own
role-based views.

## Features

| Role | What they can do |
| --- | --- |
| **HR** | Add / edit / delete employee records, assign managers, approve or reject any leave request, view attendance of everyone, post and pin news & announcements, create training programmes |
| **Manager** | See own team, approve or reject team leave requests, view team attendance, create trainings and assign them to team members |
| **Employee** | Check in / check out (late and half-day detection), apply for leave and track status, see leave balance, read announcements, complete assigned trainings, edit personal profile, change password |

Also included: JWT authentication, password hashing (bcrypt), protected routes on both frontend and backend,
role-based API authorization, a role-aware dashboard, responsive Bootstrap 5 UI.

## Tech stack

- **Frontend:** React 18 (Vite), React Router 6, Axios, Bootstrap 5, Bootstrap Icons
- **Backend:** Node.js, Express 4, async/await, REST API
- **Database:** MongoDB with Mongoose
- **Auth:** JSON Web Tokens (JWT) + bcryptjs
- **Security:** helmet, CORS, server-side validation

## Project structure

```
staffsync/
├── server/                  # Express + MongoDB API
│   ├── config/db.js
│   ├── middleware/          # auth (JWT + roles), error handling
│   ├── models/              # User, Leave, Attendance, Announcement, Training
│   ├── routes/              # auth, employees, leaves, attendance, announcements, trainings, dashboard
│   ├── utils/
│   ├── seed.js              # demo data
│   ├── server.js
│   └── .env.example
├── client/                  # React app (Vite)
│   └── src/
│       ├── components/      # Layout, ProtectedRoute, shared UI
│       ├── context/         # Auth + Toast providers
│       ├── pages/           # Login, Register, Dashboard, Attendance, Leaves, ...
│       └── api.js
└── package.json             # helper scripts
```

## Requirements

- Node.js **v16 or above** and npm v8+
- MongoDB (local install **or** a free [MongoDB Atlas](https://www.mongodb.com/atlas) cluster)
- Git, and optionally Postman for testing the API

## Getting started

```bash
# 1. install everything
npm run install-all

# 2. configure the server
cp server/.env.example server/.env
#    then open server/.env and set MONGO_URI and a long random JWT_SECRET

# 3. (optional but recommended) load demo data
npm run seed

# 4. run the API (http://localhost:5000) ...
npm run server

# 5. ... and in a second terminal, the React app (http://localhost:5173)
npm run client
```

In development the Vite dev server proxies `/api` to `http://localhost:5000`, so no extra frontend config is needed.

### Demo accounts (created by `npm run seed`)

| Role | Email | Password |
| --- | --- | --- |
| HR | `hr@staffsync.com` | `Hr@12345` |
| Manager | `manager@staffsync.com` | `Manager@123` |
| Employee | `employee@staffsync.com` | `Employee@123` |

> `npm run seed` **deletes** existing users, leaves, attendance, announcements and trainings before inserting demo data.
> Change or remove these accounts before using the app for anything real.

## Environment variables (`server/.env`)

| Variable | Description | Default |
| --- | --- | --- |
| `PORT` | API port | `5000` |
| `MONGO_URI` | MongoDB connection string | `mongodb://127.0.0.1:27017/staffsync` |
| `JWT_SECRET` | Secret used to sign tokens (**required**) | — |
| `JWT_EXPIRES_IN` | Token lifetime | `7d` |
| `CLIENT_URL` | Allowed frontend origin(s), comma separated | any (dev) |
| `TZ_OFFSET_MINUTES` | Company timezone offset from UTC in minutes (India = 330) | `330` |
| `OFFICE_START` | Check-ins after this local time are marked *late* | `09:30` |

Frontend (`client/.env`, only when hosted separately): `VITE_API_URL=https://your-api.example.com/api`

## API overview

All routes except `/api/auth/register` and `/api/auth/login` need the header `Authorization: Bearer <token>`.

| Method & path | Access | Purpose |
| --- | --- | --- |
| `POST /api/auth/register` | public | Sign up (creates an *employee*) |
| `POST /api/auth/login` | public | Log in, returns JWT |
| `GET /api/auth/me` | all | Current user |
| `PUT /api/auth/profile` · `PUT /api/auth/password` | all | Update profile / change password |
| `GET /api/dashboard` | all | Role-specific statistics |
| `GET /api/employees` | HR, manager | List (manager sees own team only, no salaries) |
| `POST/PUT/DELETE /api/employees[/:id]` | HR | Manage employees |
| `GET /api/employees/managers` | HR | Managers for the "reports to" dropdown |
| `POST /api/attendance/check-in` · `/check-out` | all | Mark attendance |
| `GET /api/attendance/today` · `/mine?month=YYYY-MM` | all | Own attendance |
| `GET /api/attendance?date=YYYY-MM-DD` | HR, manager | Attendance sheet (absent = no record) |
| `POST /api/leaves` · `GET /api/leaves/mine` · `DELETE /api/leaves/:id` | all | Apply / list / cancel own leave |
| `GET /api/leaves?status=` | HR, manager | Requests to review |
| `PUT /api/leaves/:id/status` | HR, manager | Approve / reject |
| `GET /api/announcements` | all | Read news |
| `POST/PUT/DELETE /api/announcements[/:id]` | HR | Manage news |
| `GET /api/trainings` | all | Role-filtered list |
| `POST/PUT/DELETE /api/trainings[/:id]` | HR, manager | Manage trainings |
| `PUT /api/trainings/:id/complete` | assigned person | Mark completed |

## Business rules worth knowing

- Leave balance (casual 12, sick 10, earned 15) is checked when applying and **deducted on approval**. Unpaid leave has no balance.
- Leave days are counted as calendar days from the *from* date to the *to* date, inclusive.
- Nobody can approve their own leave. A manager's leave is approved by HR; HR's own leave needs a second HR account.
- A manager can only review leaves of, and assign trainings to, people who report to them.
- A check-in after `OFFICE_START` is *late*; fewer than 4 worked hours becomes *half-day*; no record = *absent*.
- Salary is visible only to HR and to the employee themself.

## Deployment

**Option A — one service (simplest, e.g. Render / Railway):**

1. Create a MongoDB Atlas cluster and copy its connection string.
2. Create a Web Service from this GitHub repo.
   - Build command: `npm run build`
   - Start command: `npm start`
   - Environment: `NODE_ENV=production`, `MONGO_URI`, `JWT_SECRET`, `TZ_OFFSET_MINUTES`
3. The Express server also serves the built React app, so the service URL is your **demo link**.

**Option B — split:** deploy `server/` to Render/Railway and `client/` to Vercel/Netlify
(build `npm run build`, output `dist`, env `VITE_API_URL=https://<api-host>/api`, and set `CLIENT_URL` on the API to the frontend URL).
Add a rewrite of all paths to `/index.html` on the frontend host so page refreshes work.

To load demo data into a hosted database, run `npm run seed` locally with `MONGO_URI` in `server/.env` pointing to Atlas.

## Pushing to GitHub

```bash
cd staffsync
git init
git add .
git commit -m "StaffSync: MERN human resource management system"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

`.gitignore` already excludes `node_modules` and `.env`, so no secrets are uploaded.

## Possible next steps

Payroll and payslips, document uploads, department / holiday calendar, email notifications, export to CSV/PDF, automated tests.

## License

MIT
{
  "name": "staffsync",
  "version": "1.0.0",
  "description": "StaffSync - modern human resource management solution (MERN stack)",
  "private": true,
  "scripts": {
    "install-all": "npm install --prefix server && npm install --prefix client",
    "server": "npm run dev --prefix server",
    "client": "npm run dev --prefix client",
    "seed": "npm run seed --prefix server",
    "build": "npm install --prefix server && npm install --prefix client && npm run build --prefix client",
    "start": "node server/server.js"
  },
  "engines": {
    "node": ">=16"
  },
  "license": "MIT"
}
# dependencies
node_modules/

# environment files (never commit secrets)
.env
.env.local
.env.*.local

# build output
client/dist/
dist/

# logs and OS files
*.log
npm-debug.log*
.DS_Store
Thumbs.db

# editors
.vscode/
.idea/
# Only needed when the frontend is hosted separately from the API (e.g. Vercel / Netlify).
# Example: VITE_API_URL=https://your-api.onrender.com/api
# Leave unset in development - the Vite proxy forwards /api to localhost:5000.
VITE_API_URL=
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta name="description" content="StaffSync - modern human resource management solution" />
    <link rel="icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'%3E%3Crect width='100' height='100' rx='22' fill='%234f46e5'/%3E%3Ctext x='50' y='68' font-size='56' font-family='Arial' font-weight='700' text-anchor='middle' fill='white'%3ES%3C/text%3E%3C/svg%3E" />
    <title>StaffSync</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/src/main.jsx"></script>
  </body>
</html>

{
  "name": "staffsync-client",
  "private": true,
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "axios": "^1.7.2",
    "bootstrap": "^5.3.3",
    "bootstrap-icons": "^1.11.3",
    "react": "^18.3.1",
    "react-dom": "^18.3.1",
    "react-router-dom": "^6.23.1"
  },
  "devDependencies": {
    "@vitejs/plugin-react": "^4.3.1",
    "vite": "^5.3.1"
  }
}
import { Navigate, Route, Routes } from 'react-router-dom';
import ProtectedRoute from './components/ProtectedRoute';
import Layout from './components/Layout';
import Login from './pages/Login';
import Register from './pages/Register';
import Dashboard from './pages/Dashboard';
import Attendance from './pages/Attendance';
import Leaves from './pages/Leaves';
import LeaveRequests from './pages/LeaveRequests';
import Employees from './pages/Employees';
import Announcements from './pages/Announcements';
import Trainings from './pages/Trainings';
import Profile from './pages/Profile';

export default function App() {
  return (
    <Routes>
      <Route path="/login" element={<Login />} />
      <Route path="/register" element={<Register />} />

      <Route
        element={
          <ProtectedRoute>
            <Layout />
          </ProtectedRoute>
        }
      >
        <Route index element={<Dashboard />} />
        <Route path="attendance" element={<Attendance />} />
        <Route path="leaves" element={<Leaves />} />
        <Route
          path="leave-requests"
          element={
            <ProtectedRoute roles={['hr', 'manager']}>
              <LeaveRequests />
            </ProtectedRoute>
          }
        />
        <Route
          path="employees"
          element={
            <ProtectedRoute roles={['hr', 'manager']}>
              <Employees />
            </ProtectedRoute>
          }
        />
        <Route path="announcements" element={<Announcements />} />
        <Route path="trainings" element={<Trainings />} />
        <Route path="profile" element={<Profile />} />
      </Route>

      <Route path="*" element={<Navigate to="/" replace />} />
    </Routes>
  );
}
import axios from 'axios';

export const TOKEN_KEY = 'staffsync_token';

const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL || '/api',
});

api.interceptors.request.use((config) => {
  const token = localStorage.getItem(TOKEN_KEY);
  if (token) config.headers.Authorization = `Bearer ${token}`;
  return config;
});

// An expired / invalid token sends the person back to the login page
api.interceptors.response.use(
  (res) => res,
  (err) => {
    const isLogin = err.config?.url?.includes('/auth/login');
    if (err.response?.status === 401 && !isLogin) {
      localStorage.removeItem(TOKEN_KEY);
      if (!window.location.pathname.startsWith('/login')) window.location.href = '/login';
    }
    return Promise.reject(err);
  }
);

export const errMsg = (e) => e?.response?.data?.message || e?.message || 'Something went wrong';

export default api;
import { useState } from 'react';
import { NavLink, Outlet, useNavigate } from 'react-router-dom';
import { useAuth } from '../context/AuthContext';
import { StatusBadge } from './ui';
import { roleLabel } from '../utils';

const NAV = [
  { to: '/', label: 'Dashboard', icon: 'speedometer2', end: true },
  { to: '/attendance', label: 'Attendance', icon: 'calendar-check' },
  { to: '/leaves', label: 'My Leaves', icon: 'airplane' },
  { to: '/leave-requests', label: 'Leave Requests', icon: 'inbox', roles: ['hr', 'manager'] },
  { to: '/employees', label: 'Employees', icon: 'people', roles: ['hr'] },
  { to: '/employees', label: 'My Team', icon: 'people', roles: ['manager'] },
  { to: '/announcements', label: 'Announcements', icon: 'megaphone' },
  { to: '/trainings', label: 'Training', icon: 'mortarboard' },
  { to: '/profile', label: 'My Profile', icon: 'person-circle' },
];

export default function Layout() {
  const { user, logout } = useAuth();
  const navigate = useNavigate();
  const [open, setOpen] = useState(false);

  const items = NAV.filter((n) => !n.roles || n.roles.includes(user.role));

  const handleLogout = () => {
    logout();
    navigate('/login');
  };

  return (
    <div className="app-shell">
      <aside className={`sidebar ${open ? 'open' : ''}`}>
        <div className="brand">
          <span className="brand-mark">S</span>
          <span className="fw-semibold fs-5">StaffSync</span>
        </div>
        <nav className="nav flex-column gap-1 px-2">
          {items.map((n) => (
            <NavLink
              key={n.label}
              to={n.to}
              end={n.end}
              className="side-link"
              onClick={() => setOpen(false)}
            >
              <i className={`bi bi-${n.icon}`} />
              <span>{n.label}</span>
            </NavLink>
          ))}
        </nav>
        <div className="mt-auto p-3">
          <button className="btn btn-outline-light btn-sm w-100" onClick={handleLogout}>
            <i className="bi bi-box-arrow-right me-2" />
            Logout
          </button>
        </div>
      </aside>
      {open && <div className="sidebar-scrim" onClick={() => setOpen(false)} />}

      <div className="main">
        <header className="topbar">
          <button className="btn btn-light d-lg-none" onClick={() => setOpen(true)} aria-label="Open menu">
            <i className="bi bi-list fs-5" />
          </button>
          <div className="ms-auto d-flex align-items-center gap-3">
            <div className="text-end lh-sm d-none d-sm-block">
              <div className="fw-medium">{user.name}</div>
              <small className="text-muted">{user.designation}</small>
            </div>
            <StatusBadge value={user.role} label={roleLabel[user.role]} />
            <span className="avatar">{user.name.charAt(0).toUpperCase()}</span>
          </div>
        </header>
        <main className="content">
          <Outlet />
        </main>
      </div>
    </div>
  );
}
