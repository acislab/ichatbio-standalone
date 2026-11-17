# Quickstart

Uses `docker compose` to deploy a local iChatBio instance.

## Requirements

This accesses private GitHub repos, so you'll need an SSH key and a running SSH agent. You can use `keychain` to start the agent:

```
sudo apt update && sudo apt install keychain
eval $(keychain --eval --agents ssh id_rsa)
```

Replace `id_rsa` if your key has a different name (e.g. `id_ed25519`).

You can add that `eval ...` line to your `~/.bashrc` or `~/.zshrc` (etc.) to make sure the SSH agent is always running.

## Run

Use `.env` to set your LLM credentials:

```env
# .env
LLM_BASE_URL=
LLM_API_KEY=
```

Then run everything:

```bash
docker compose up --build
```

By default, this will run the backend, frontend, and a handful of agents. This is defined by `COMPOSE_PROFILES` in [.env.defaults](.env.defaults). You can use `.env` to override any default variables without upsetting git.

All services will read `.env.defaults` and `.env`, but if needed, you can set per-service environments by creating the following files:
* `backend.env`
* `frontend.env`
* `agents.env`
