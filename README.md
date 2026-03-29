# Sportz

A lightweight sports match management API with real-time WebSocket notifications.

## 🚀 Overview

`Sportz` is an Express-based Node.js app (ESM) using Postgres + Drizzle ORM and Zod input validation. Features:
- Create and list matches with validation
- Match status computation (`scheduled`, `live`, `finished`)
- WebSocket broadcast for newly created matches
- Drizzle migrations and schema managed via `drizzle-kit`

## 🧩 Folder structure

- `src/index.js` - application entry point
- `src/routes/matches.js` - REST routes for `/matches`
- `src/ws/server.js` - WebSocket server at `/ws`
- `src/db/db.js` - database connection setup
- `src/db/schema.js` - Drizzle schema definitions (`matches`, `commentary`)
- `src/validation/matches.js` - Zod schemas
- `src/utils/match-status.js` - match status helper
- `src/arcjet.js` - security middleware integration
- `drizzle/` - migration assets

## 🛠️ Prerequisites

- Node.js 18+
- PostgreSQL database
- npm

## ⚙️ Setup

1. Clone repo
2. Copy `.env.example` to `.env` (or create `.env`)
3. Set env vars:

```env
DATABASE_URL=postgres://user:pass@localhost:5432/sportz
PORT=8000
HOST=0.0.0.0
```

4. Install deps

```bash
npm install
```

## 🧪 Database

- Generate schema from code disabled by default; existing schema is in `src/db/schema.js`
- Run migrations via `drizzle-kit`:

```bash
npm run db:migrate
```

- Start Drizzle studio

```bash
npm run db:studio
```

- Demo script

```bash
npm run db:demo
```

## ▶️ Start server

- Dev (auto-reload): `npm run dev`
- Prod: `npm start`

Default HTTP API base: `http://localhost:8000`
WebSocket URL: `ws://localhost:8000/ws`

## 🛰️ API

### GET /matches

List matches (request uses JSON body currently; consider switching to query params later).

Request body
```json
{"limit": 20}
```

Response
```json
{"data": [ ... ]}
```

### POST /matches

Create match

Request body

```json
{
  "sport": "football",
  "homeTeam": "A",
  "awayTeam": "B",
  "startTime": "2026-03-29T15:00:00.000Z",
  "endTime": "2026-03-29T17:00:00.000Z",
  "homeScore": 0,
  "awayScore": 0
}
```

Response
```json
{"data": {...}}
```

## 🌐 WebSocket events

Connect to: `ws://localhost:8000/ws`

On connection:
- `{"type":"welcome"}`

On new match creation (server broadcast):
- `{"type":"match created","data":{...}}`

## 🔐 Security

- `src/arcjet.js` provides optional request-level and WebSocket-level protection via `@arcjet/node`.

## 🔧 Development notes

- Input validation uses Zod and sends 400 on invalid payload
- status is computed by `getMatchStatus(startTime,endTime)`
- DB uses Drizzle ORM and `pg`

## 📌 TODOS

- add more REST methods (GET /matches/:id, PATCH/PUT, DELETE)
- include a full API docs and Postman collection
- add tests

---

*Generated for your current sportz codebase (feat/arcjet branch)*
