# Workout Auto-Progression Tracker

A full-stack fitness app that logs workouts and automatically recommends weight/rep progressions based on your performance history and experience level.

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | FastAPI, SQLAlchemy, PostgreSQL |
| Auth | JWT (python-jose + bcrypt) |
| Frontend | React Native (Expo), TypeScript, Zustand |
| Deployment | Render (backend + database) |

---

## Project Structure

```
├── backend/
│   ├── app/
│   │   ├── api/routes/       # Auth, users, exercises, workouts, progression
│   │   ├── models/           # SQLAlchemy ORM models
│   │   ├── schemas/          # Pydantic request/response schemas
│   │   ├── services/         # Business logic (progression engine, auth, etc.)
│   │   ├── database/         # DB session, init SQL, migrations
│   │   └── utils/            # JWT helpers, exercise registry
│   ├── tests/                # Pytest test suite
│   └── requirements.txt
└── frontend/
    └── WorkoutTracker/
        ├── src/
        │   ├── screens/      # Auth, Dashboard, Workout Log, History, Progression, etc.
        │   ├── components/   # Reusable workout UI components
        │   ├── services/     # API client
        │   └── store/        # Zustand state (auth, workout session)
        └── package.json
```

---

## Features

- **User authentication** — register, login, JWT-protected routes
- **Exercise library** — pre-seeded canonical exercises + custom exercise support
- **Workout logging** — log sets with weight, reps, RPE per exercise
- **Auto-progression engine** — analyzes recent sets and recommends next target weight/reps based on rep ranges and experience level (beginner / intermediate / advanced)
- **Progression dashboard** — visualize performance trends per exercise
- **Workout history** — browse past sessions

---

## Getting Started

### Prerequisites

- Python 3.10+
- PostgreSQL running locally
- Node.js 18+ and npm
- Expo Go app (for physical device testing)

---

### Backend

```bash
cd backend

# Create and activate virtual environment
python3 -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Set environment variables (optional — defaults to local Postgres)
export DATABASE_URL="postgresql://postgres:postgres@localhost:5432/workout_tracker"
export SECRET_KEY="your-secret-key"

# Start the API server
uvicorn app.main:app --reload
```

API runs at `http://localhost:8000`  
Interactive docs at `http://localhost:8000/docs`

#### Database Setup

Create the database and run the init script:

```bash
psql -U postgres -c "CREATE DATABASE workout_tracker;"
psql -U postgres -d workout_tracker -f app/database/init.sql

# Optional: seed exercise data
python seed_exercises.py
```

---

### Frontend

```bash
cd frontend/WorkoutTracker

# Install dependencies
npm install

# Start Expo
npx expo start
```

- Press `i` — iOS Simulator
- Press `a` — Android emulator
- Scan the QR code with Expo Go to run on your phone

The frontend connects to the deployed backend at `https://workout-tracker-api-uoob.onrender.com/api/v1` by default. To point it at your local backend, update `src/config.ts`.

---

## Running Tests

```bash
cd backend
source venv/bin/activate
pytest tests/
```

---

## Deployment

The backend is configured for one-click deploy on **Render** via `backend/render.yaml`. It provisions a free PostgreSQL database and web service automatically.

---

## Environment Variables

| Variable | Default | Description |
|---|---|---|
| `DATABASE_URL` | `postgresql://postgres:postgres@localhost:5432/workout_tracker` | PostgreSQL connection string |
| `SECRET_KEY` | `dev-secret-key-change-in-production` | JWT signing secret — **change in production** |
| `ENV` | `development` | Set to `production` to enforce `SECRET_KEY` |
