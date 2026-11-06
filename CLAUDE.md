# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

A small Express (Node 22, CommonJS) REST API for users, with an in-memory data store. It's the starter project for the Claude Code course; later course projects build on it.

## Commands

- `npm run dev` — start the API on http://localhost:3000 with `node --watch` (port from `PORT` env var)
- `npm test` — run all tests with the built-in `node:test` runner
- `node --test tests/users.test.js` — run one test file; add `--test-name-pattern="404"` to run matching tests only
- `npm run lint` — ESLint (`eslint:recommended`); CI runs lint then tests on every push/PR

## Conventions

- Use CommonJS (`require` / `module.exports`), not ES modules — ESLint is set to `sourceType: "script"`.
- Write tests with `node:test` + `node:assert` and `supertest` against the exported `app`, not Jest/Mocha, and never call `app.listen()` in tests.
- Route handlers access data only through `db/store.js` functions; never keep data in route files.
- Error responses are JSON shaped `{ error: "message" }` with the right status code (400 for bad input, 404 for missing records).
- Never read or edit `.env`; add new config keys to `.env.example` instead.

## Architecture

- `server.js` builds the Express app, mounts one router per resource (`/users`, `/health`), and exports `app`. It only calls `listen()` when run directly (`require.main === module`), so tests can import it without opening a port.
- `routes/` holds one file per resource, each exporting an `express.Router()`. A new resource means a new route file plus an `app.use()` line in `server.js`.
- `db/store.js` is an in-memory stand-in for a database (module-level arrays and an id counter). Data resets on restart, and state is shared across tests in the same process — tests that create records can affect later ones.
