# Pipeline Generation & Outreach System

End-to-end automation system for sourcing, enriching, qualifying, and converting B2B prospects into outbound pipeline.

---

## Overview

Built for a boat rental marketplace. This system was designed to support a proactive supply acquisition strategy in markets where demand was growing but supply was insufficient.

Unlike markets where supply was coming in organically or through inbound channels, these were markets we intentionally targeted as a team — identifying the gap and going after it before it became a bottleneck.

To do that at scale, we needed a repeatable, automated system that could source, evaluate, and route qualified prospects into an outreach pipeline without manual research for each market.

This system replaced that manual process with a scalable, automated pipeline that integrates scraping tools, AI models, and workflow automation to enable consistent outbound execution across multiple markets simultaneously.

---

## Problem

Outbound acquisition was initially manual and fragmented:

- We detected markets with high levels of demand and low levels of supply
- Prospect research was time-consuming
- Outreach lacked consistency and personalization
- Leads were not systematically tracked

This created bottlenecks in launching outbound efforts and limited the ability to respond to demand signals in target markets.

---

## System Architecture

```mermaid
flowchart TD
    subgraph WF1["Workflow 1 — Prospect Generation & Enrichment"]
        A["Lead Sources\n(Google Maps, Apify)"] --> B["Prospect Database\n(Google Sheets)"]
        B --> C["Web Scraping\n(Firecrawl)"]
        C --> D["AI Enrichment\n(OpenAI)"]
        D --> E{"Qualified?"}
        E -->|No| X["Discarded"]
        E -->|Yes| F["Google Sheets\n(Outreach Queue)"]
        E -->|Yes| G["HubSpot CRM"]
    end

    subgraph WF2["Workflow 2 — Prospect Outreach"]
        H["Read Prospects\n(Google Sheets)"] --> I["Create Conversation\n(Kustomer)"]
        I --> J{"Has Phone\nor Email?"}
        J -->|Phone| K["Send SMS"]
        J -->|Email| L["Send Email"]
        K --> M["Update Sheet\n(Status Tracking)"]
        L --> M
    end

    F -->|Handoff| H
```

---

## Workflow Screenshots

### Workflow 1 — Prospect Generation & Enrichment
![Prospect Generation & Enrichment](docs/Prospect%20Generation%20%26%20Enrichment.png)

### Workflow 2 — Prospect Outreach
![Prospect Outreach](docs/Prospect%20Outreach.png)


### Core Components

| Layer | Description |
|---|---|
| Sourcing | Identifies relevant businesses via search-based inputs |
| Extraction | Scrapes website and public data sources |
| Enrichment | Uses AI to structure unstructured data |
| Scoring | Evaluates leads based on ICP criteria |
| Routing | Sends qualified leads into CRM |
| Execution | Triggers outreach and follow-up workflows |

---

## System Behavior

This system operates as a multi-step, event-driven workflow orchestrated through n8n.

- Each stage triggers the next through structured outputs (JSON payloads)
- Data flows between steps via API calls and transformations
- Conditional logic is used to filter, score, and route leads based on defined criteria
- Failures at any step can interrupt downstream processes, requiring retry or manual intervention

The system is designed to run in batches per market, allowing parallel execution across multiple regions.

---

## Workflow Breakdown

### 1. Prospect Sourcing
- Automated sourcing of relevant businesses based on market demand signals

### 2. Data Extraction
- Scraping of websites and public sources
- Preparation of raw data for downstream processing

### 3. AI Enrichment
Structured extraction of:
- Services
- Fleet characteristics
- Pricing signals
- Location and quality indicators

Transforms unstructured content into usable, structured data.

### 4. Qualification & Scoring
- Leads scored against ICP criteria
- Filtering logic to prioritize high-quality prospects

### 5. Personalization
- Dynamic generation of outreach messages using structured data
- Context-aware messaging (services, location, demand signals)
- See the full prompt: [prompts/enrichment-prompt.md](prompts/enrichment-prompt.md)

### 6. CRM Integration & Outreach
- Automated lead insertion into CRM
- Triggering of outreach sequences and follow-ups
- Tracking of responses and pipeline stages

---

## Tech Stack

| Category | Tools / Technologies |
|---|---|
| Automation | n8n |
| Integrations | REST APIs, webhooks |
| Data Processing | JSON transformations, structured outputs |
| AI | LLM-based extraction and enrichment (OpenAI) |
| CRM | HubSpot (lead storage and pipeline tracking) |
| Outreach Execution | Kustomer (Email + SMS delivery, conversation threading) |
| Storage / Reporting | Google Sheets |
| Scraping | Firecrawl, Apify |

---

## Results

- Generated qualified prospects per market across multiple U.S. markets  
- Reduced outbound launch time from 1–2 weeks → 1–2 days  
- Contributed to ~300 net new boats across ~30 markets (2025)  

---

## Reliability & Limitations

This system was built iteratively and surfaced several challenges:

- **Scraping variability:** Website structures required adaptive extraction logic  
- **Data consistency:** AI outputs required normalization and validation  
- **Error handling:** Retry logic is partially implemented at the workflow level, but lacks centralized error handling and fallback paths for failed API calls or incomplete data  
- **State management:** The system does not maintain persistent state across steps, which can lead to duplicated processing or missed records under failure conditions  
- **Coupling:** Certain steps are tightly connected, limiting modularity and flexibility  
- **Monitoring:** Limited observability into failures across long-running workflows  

---

## Future Improvements

- Introduce queue-based processing to decouple steps  
- Implement centralized logging and monitoring  
- Improve retry and fallback strategies  
- Add data validation layers before CRM insertion  
- Modularize workflows into independent, reusable components  

---

## Key Takeaway

This system evolved from a collection of scripts into a loosely-coupled automation pipeline, highlighting the challenges of reliability, observability, and scalability in real-world workflow systems.
