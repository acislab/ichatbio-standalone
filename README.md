# Quickstart

Uses `docker compose` to deploy a local iChatBio instance.

First create a .env to hold your LLM credentials:

```env
# .env
LLM_BASE_URL=
LLM_API_KEY=
```

Then run everything:

```bash
docker compose up --build
```

By default, this will run the backend, frontend, and a handful of agents. This is defined by `COMPOSE_PROFILES` in [.env.defaults](.env.defaults).

You can set per-service environments by creating the following files:
* `backend.env`
* `frontend.env`
* `agents.env`
