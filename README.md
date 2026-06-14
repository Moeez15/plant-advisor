# 🌿 Plant Advisor

A conversational AI agent that gives grounded, season-aware care advice for houseplants. Powered by **Llama 3.3 70B** via Groq and built with a tool-calling agentic loop — not just a chatbot.

![Plant Advisor UI](docs/screenshot.png)

---

## What It Does

Ask about any plant in its database and the agent will:

1. **Look up the plant** — watering schedule, light needs, humidity, temperature range
2. **Check the current season** — and automatically adjust its advice (e.g., water less in winter)
3. **Synthesize a grounded answer** — citing the data it retrieved, not just general LLM knowledge

> *"How often should I water my snake plant in winter?"*
> → The agent calls `lookup_plant("snake plant")` + `get_seasonal_conditions("winter")`, then answers with specifics from both.

---

## Tech Stack

| Layer | Technology |
|---|---|
| LLM | Llama 3.3 70B Versatile (via Groq) |
| Agent loop | Custom tool-calling loop (no framework) |
| UI | Gradio |
| Plant data | 15-plant JSON database with aliases |
| Seasonal data | Spring / Summer / Fall / Winter JSON |

---

## Setup

**Requirements:** Python 3.9+

**1. Clone this repo:**

```bash
git clone <repo-url>
cd plant_advisor
```

**2. Create and activate a virtual environment:**

```bash
python -m venv .venv
source .venv/bin/activate      # Mac/Linux
.venv\Scripts\activate         # Windows
```

**3. Install dependencies:**

```bash
pip install -r requirements.txt
```

**4. Add your Groq API key:**

Copy `.env.example` to `.env` and paste in your key from [console.groq.com](https://console.groq.com):

```
GROQ_API_KEY=your_key_here
```

**5. Run the app:**

```bash
python app.py
```

Then open the local URL printed in your terminal (e.g., `http://127.0.0.1:7860`).

---

## How It Works

The agent runs a **tool-calling loop** rather than a single LLM call:

```
User message
     ↓
LLM decides which tool(s) to call
     ↓
lookup_plant()  and/or  get_seasonal_conditions()
     ↓
Tool results fed back to LLM
     ↓
LLM synthesizes a grounded response
     ↓
Final answer to user
```

The loop continues until the LLM stops requesting tools or `MAX_TOOL_ROUNDS` (5) is reached — preventing runaway loops.

---

## Supported Plants

Aloe Vera · Boston Fern · Calathea · Chinese Evergreen · Fiddle Leaf Fig · Monstera · Orchid · Peace Lily · Philodendron · Pothos · Rubber Plant · Snake Plant · Spider Plant · Succulent · ZZ Plant

Plants can be referenced by common name, scientific name, or nickname (e.g., *devil's ivy*, *mother-in-law's tongue*, *swiss cheese plant*).

---

## Project Structure

```
plant_advisor/
├── app.py          ← Gradio UI
├── agent.py        ← Tool definitions + agentic loop
├── tools.py        ← lookup_plant() and get_seasonal_conditions()
├── config.py       ← API keys, model, and settings
├── data/
│   ├── plants.json ← 15-plant database
│   └── seasons.json← Seasonal care data
└── requirements.txt
```
