# AI-debate 🤖⚛️  
[![Python](https://img.shields.io/badge/python-3.10%2B-blue.svg)](https://www.python.org/)

**Prototype – AI Debate for particle-physics strategy (P5 2023 + FCC 2025, neutral multi-corpus)**

This project explores domain-specific decision-making with automated judging and diverse opening arguments.

- Large-language-model **“Strategy 1 vs Strategy 2”** debates over a **combined evidence context**: the US **[P5 (2023)](https://www.usparticlephysics.org/2023-p5-report/)** report and CERN’s **[FCC Feasibility Study (2025)](https://fcc.web.cern.ch/)**.  
- Two GPT-style agents argue, an LLM judge decides who’s more persuasive (emitting a strict `WINNER: Strategy-1` / `WINNER: Strategy-2` line), and the whole exchange (with token stats + context provenance) is logged to disk.
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

#   Run a prototype debate
python scripts/run_debate.py --repeats 1 --turns 2 --quiet --out runs/YYYYMMDD/test.json
```

---

## Repo layout 

```
llm-debate/              
│
├─ src/ai_debate_p5/       – importable package
│   ├─ debate_engine.py    – debate loop & openings
│   ├─ judge_module.py     – impartial LLM judge
│   ├─ stats_module.py     – global counters / helpers
│   └─ stats/elo_bt.py     – Bradley–Terry fitter + win matrix
│
├─ scripts/                – CLI entry points
│   └─ run_debate.py
│
├─ docs/
│   └─ p5_summary.txt
│   └─ fcc_summary.txt    
    
├─ runs/                   – outputs (logs, stats, Elo CSVs)
├─ debate_sample
├─ requirements.txt
└─ README.md               
```

---
## References

- **US P5 Report (2023):** [https://www.usparticlephysics.org/2023-p5-report/](https://www.usparticlephysics.org/2023-p5-report/)
- **FCC Feasibility Study (2025):** See: see CERN’s overview announcing the report (links to Vols 1–3): <https://home.cern/news/news/accelerators/cern-releases-report-feasibility-possible-future-circular-collider>
- **Khan et al. (2024):** *Debating with More Persuasive LLMs Leads to More Truthful Answers*, arXiv:2402.06782 — [https://arxiv.org/abs/2402.06782](https://arxiv.org/abs/2402.06782)

