# DailyJobMatch – AI Job Matching Pipeline

**AI-driven job matching and ranking pipeline built on top of an n8n workflow.**

DailyJobMatch automates job collection, filtering, ranking, and tracking using rule-based filtering, heuristic scoring, and large language model evaluation.
The system is designed as a **multi-stage decision pipeline**, not just a job scraper.

---

## Overview

DailyJobMatch is an automated workflow that:

1. Collects job postings from LinkedIn via Apify
2. Filters and deduplicates jobs using rule-based logic
3. Applies lightweight heuristic scoring (PreScore)
4. Uses a large language model to evaluate job–CV fit
5. Ranks jobs and selects Top-N opportunities
6. Saves results to a Notion database
7. Logs pipeline metrics (scraped, filtered, scored, selected)
8. Provides a webhook endpoint for future full-stack integration

The goal of this project is to explore how AI agents and workflow automation can be combined to build a **decision-support pipeline**, not just a scraper.

---

## Architecture

The system is designed as a multi-stage filtering and ranking pipeline:

```
LinkedIn Jobs (Apify)
        ↓
Filter & Deduplicate
        ↓
PreScore (rule-based scoring)
        ↓
Prepare Prompt Input
        ↓
LLM Job Scoring (Groq)
        ↓
Ranking & Top N
        ↓
Notion Database
        ↓
Logging & Metrics
        ↓
Webhook / API Trigger
```

### Design Goals

* Reduce unnecessary LLM usage
* Improve job relevance through multi-stage filtering
* Provide structured scoring output
* Enable full automation
* Prepare for future full-stack integration

---

## Key Features

### Multi-stage Filtering

Jobs are filtered through multiple stages:

* Rule-based filtering (internships, temporary roles, etc.)
* Visa sponsorship filtering
* Deduplication using hash keys
* Lightweight heuristic scoring before LLM evaluation

### Heuristic Pre-Scoring

The system assigns a **PreScore** based on:

* Keywords relevance
* Location preference
* Language requirements
* Seniority level
* Visa sponsorship signals

This reduces LLM calls and improves ranking efficiency.

### LLM Job–CV Matching

A large language model evaluates each job against the CV across multiple dimensions:

* Background match
* Skills overlap
* Experience relevance
* Seniority fit
* Language requirements
* Company/industry fit

The model returns structured JSON scores for automated ranking.

### Logging & Metrics

Each workflow run records:

* Number of jobs scraped
* Jobs after filtering
* Jobs evaluated by LLM
* Final Top-N jobs

This helps monitor pipeline performance and optimize filtering thresholds.

### Webhook Integration

A webhook endpoint was added to allow external triggering of the workflow.
This is the first step toward turning the system into a **full-stack AI job management application**.

---

## Tech Stack

* **n8n** – Workflow automation
* **Apify** – LinkedIn job scraping
* **Groq / LLM** – Job–CV semantic evaluation
* **Notion API** – Job tracking database
* **Google Drive API** – CV retrieval
* **Docker** – Deployment
* **JavaScript** – Filtering, scoring, logging logic
* **Webhooks** – External triggering / future API integration

---

## Setup

### Run n8n with Docker

```bash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  docker.n8n.io/n8nio/n8n
```

Open:

```
http://localhost:5678
```

Import workflow:

```
workflow/Daily_Job_Match.json
```

---

## Configuration

Create a `.env` file based on `.env.example`:

```
APIFY_API_TOKEN=
GROQ_API_KEY=
NOTION_API_KEY=
GOOGLE_DRIVE_FILE_ID=
```

In the Config node you can set:

* Keywords
* Location
* Preferred languages
* Seniority level
* Top N jobs
* LinkedIn filters

---

## Logging & Metrics

Each workflow run logs:

| Metric      | Description           |
| ----------- | --------------------- |
| Scraped     | Total jobs collected  |
| Filtered    | Jobs after filtering  |
| LLM         | Jobs evaluated by LLM |
| Final       | Top N selected jobs   |
| Filter Rate | Filtered / Scraped    |
| LLM Rate    | LLM / Filtered        |
| Final Rate  | Final / LLM           |

This allows monitoring and optimization of the pipeline.

---

## Roadmap

Future development plans:

* [x] Job scraping automation
* [x] Rule-based filtering
* [x] Heuristic pre-scoring
* [x] LLM job matching
* [x] Notion database tracking
* [x] Logging and metrics
* [x] Webhook integration
* [ ] Frontend dashboard
* [ ] Backend API service
* [ ] Automated application assistant
* [ ] Interview tracking system
* [ ] Full-stack job management platform

---

## My Extensions (Compared to Original Workflow)

This project is based on an existing open-source n8n workflow and significantly extended.

Major additions and improvements include:

* Rule-based filtering and visa sponsorship filtering
* Deduplication using hashing
* Heuristic pre-scoring and ranking logic
* Structured LLM evaluation and scoring system
* Prompt engineering for job–CV matching
* Top-N ranking logic
* Logging and pipeline metrics
* Token optimization for LLM prompts
* Webhook integration for future full-stack development
* System architecture redesign into a multi-stage decision pipeline

The focus of this project is **system design, scoring logic, automation architecture, and extensibility**.

---

## License

This project is based on an open-source project licensed under the MIT License.
Modifications and extensions were made by Garbo.

See LICENSE file for original license.

---

## Project Summary

This project demonstrates:

* Workflow automation
* AI-assisted decision systems
* Heuristic ranking algorithms
* Prompt engineering
* LLM structured outputs
* System integration and orchestration
* Pipeline logging and metrics
* Extensible architecture for full-stack applications


---

如果这是放在 GitHub 上用于求职项目，这个 README 的定位已经是：

**AI workflow / LLM system / automation platform prototype**

而不是普通脚本项目，这一点很重要。
