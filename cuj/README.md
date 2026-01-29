# Rapid MVP – Classroom Presentation Randomizer

This repository contains the code for our **Rapid MVP** developed as part of **Assignment 2: Rapid MVP & CUJ Framework**.  
The goal of this sprint was to build a simple, functional web application under tight time constraints and use it as a testbed for conducting a **Critical User Journey (CUJ)** analysis.

The application is a **Classroom Presentation Randomizer** designed to help instructors manage in-class group presentations by randomly selecting teams, tracking which teams have presented, and managing presentation and Q&A timing.

---

## Code Repository

**GitHub Repository:**  
https://github.com/dcsil/PyGuard-Presentation-Randomizer

---

## Tech Stack

- **Frontend:** React (Vite)
- **Backend:** Express.js
- **Database:** SQLite (via Prisma ORM)
- **Architecture:** Decoupled frontend and backend, running locally

This structure is intentionally designed to be **local-to-cloud ready** for future assignments.

---

## Local Setup Instructions

The application consists of a decoupled frontend and backend, which must both be running locally.

### Backend Setup

```bash
cd backend
npm install
npx prisma migrate dev --name init
npm run seed
npm run dev
```

### Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

Open the app at:
```
http://localhost:5173/
```
