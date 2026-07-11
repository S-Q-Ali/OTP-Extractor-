# OTP Extractor

A secure authentication portal with TOTP-based two-factor authentication and GoHighLevel (GHL) OTP extraction. Users log in, set up 2FA via an authenticator app, and retrieve OTPs from their GHL email inbox.

## Project Structure

```
├── Backend/          # Express 5 REST API (Node.js)
│   ├── app.js              # Entry point
│   ├── controllers/        # Route handlers
│   ├── middlewares/        # CORS, IP extraction, shared-key auth
│   ├── routes/             # Route definitions
│   └── utils/              # Helpers, caching, user persistence
├── Frontend/         # React 19 SPA (Vite + Tailwind CSS)
│   ├── src/
│   │   ├── App.jsx               # Root component
│   │   ├── AppRouter.jsx         # Custom state-based router
│   │   ├── components/screens/   # Login, QR, TOTP, GHL, etc.
│   │   ├── components/ui/        # Reusable UI components
│   │   └── config/api.js         # API client & endpoints
│   └── index.html
```

## Features

- **Password-based login** with bcrypt hashing
- **Auto-registration** on first login
- **TOTP 2FA** using `speakeasy` (Google/Microsoft Authenticator compatible)
- **GHL OTP proxy** — fetch OTPs from GoHighLevel email inbox
- **Admin panel** — manage users (create, update, delete, reset 2FA)
- **Shared-key API authentication** on all endpoints
- **Single-origin CORS** enforcement
- **Glassmorphism UI** with animated transitions

## Tech Stack

| Layer        | Technology                                         |
| ------------ | -------------------------------------------------- |
| Frontend     | React 19, Vite 7, Tailwind CSS 4, react-hook-form  |
| Backend      | Node.js, Express 5, speakeasy, bcrypt               |
| Data         | JSON file-based persistence                         |
| Deployment   | Vercel                                              |

## API Endpoints

All endpoints (except `GET /`) require the `X-APP-KEY` header with a Base64-encoded shared key.

| Method   | Endpoint                | Description                  |
| -------- | ----------------------- | ---------------------------- |
| `GET`    | `/`                     | Health check                 |
| `POST`   | `/auth/login`           | Login or auto-register       |
| `POST`   | `/auth/verify-otp`      | Verify TOTP code             |
| `POST`   | `/auth/get-secret`      | Get user's TOTP secret       |
| `POST`   | `/ghl/get-ghl-otp`      | Fetch OTP from GHL           |
| `GET`    | `/admin/users`          | List all users               |
| `POST`   | `/admin/create-user`    | Create a user                |
| `PATCH`  | `/admin/update-user/:email` | Update user              |
| `DELETE` | `/admin/delete-user/:email` | Soft-delete user         |
| `PATCH`  | `/admin/reset-user/:email`  | Reset TOTP secret       |

## Environment Variables

### Backend

| Variable      | Description                        |
| ------------- | ---------------------------------- |
| `PORT`        | Server port (default: 3000)        |
| `CORS_URL`    | Allowed CORS origin                |
| `SHARED_KEY`  | Secret for `X-APP-KEY` auth        |
| `ADMIN_EMAIL` | Email assigned the `admin` role    |
| `GHL_OTP`     | External GHL OTP script URL        |
| `DATA_DIR`    | Custom data directory path         |

### Frontend

| Variable          | Description                 |
| ----------------- | --------------------------- |
| `VITE_API_BASE`   | Backend API base URL        |
| `VITE_SHARED_KEY` | Shared key for API auth     |

## Getting Started

### Backend

```bash
cd Backend
npm install
npm start
```

### Frontend

```bash
cd Frontend
npm install
npm run dev
```

## Application Flow

```
Login → QR Setup (2FA) → TOTP Verification → GHL Email → OTP Display
```

## Deployment

Both `Backend/vercel.json` and `Frontend/vercel.json` are configured for Vercel deployment. Each directory can be deployed independently as a Vercel project.

**Live app:** [otpsharingapp.vercel.app](https://otpsharingapp.vercel.app/)

## License

MIT
