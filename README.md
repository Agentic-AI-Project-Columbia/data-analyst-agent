# Airbnb Analytics Agent for Business Decision-Making

## Project Overview

This project is a **multi-agent data analysis web application** that helps users analyze Airbnb market data for business decision-making.

The system is designed to answer questions related to:

* pricing
* demand
* investment potential

It follows the first three steps of a real data analysis lifecycle:

1. **Collect** relevant real-world data at runtime
2. **Perform exploratory data analysis (EDA)** using tools
3. **Generate a hypothesis or recommendation** supported by evidence

This directly matches the project requirement that every submission must implement **Collect → EDA → Hypothesize**. 

---

## Why We Chose This Project

We want to build something that is:

* realistic
* analytically meaningful
* easy to explain in a demo
* aligned with business use cases

Airbnb data is a strong fit because it supports questions like:

* Which neighborhoods are expensive?
* Which areas show strong demand?
* Where should someone invest in Airbnb listings?

---

## Scope

We will focus on **one city only** for the demo, such as **New York City**.

We will use the following datasets from **Inside Airbnb**:

* `listings.csv` 
* `calendar.csv` 

Optional:

* `reviews.csv` 

### Dataset roles

* **Listings**: static listing features such as neighborhood, room type, price, and host attributes
* **Calendar**: daily availability and price, used to estimate demand and seasonality
* **Reviews (optional)**: sentiment or review-frequency analysis

---

## Core Requirements We Must Satisfy

The course project requires:

* a frontend
* an agent framework
* tool calling
* a non-trivial real dataset retrieved at runtime
* a multi-agent pattern
* deployment
* a README explaining how the three steps are implemented 

This project is designed specifically to satisfy all of those.

---

## Tech Stack

We will use:

* **Backend / frontend server:** FastAPI
* **Agent framework:** LangGraph
* **LLM access layer:** OpenAI SDK
* **Main model:** Gemini
* **EDA / analysis:** pandas + DuckDB
* **Data source:** Inside Airbnb
* **Deployment:** Render / Railway / similar FastAPI-friendly hosting


## Agent Architecture

We will implement a **multi-agent pattern**, since the assignment explicitly requires at least two distinct agents with different responsibilities. 

### Agents

#### 1. Orchestrator Agent

Responsible for:

* receiving the user query
* deciding the main business intent
* routing to the correct specialist agent

#### 2. Pricing Agent

Handles questions about:

* price by neighborhood
* price by room type
* pricing differences
* overpriced / underpriced areas

#### 3. Demand Agent

Handles questions about:

* availability
* occupancy proxy
* seasonality
* demand trends over time

#### 4. Investment Agent

Handles questions about:

* revenue potential
* best neighborhoods to invest in
* supply-demand opportunity

---

## Multi-Agent Pattern

We will use this workflow:

**User Query**
→ **Orchestrator Agent**
→ **Specialist Agent (Pricing / Demand / Investment)**
→ **Tool Calls for EDA**
→ **Hypothesis / Recommendation**

This satisfies the requirement for a true multi-agent pattern rather than a single chatbot. 

---

## Tool Calling

The project must use at least one tool call, and EDA must involve tool usage on collected data. 

We will implement analytical tools as Python functions.

### Required tools

#### Data loading

* `load_listings_data(city: str)` 
* `load_calendar_data(city: str)` 

#### Pricing tools

* `get_avg_price_by_neighborhood(city: str)` 
* `get_price_by_room_type(city: str)` 

#### Demand tools

* `compute_occupancy_proxy(city: str)` 
* `demand_by_month(city: str)` 

#### Investment tools

* `estimate_revenue_potential(city: str)` 
* `compare_neighborhood_opportunities(city: str)` 

### Important note

Calendar unavailability is only a **proxy** for occupancy.
Unavailable dates may mean:

* booked
* blocked by host

So the system must clearly communicate this limitation.

---

## How We Implement the Three Required Steps

### 1. Collect

The system retrieves Airbnb data at runtime from a real external dataset source.

We will:

* download or load the data dynamically
* avoid hard-coding any final answers into prompts
* retrieve only the data needed for the question

This satisfies the **Collect** requirement. 

---

### 2. Explore and Analyze (EDA)

The system must not jump straight to an answer.

It must:

* call at least one analysis tool
* compute statistics or trends
* inspect the data before concluding

Examples:

* grouped average price by neighborhood
* demand by month
* occupancy proxy
* revenue estimate

This satisfies the **EDA** requirement. 

---

### 3. Hypothesize

After analysis, the system will generate a business-oriented conclusion supported by evidence.

Examples:

* "Brooklyn shows higher demand relative to price, suggesting better investment opportunity."
* "Manhattan has the highest prices but weaker relative occupancy, suggesting possible pricing inefficiency."

This satisfies the **Hypothesize** requirement. 

---

## Example User Questions

The system should support questions like:

* Which neighborhoods have the highest Airbnb prices?
* Which neighborhoods show the strongest demand?
* How does demand vary by month?
* Which room type is most expensive?
* Where should I invest in Airbnb and why?
* Which areas have high price but weak demand?
* Which neighborhoods have the strongest revenue potential?

---

## API Design (FastAPI)

### Main endpoints

#### `GET /` 

Returns a basic web page or landing page

#### `POST /analyze` 

Accepts:

* user question
* city

Runs:

* orchestrator
* specialist agent
* relevant tool calls
* final hypothesis generation

Returns:

* selected agent
* metrics / tool outputs
* final answer

#### Optional endpoints

* `GET /health` 
* `GET /cities` 
* `GET /example-questions` 

---

## Proposed File Structure

```text
project/
│
├── app/
│   ├── main.py
│   ├── routes.py
│   └── schemas.py
│
├── agents/
│   ├── orchestrator.py
│   ├── pricing_agent.py
│   ├── demand_agent.py
│   └── investment_agent.py
│
├── tools/
│   ├── data_loader.py
│   ├── pricing_tools.py
│   ├── demand_tools.py
│   └── investment_tools.py
│
├── graph/
│   └── workflow.py
│
├── templates/
│   └── index.html
│
├── static/
│   └── style.css
│
├── utils/
│   ├── config.py
│   └── helpers.py
│
├── README.md
├── requirements.txt
└── .env.example
```

---

## Team Split

To avoid overlap, responsibilities should be separated clearly.

### Person A — Data + EDA Layer

Owns:

* data loading
* data cleaning
* analysis tools
* grouped metrics
* pandas / DuckDB logic

Files:

* `tools/data_loader.py` 
* `tools/pricing_tools.py` 
* `tools/demand_tools.py` 
* `tools/investment_tools.py` 

### Person B — Agent + FastAPI Layer

Owns:

* LangGraph workflow
* orchestrator and specialist agents
* API endpoints
* request/response flow
* basic frontend page
* final response formatting

Files:

* `agents/*` 
* `graph/workflow.py` 
* `app/*` 
* `templates/*` 

---

## Development Plan

### Phase 1

* finalize scope
* pick one city
* load data
* implement basic tools

### Phase 2

* build orchestrator
* build one specialist agent
* connect tools to agent flow

### Phase 3

* build FastAPI routes
* add simple frontend page
* test full pipeline end-to-end

### Phase 4

* improve outputs
* deploy
* finalize README

---

## Acceptance Criteria

The project is complete only if:

1. A user can interact with it through a web interface
2. It uses an actual agent framework
3. It uses a multi-agent pattern
4. It retrieves Airbnb data at runtime
5. It performs at least one EDA tool call before answering
6. It returns a business-oriented hypothesis supported by analysis
7. It is deployed
8. The README clearly explains how requirements are met

---

## Final Positioning

This is **not** just a chatbot.

It is a **multi-agent Airbnb analytics system** that:

* collects real data
* performs EDA with tools
* generates stakeholder-facing insights

That is the main framing we should keep consistent in the code, README, and demo.
