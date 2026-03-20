## Description
This repo is a comprehensive set of agentic flow patterns built with **picoflow Agentic Flow**. The included `BasicFlow` showcases step orchestration, scoped memories, model routing, prompt-driven logic blocks, and concurrent fan-out spawning—intended as a starting point for your own production flows rather than the old hotel-reservation demo.

## What's inside
- NestJS API (`/ai/run`, `/ai/end`) wrapped around picoflow `FlowEngine`.
- `BasicFlow` illustrating: multi-model selection, shared vs. isolated memories, in-context prompt steps, conditional/personalized branches (e.g., `config.isPresident`), and concurrent batches via `spawnSteps`.
- Swagger UI auto-generated at http://localhost:8000/api for quick exploration.

## Quick start
```bash
yarn install
cp .env-example .env   # fill in keys below
```

### Environment variables
Fill these in `.env` (see `.env-example`):
```text
PICOFLOW_KEY=          # request a trial at dev@picoflow.io
OPENAI_KEY=
GEMINI_KEY=
ANTHROPIC_KEY=

LLM_RETRY=3
LLM_TEMPERATURE=0.2
SESSION_EXPIRATION=50000

DOCUMENT_DB=COSMO      # or set to MONGO
COSMODB_KEY=...
COSMODB_URL=http://localhost:8081/
COSMODB_ID=picoflow
COSMODB_SESSION_ID=sessions

MONGODB_NAME=picoflow
MONGODB_COLLECTION=sessions
MONGODB_URL=mongodb://localhost:27017/?directConnection=true
```
Choose either Cosmos DB (emulator or cloud) or MongoDB for session/document storage.

## Run
```bash
# watch mode for development
yarn start:dev

# or build then run compiled output
yarn start
```
The API listens on `http://0.0.0.0:8000`.

## Calling the flow
- `POST /ai/run` with body:
  ```json
  { "flowName": "BasicFlow", "message": "<user text>", "config": { "isPresident": false } }
  ```
  Include `CHAT_SESSION_ID` header on subsequent turns; omit it on the first call to have one created for you (returned in the response headers).
- `POST /ai/end` ends a session; requires the same `CHAT_SESSION_ID`.

## Where to look next
- `src/myflow/basic-flow/basic-flow.ts` — step wiring, model usage, concurrent spawning.
- `src/myflow/basic-flow/prompt/*` — prompt and in-context assets used by the flow.
- `src/controllers/chat-controller.ts` — API surface and flow registration.

