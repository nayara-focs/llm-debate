# AI Debate for Science: A Case Study in Particle Physics

### Companion code and minimal data for our paper on structured LLM debate for particle-physics strategy; includes scripts to run the debates and compute win rates/Elo.

TL;DR: A reproducible LLM-debate protocol for science-strategy choices, tested on particle-physics roadmaps. We randomize context order, balance label assignment, compare a best-of-N opening vs temperature-controlled debaters, and report stance win rates and Elo-like (Bradley–Terry) ratings to support expert deliberation.

--- 

## How it works – high-level flow

``` 
P5 + FCC (static context) ──► Neutral initial prompt
                               "Given P5 (2023) and FCC (2025), what’s the best
                                strategy for HEP in the next decade?"

                   ┌──────────────── diverse openings ────────────────┐
                   │  Strategy-1 Opening A   Strategy-1 Opening B  …  │
                   │  Strategy-2 Opening A   Strategy-2 Opening B  …  │
                   └──────────────────────────────────────────────────┘
                                        │ pairwise
                                        ▼
Debate Matches  (A1 vs B1, A1 vs B2, …) – multi-round back-and-forth
                                        │
                                        ▼
LLM Judge  →  WINNER: Strategy-1 / WINNER: Strategy-2  +  rationale
                                        │
                                        ▼
Quant stats  →  win-rate · average tokens / turn · Elo (BT) 
```

---

## Quick start

```bash

### Install and set your key

# create env and install
pip install -r requirements.txt

# set your key (Unix/macOS)
export OPENAI_API_KEY=sk-...

# or use the .env file
cp .env.example .env
# edit .env and add: OPENAI_API_KEY=sk-...

# Run a prototype debate (Choose the roster and context in config.py)
python scripts/run_debate.py \
  --repeats 6 \
  --turns 6 \
  --context-order random \
  --seed 42 \
  --ctx-p5  contexts/p5_summary.txt \
  --ctx-fcc contexts/fcc_summary.txt \
  --quiet \
  --out runs/$(date +%Y%m%d)/test.json
```

---

## Repo layout 

```
llm-debate/              
│
├─ src/ai_debate/       
│   ├─ debate_engine.py    – debate loop & openings
│   ├─ judge_module.py     – LLM judge
│   ├─ stats_module.py     – global counters / helpers
│   └─ stats/elo_bt.py     – Bradley–Terry fitter + win matrix
│
├─ scripts/
|   ├─ compute_elo.py           
│   └─ run_debate.py
│
├─ docs/                   - source samples
│   └─ p5_summary.txt
│   └─ fcc_summary.txt
|   └─ context_p5_plus_fcc.txt
|  
├─ runs/                   – outputs (logs, stats, Elo CSVs)
|   └─ debate_example.json
|
├─ requirements.txt
├─ config.py
└─ README.md               
```


