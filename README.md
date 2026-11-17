# Quickstart

Uses `docker compose` to deploy a local iChatBio instance.

## Requirements

Requirements:
* [Docker compose](https://docs.docker.com/compose/install/)
* [git](https://git-scm.com/install/)
* [A GitHub account with an SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent?platform=linux)

First clone this repository:

```bash
git clone https://github.com/acislab/ichatbio-standalone.git && cd ichatbio-standalone
```

The iChatBio backend and frontend are private GitHub repos, so you'll need an SSH key and a running SSH agent to access them. You can use `keychain` to start the SSH agent:

```
sudo apt update && sudo apt install keychain
eval $(keychain --eval --agents ssh id_rsa)
```

Replace `id_rsa` if your key has a different name (e.g. `id_ed25519` in the `~/.ssh/` directory).

You can add that `eval ...` line to your `~/.bashrc` or `~/.zshrc` (etc.) so you don't have to repeat this step every time you open a new terminal.

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

### Connecting a local agent

Edit `backend.toml`:

```toml
agents = [
    { id = "data_handler", url = "http://localhost:9090/" },
    { id = "map_maker", url = "http://localhost:9091/" },
    { id = "idigbio", url = "http://localhost:9092/" },
    { id = "gbif", url = "http://localhost:9093/" },
    { id = "YOUR_AGENT", url = "http://localhost:YOURPORT/" }
]
```

You can remove the other agents if they're not needed, but it's a good idea to keep the `data_handler`. To deploy without the other agents, add this to your `.env`:

```env
COMPOSE_PROFILES=backend,frontend,data_handler
```

Notice that this uses the `data_handler` profile instead of `agents` (which includes the data handler agent).
