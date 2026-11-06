# NOTES

## What's in my CLAUDE.md, and what I left out

I kept `CLAUDE.md` to things Claude can't easily work out alone or would likely get wrong. It has a one-line description of the project. It lists the run, test and lint commands, plus how to run a single test file or test name. It sets five rules: use CommonJS, write tests with `node:test` and `supertest`, get data only through `db/store.js`, return errors as `{ error }`, and stay out of `.env`. A short architecture section explains how `server.js`, `routes/` and `db/store.js` fit together. It also warns that the in-memory store keeps its data across tests in the same run.

I left out a file-by-file list of the project, because Claude can see the files itself. I also left out every endpoint and the store's function list, because those change whenever the code does. There's nothing about how to set up Claude Code for the course, since that's a one-time task and not guidance for working on the code. There are no secrets or values from `.env`. A shorter file loads faster, and each line in it gets more attention.

## Permission rules

- **Allow:** `npm test`, `npm run lint` and `node --test`. These only read code and run checks, and I use them all the time, so a prompt each time would just slow me down.
- **Ask:** `git push`. A push sends work to a shared remote, so I want to confirm each one.
- **Deny:**
  - reading or editing `.env`
  - `git push --force` and `git push -f`
  - `rm -rf`

Without the deny rules, Claude could read `.env` while looking around the project. Any real secrets in it would then end up in the conversation, and from there they could get into logs, code or commits. A force-push could overwrite commits on the remote and lose other people's work. `rm -rf` could delete files that can't be recovered. Deny rules override allow rules, so these stay blocked even if a broader rule gets added later.

At first I also had a rule blocking `.env.*`. I removed it because it matched `.env.example` too, and `CLAUDE.md` tells Claude to add new config keys to that file.

## Verification

- `/memory` shows `CLAUDE.md` loaded: _TODO, confirm in a fresh session_
- `/permissions` shows the allow, ask and deny rules: _TODO, confirm in a fresh session_
