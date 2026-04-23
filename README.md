# litchain

Chainlit-based AI assistant with [MCP (Model Context Protocol)](https://modelcontextprotocol.io) support, built on the Anthropic Claude API. Deployed on Google Cloud Run.

## What it does

- Users can connect any MCP server (stdio or SSE transport) directly from the chat UI
- Exposes MCP tools to Claude so the LLM can call them during conversations
- Tracks token usage with `tiktoken` to stay within context limits
- Supports **password auth** and **OAuth** (Google) out of the box
- Persists conversation threads and message history via a Postgres-backed Chainlit data layer ([chainlit-datalayer](https://github.com/riekert7/chainlit-datalayer/tree/develop))

## Stack

| Component | Technology |
|-----------|-----------|
| UI framework | Chainlit (custom build — see below) |
| LLM | Anthropic Claude via `anthropic` SDK |
| MCP client | `mcp` Python SDK |
| Token counting | `tiktoken` |
| Deployment | Google Cloud Run |

## Custom Chainlit build

This app depends on a fork of Chainlit ([riekert7/chainlit — develop](https://github.com/riekert7/chainlit/tree/develop)) that packages as a universal `.whl`. The built wheel is included in the repo so no build step is needed locally.

## Running locally

```bash
pip install -r requirements.txt
cp .env.example .env  # fill in ANTHROPIC_API_KEY, CHAINLIT_AUTH_SECRET, OAuth credentials
chainlit run app.py
```

## Deployment

```bash
./build.sh   # builds and tags the Docker image
./deploy.sh  # deploys to Google Cloud Run
```

The `CHAINLIT_URL` environment variable must be set to the Cloud Run service URL so OAuth redirects and MCP SSE connections resolve correctly behind the proxy.
