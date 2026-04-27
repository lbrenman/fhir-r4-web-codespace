# FHIR R4 Web App

> Standalone browser UI for the [fhir-r4-api-codepsace](https://github.com/lbrenman/fhir-r4-api-codepsace) mock data server. Browse, search, and manage all 15 FHIR R4 resources.

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/lbrenman/fhir-r4-web-codespace)

## Overview

A lightweight Node.js/Express server that serves a single-page web app. The server has one job: expose a `/config.js` endpoint that injects `FHIR_API_BASE_URL` and `FHIR_API_KEY` from `.env` into the browser — so no credentials are hardcoded in the HTML.

## Features

- **Dashboard** — live resource count cards for all 15 FHIR types
- **Resource tables** — paginated lists with resource-aware column rendering (status badges, dates, FHIR references, SNOMED/LOINC codes)
- **Detail view** — Summary fields + Raw JSON tabs
- **Create** — pre-filled FHIR R4 JSON templates per resource, with validation
- **Edit** — load and modify any resource's full JSON
- **Delete** — confirmation prompt before delete
- **Light / dark mode** — toggle in the sidebar footer (persisted to localStorage)
- **Mobile-responsive** — collapsible sidebar with hamburger menu

## Prerequisites

- Node.js 20+
- A running FHIR Server with API for data access

## Quick Start

### Option A — GitHub Codespaces

1. Click the **Open in GitHub Codespaces** badge
2. After setup completes, edit `.env` and set `FHIR_API_BASE_URL` to your FHIR server's API
3. Run `npm run dev`
4. The browser opens automatically to port 4000

### Option B — Local

```bash
npm install
cp .env.example .env
# Edit .env — set FHIR_API_BASE_URL and FHIR_API_KEY
npm run dev
# Open http://localhost:4000
```

## Environment Variables

| Variable | Default | Description |
|---|---|---|
| `PORT` | `4000` | Port the web app server listens on |
| `FHIR_API_BASE_URL` | `http://localhost:3000` | URL of the `fhir-r4-api` server |
| `FHIR_API_KEY` | *(required)* | API key sent as `x-api-key` header on every FHIR request |

## Codespaces Note

When both `fhir-r4-api` and `fhir-r4-web` run in the **same Codespace**, use `localhost`:
```
FHIR_API_BASE_URL=http://localhost:3000
```

When they run in **separate Codespaces** (or the API is deployed elsewhere), set `FHIR_API_BASE_URL` to the public forwarded URL of the API, e.g.:
```
FHIR_API_BASE_URL=https://<codespace-name>-3000.preview.app.github.dev
```

## Architecture

```
Browser  ──fetch──►  /config.js (Express)  ──reads──►  .env
   │
   └──fetch──►  fhir-r4-api  (separate server)
                  GET /Patient
                  POST /Observation
                  PUT /Condition/{id}
                  DELETE /Encounter/{id}
                  ...
```

The Express server only serves static files and the config endpoint. It never proxies FHIR traffic.

## Development

```bash
npm run dev    # nodemon auto-reload
npm start      # production start
```
