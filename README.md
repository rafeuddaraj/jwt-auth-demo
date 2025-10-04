JWT Auth Demo
=============

Lightweight demo showing how to implement JWT-based authentication with Node.js, Express and MongoDB.

Why this repo
-------------
- Small, focused example for learning JWT auth flows: register, login, and a protected profile route.
- Uses bcrypt for secure password hashing and jsonwebtoken for token creation/verification.

Requirements
------------
- Node.js (16+ recommended)
- pnpm, npm or yarn (pnpm used for the lockfile here)
- A running MongoDB instance (local or hosted)

Environment variables
---------------------
Create a `.env` file in the project root with the following values:

- `PORT` (optional) - server port, default 5000
- `MONGO_URI` - MongoDB connection string
- `JWT_SECRET` - secret string used to sign JWT tokens (keep this private)

Example `.env`:

```
PORT=5000
MONGO_URI=mongodb://localhost:27017/jwt-demo
JWT_SECRET=your-super-secret-key
```

Quick start
-----------
1. Install dependencies:

```bash
pnpm install
# or `npm install` / `yarn install`
```

2. Start the server in development mode (uses nodemon):

```bash
pnpm run dev
# or `npm run dev` if you use npm
```

3. Server will be available at http://localhost:5000 (or your `PORT`).

Project layout (important files)
--------------------------------
- `server.js` — app entry point
- `routes/auth.js` — register/login/profile routes
- `controllers/authController.js` — request handlers
- `services/hashService.js` — bcrypt helpers
- `services/jwtService.js` — token helpers
- `models/User.js` — Mongoose user model
- `middlewares/authMiddleware.js` — protects routes by verifying JWT

API reference (examples)
------------------------
Base path: `/api/auth`

1) Register

- Endpoint: POST /api/auth/register
- Body (JSON): { name, email, password }
- Response: 201 on success

Example (curl):

```bash
curl -X POST http://localhost:5000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"name":"Sumit Saha","email":"sumit@saha.com","password":"secret"}'
```

2) Login

- Endpoint: POST /api/auth/login
- Body (JSON): { email, password }
- Response: { message, token }

Example (curl):

```bash
curl -X POST http://localhost:5000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"sumit@saha.com","password":"secret"}'
```

3) Profile (protected)

- Endpoint: GET /api/auth/profile
- Requires `Authorization: Bearer <token>` header

Example (curl):

```bash
curl http://localhost:5000/api/auth/profile \
  -H "Authorization: Bearer <TOKEN_FROM_LOGIN>"
```

Notes and best practices
------------------------
- Use a strong, unpredictable `JWT_SECRET` and keep it out of source control.
- In production, set token expiration and consider refresh tokens for long sessions.
- Hash passwords (already done with bcrypt here) and never return raw password fields in responses.
- Consider adding rate limiting and account lockout to mitigate brute-force attacks.

Testing
-------
- You can test the endpoints with curl, httpie, Postman, or a small test script.

License & credits
-----------------
This demo is provided for educational purposes. Feel free to reuse and adapt.
