# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a Docker Compose setup for running [Unleash](https://github.com/Unleash/unleash/) (feature flag management). It's a fork of the upstream [Unleash/unleash-docker](https://github.com/Unleash/unleash-docker) repo, deployed to Heroku via `heroku.yml`.

## Commands

```bash
# Build and run locally
docker-compose build
docker-compose up

# Restart Unleash if DB isn't ready on first start
docker-compose restart web
```

## Architecture

- **`docker-compose.yml`** — Defines two services:
  - `web`: Unleash server (port 4242), uses image `unleashorg/unleash-server:latest` or builds from `Dockerfile`
  - `db`: PostgreSQL 17 backing database
- **`Dockerfile`** — Multi-stage Node.js build that compiles Unleash from source with pnpm, includes the `wait-for` script for startup ordering
- **`heroku.yml`** — Heroku container deployment config (builds `web` from `Dockerfile`)
- **`wait-for`** — Shell script to delay Unleash startup until the database is accepting connections (used via `command` in compose)

## Deployment

- **Heroku remote**: `heroku` → `myc-feature-flags.git` (container-based deploy via `heroku.yml`)
- **Origin**: `cerascreen/unleash-docker` on GitHub

## Default Credentials (local dev only)

- Username: `admin`
- Password: `unleash4all`
- Client API token: `default:development.unleash-insecure-api-token`

## Requirements

- Docker Engine 19.03.0+
- Docker Compose 2.0.0+ (compose file version 3.9)
