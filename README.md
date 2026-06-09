# Canvasflow

A collaborative drawing app prototype with a Next.js frontend, REST API, and WebSocket backend.

## Overview

This repository is a monorepo for a realtime collaborative whiteboard-style app.
It combines a browser-based drawing client with a REST backend for room/chat history and a WebSocket server for live sync.

The project is useful as a developer prototype for shared canvas collaboration, room-based sessions, and a Turbo/`pnpm` monorepo structure.

## Features

- Real-time shared canvas using WebSocket messages
- Room-based sessions for separate drawing spaces
- Persisted message/history retrieval via HTTP API
- Basic drawing tools for rectangles and circles
- Shared UI and backend packages in a monorepo

## Tech Stack

- Next.js 15 + React 19 for the frontend
- Tailwind CSS for styling
- Express for REST endpoints
- `ws` WebSocket server for realtime collaboration
- Prisma + PostgreSQL for persistence
- `pnpm` and Turbo for monorepo management

## Installation

1. Clone the repository

```bash
git clone https://github.com/hrshya/canvasflow
cd canvasflow
```

2. Install dependencies

```bash
pnpm install
```

3. Create environment variables

Create a `.env` file in the repository root with at least:

```env
DATABASE_URL="postgresql://user:password@localhost:5432/database"
JWT_SECRET="123123"
```

4. Prepare the database

```bash
cd packages/db
pnpm exec prisma migrate dev --name init
```

5. Start the project locally

```bash
cd ../../
pnpm dev
```

### Alternate startup

Run services individually if needed:

```bash
pnpm --filter frontend dev
pnpm --filter http-backend dev
pnpm --filter ws-backend dev
```

## Usage

- Open the frontend at `http://localhost:3000`
- Use `/canvas/<roomId>` to open a room, for example `http://localhost:3000/canvas/1`
- Draw shapes on the canvas and share the URL with other connected clients
- The frontend loads room history from the HTTP API and receives live updates over WebSocket

> Note: the current frontend includes a development token in `apps/frontend/components/RoomCanvas.tsx`. Replace it with a real auth token before using the app in a production context.

## Project Structure

- `apps/frontend/` — main drawing client
  - `app/` — Next.js routes and pages
  - `components/` — UI and canvas components
  - `draw/` — drawing engine and helpers
- `apps/http-backend/` — REST API for signup, signin, room lookup, and chat history
- `apps/ws-backend/` — WebSocket server for live room messaging
- `packages/backend-common/` — shared backend configuration
- `packages/db/` — Prisma client and database schema
- `packages/common/` — shared types and validation schemas
- `packages/ui/` — shared UI primitives

## Configuration

| Variable | Purpose | Required |
|---|---|---|
| `DATABASE_URL` | PostgreSQL connection string | yes |
| `JWT_SECRET` | JWT signing secret | no (defaults to `123123`) |

The frontend currently uses hardcoded backend URLs in `apps/excelidraw-frontend/config.ts`:

- `HTTP_BACKEND = http://localhost:3001`
- `WS_URL = ws://localhost:8080`

## Future Improvements

- Replace the hardcoded WebSocket auth token with a real login flow
- Hash passwords and strengthen authentication
- Add room creation and join UI
- Implement pencil/freehand drawing and more shape tools
- Add better error handling and sync conflict resolution

## Contributing

This is a personal prototype, but contributions and ideas are welcome. Open an issue or submit a PR if you want to improve the app.

## Acknowledgements

- Next.js
- Prisma
- Express
- ws
- Tailwind CSS