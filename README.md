Overview

This service ingests data from arbitrary web pages, normalises it, stores it in PostgreSQL, and exposes it through an API and a Streamlit dashboard. It uses FastAPI for the backend, Playwright for JavaScript-rendered pages, BeautifulSoup for static HTML, SQLAlchemy for persistence, and Docker/docker-compose for full containerisation. A simple scheduler demonstrates recurring scraping tasks.

Features

/scrape: Accepts a URL, chooses Playwright or BeautifulSoup automatically, extracts title, meta description, headers, internal links, table data, and raw HTML, normalises the output, stores it, and returns a summary.

/records: Returns stored scrape results with optional filtering and limit.

Scheduler: Minimal async scheduler with a sample schedule_daily_scrape(url) function.

Streamlit Dashboard: Shows record counts, sources, latest entries, filtering options, and an optional word-frequency chart.

Tests: Pytest coverage for parsing, normalisation, and a mocked insert flow.

Dockerised Stack: API and dashboard each run in separate containers with a shared PostgreSQL instance. Playwright dependencies are installed automatically.

Architecture

The API handles scraping and normalisation, then writes results to PostgreSQL. The dashboard reads from the same database and presents analytics.

Folder Structure

Clear separation of API, services, models, dashboard, tests, and infra files such as Dockerfile, docker-compose, and environment templates.

Getting Started
Requirements

Python 3.11+, Docker, Docker Compose. Playwright is installed via Python.

Local Development

Clone the repo, create a virtual environment, install dependencies, install Playwright browsers once, configure the .env, start PostgreSQL, and launch both the FastAPI server (port 8000) and Streamlit dashboard (port 8501). Tables are created automatically on first run.

Docker

Copy .env.example to .env, set values, then run docker-compose up --build. This starts the API, PostgreSQL, and the dashboard. Stop with docker-compose down.

Environment Variables

All configuration is done via .env, including database URL, Playwright mode, logging level, and scrape timeout.

API Usage
Health Check

GET /health returns {"status":"ok"}

Scrape a URL

POST to /scrape with a JSON body containing a URL. The service scrapes the page with the appropriate engine, processes the results, stores them, and returns a success response.

Retrieve Records

GET /records returns recent scraped entries, with limit and source filters.

Dashboard

Displays total records, source breakdown, latest scraped items, text search/filtering, and optional word-frequency visuals. Available at port 8501.

Playwright

Playwright handles JavaScript-rendered sites. Installed automatically in Docker; install manually with python -m playwright install in local development. Headless mode is controlled via PLAYWRIGHT_HEADLESS.

Testing

Pytest suite covers parsing, normalisation, and mocked database operations. Run with pytest -q.

Deployment

Designed for Docker-based deployment on Render, Railway, or AWS ECS/Fargate. Deploy the API and dashboard as separate services/containers, connect them to a shared PostgreSQL instance, and configure environment variables per platform. Optional scheduler services can be deployed separately.
