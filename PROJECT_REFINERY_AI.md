# Project Refinery AI

## The Mission
Build 12 working AI prototypes for refinery and trading operations — trained on real process data, connected to live market prices — as the Strategy & AI Transformation team.

Working models on real data, not assessments on slides.

---

## Context
The world's largest refining complex at Jamnagar (1.24M bpd across DTA + SEZ). A systems assessment (completed in `ai-transformation/`) identified 5 structural problems and sized the AI opportunity at $964M-$2.7B/year. Project Refinery AI is the execution of that assessment.

The 5 structural problems:
1. Islands of automation — best-in-class point solutions (Aspen, Honeywell, SAP, OSIsoft) that don't talk to each other
2. No unified data platform — everything converges in SAP BW or Excel
3. APC underperformance — controllers running at <70% uptime, losing ~$4-5M/yr per 1% downtime
4. Planning-execution disconnect — PIMS weekly plan → Excel → manual coordination → RTO offline
5. Trading flying blind — overnight batch VaR, manual market intelligence, no AI signals on $50B+ crude procurement

---

## The 12 Projects

### P1: DWSIM CLI Engine (Foundation)
**What:** Rip the open-source DWSIM process simulator (.NET) out of its GUI and wrap it as a CLI/Python API that Claude can call programmatically.
**Why:** No one has done this. A conversational AI that can run thermodynamic simulations on demand is genuinely novel. This unlocks P2, P5, P6, and P10.
**Data:** DWSIM source code from GitHub (https://github.com/DanWBR/dwsim)
**Deliverable:** Claude asks "what happens if I feed Arab Heavy at 100K bpd?" and gets back yields, temperatures, compositions.
**Maps to:** Foundation for the entire digital twin

### P2: Crude Selection Optimizer
**What:** AI that recommends the optimal crude slate given current market prices and refinery yield predictions.
**Why:** The refinery procures ~$50B+ of crude annually. Aspen PIMS runs weekly LP optimization — slow and disconnected from real-time market moves. Even a 0.5% improvement in crude selection = $250M/year.
**Data:** P1 (DWSIM yields) + EIA live prices + crude assay database (Arab Heavy, Basrah Light, Murban, Upper Zakum, etc.)
**Deliverable:** "Switch 30% of your slate from Arab Heavy to Upper Zakum this week — $2.40/bbl margin gain based on current Dubai-Brent spread."
**Maps to:** Aspen PIMS replacement | $68-181M/year

### P3: Predictive Maintenance
**What:** Detect equipment faults from sensor patterns before they cause failures or shutdowns.
**Why:** The refinery runs reactive maintenance via SAP PM. Bently Nevada vibration data and OSIsoft PI sensor data exist but aren't connected to ML. Unplanned downtime at Jamnagar's scale costs millions per day.
**Data:** Tennessee Eastman Process dataset — 52 sensors, 21 fault modes, ~150MB
**Deliverable:** Model that catches a compressor fault 45 minutes before the DCS alarm fires.
**Maps to:** SAP PM reactive culture | $36-91M/year

### P4: Soft Sensor
**What:** Predict product quality (e.g., butane content) from live process sensors, eliminating the 2-4 hour wait for lab results.
**Why:** LIMS at Jamnagar has a 2-4 hour cycle time. During that window, off-spec product could be blended into tanks, causing quality giveaway. A soft sensor gives real-time quality estimates.
**Data:** Debutanizer column dataset — 7 process variables + butane content target, ~2300 samples
**Deliverable:** Model predicts butane content within 0.5% accuracy from temperature/pressure/flow readings.
**Maps to:** LIMS lag | $23-68M/year

### P5: Blend Optimizer
**What:** Optimize product blending (gasoline, diesel) to hit spec targets (octane, sulfur, pour point) at minimum component cost.
**Why:** Honeywell Blend Control is installed but quality giveaway runs 1-3%. At Jamnagar volumes, 1% giveaway on diesel alone is significant.
**Data:** P1 (DWSIM component properties) + BIS/Euro fuel spec tables + component pricing
**Deliverable:** "Reduce premium gasoline RON giveaway from 1.2 to 0.3 points — saves $0.80/bbl."
**Maps to:** Honeywell Blend quality giveaway | $36-91M/year

### P6: Energy/Steam Optimizer
**What:** Minimize fuel gas consumption and optimize steam/power balance across boilers, turbines, and heat exchangers.
**Why:** Visual MESA is installed but boiler loading is sub-optimal. Energy is 50-60% of refinery operating cost. Even 2-3% improvement is massive at Jamnagar scale.
**Data:** LBNL industrial energy audit data + DWSIM utilities model
**Deliverable:** Optimal boiler loading schedule that saves 3% on fuel gas.
**Maps to:** Visual MESA gap | $36-91M/year

### P7: Alarm Rationalization
**What:** Cluster, prioritize, and suppress nuisance alarms using pattern recognition on alarm event data.
**Why:** DCS alarm flooding is a known issue — operators get hundreds of alarms per shift, most are noise. ISA-18.2 standard says <1 alarm per 10 minutes; most refineries run 5-10x that.
**Data:** Generate alarm sequences from TEP fault scenarios (P3 dataset)
**Deliverable:** Alarm clustering that reduces standing alarms by 60-70%, highlights the 3 that actually matter.
**Maps to:** DCS alarm flooding | $9-23M/year

### P8: Market Intelligence Engine
**What:** NLP on news articles → geopolitical signals → scenario probabilities → trading recommendations.
**Why:** Platts/Argus/Bloomberg data is manually compiled by the trading desk. No systematic AI processing of market-moving information.
**Data:** Already built in oil-dashboard — 149 articles scored, 5 scenarios, softmax probabilities, Claude Haiku scoring.
**Deliverable:** Already live at https://parthreddy123.github.io/oil-dashboard/
**Maps to:** Platts/Argus manual extraction | $14-36M/year

### P9: Real-Time Risk Dashboard
**What:** Live VaR (Value at Risk), position tracking, crude cargo exposure, and hedging recommendations.
**Why:** ETRM system runs overnight batch VaR. For a business with $50B+ crude exposure, the trading desk has no real-time view of risk.
**Data:** yfinance futures (CL=F, BZ=F, HO=F, RB=F) + simulated portfolio positions
**Deliverable:** Real-time dashboard showing portfolio VaR, Greeks, and "your Dubai exposure increased $12M since this morning."
**Maps to:** ETRM overnight batch VaR

### P10: APC Self-Tuning Simulator
**What:** Simulate an Advanced Process Control (APC) controller, then build an AI agent that detects when it's degrading and recommends retuning.
**Why:** Aspen DMC3 and Honeywell RMPCT controllers run at <70% uptime with manual retuning. Every 1% APC downtime = ~$4-5M/yr in lost optimization value. This is the single fastest payback.
**Data:** TEP dataset + control theory simulation (PID/MPC loops)
**Deliverable:** AI that says "CDU APC controller 3 has degraded 15% over the last 72 hours — here's the recommended model update."
**Maps to:** DMC3/RMPCT <70% uptime | $45-113M/year

### P11: Demand Forecasting
**What:** Predict Indian fuel demand by product (diesel, petrol, LPG, ATF), region, and season using ML.
**Why:** SAP APO forecast accuracy is <80%. Demand drives production planning, distribution, and pricing. Better forecasts = less inventory, fewer stockouts.
**Data:** PPAC monthly consumption data (5+ years historical), seasonal patterns, GDP correlation
**Deliverable:** Model that forecasts monthly Indian diesel demand within 5% accuracy, 3 months ahead.
**Maps to:** SAP APO <80% accuracy

### P12: Digital Twin Dashboard
**What:** Unified cockpit that ties all 11 projects into a single view — the refinery's nervous system.
**Why:** PI Vision has multiple disconnected dashboards. No single screen shows the refinery's health, margins, risks, and opportunities together.
**Data:** Outputs from P1-P11
**Deliverable:** One screen: live crude prices → DWSIM yields → product margins → quality predictions → equipment health → trading risk → demand forecast.
**Maps to:** PI Vision disconnected dashboards | $23-45M/year

---

## Total Addressable Value
$358M - $964M/year across the 12 initiatives (conservative to aggressive estimates from systems assessment)

---

## Software Setup

Install these before starting development. All are free/open-source.

### Core Environment
| Software | Version | Install | Used By |
|----------|---------|---------|---------|
| Python | 3.10+ | https://www.python.org/downloads/ | All projects |
| Git | Latest | https://git-scm.com/downloads | All projects |
| VS Code | Latest | https://code.visualstudio.com/ | All projects |

### Claude Code

All development on this project is done with Claude Code — an AI coding agent that runs in your terminal. It reads your codebase, writes code, runs commands, and iterates with you.

**Prerequisites:**
| Software | Install | Notes |
|----------|---------|-------|
| Node.js 18+ | https://nodejs.org/ | Required for Claude Code CLI |
| Anthropic API Key | https://console.anthropic.com/ | Set via `ANTHROPIC_API_KEY` env var or enter on first run |

**Install:**
```bash
# Install Claude Code
npm install -g @anthropic-ai/claude-code

# Navigate to your project folder and start
cd project-refinery-ai
claude

# Or run it inside VS Code — install the Claude Code extension from the marketplace
```

**How to use it:** Start Claude Code in your project directory. Describe what you want to build — e.g. "Build P4: a soft sensor model using the debutanizer dataset in input/debutanizer/". Claude will read the data, write the code, run it, fix errors, and iterate until the model works. Use it for all 12 projects.

### Python Packages
```bash
pip install numpy pandas scikit-learn matplotlib plotly streamlit flask sqlalchemy
pip install torch torchvision               # P3, P4, P10 (deep learning models)
pip install yfinance requests beautifulsoup4 # P8, P9 (market data)
pip install anthropic                        # P8, P12 (Claude API)
pip install scipy statsmodels prophet        # P11 (demand forecasting)
pip install pdfplumber tabula-py             # P11 (PPAC PDF extraction)
```

### .NET / DWSIM (P1 only)
| Software | Install | Notes |
|----------|---------|-------|
| .NET SDK 6.0+ | https://dotnet.microsoft.com/download | Required to build DWSIM from source |
| DWSIM Desktop | https://dwsim.org/index.php/download/ | Install for visual flowsheet testing |
| DWSIM Source | `git clone https://github.com/DanWBR/dwsim.git input/dwsim_source/` | ~2GB with dependencies |

### API Keys
| Key | Where to Get | Used By |
|-----|-------------|---------|
| EIA API Key | https://www.eia.gov/opendata/register.php | P2, P9 (crude/product prices) |
| Anthropic API Key | https://console.anthropic.com/ | P8, P12 (Claude scoring + narratives) |
| NewsAPI Key | https://newsapi.org/register | P8 (news scraping) |

---

## Data Downloads

All datasets go into `input/`. Download in priority order — earlier projects unblock later ones.

### 1. Debutanizer Column → `input/debutanizer/`
**For:** P4 (Soft Sensor)
**Size:** ~1MB — single CSV, ~2300 samples
**Download:** https://www.kaggle.com/datasets/algorithmiaio/debutanizer-column-dataset
**Alt:** Search "Fortuna debutanizer dataset" (Fortuna et al. 2007)
**Contents:** 7 process variables + butane content target from a real refinery distillation column.

### 2. Tennessee Eastman Process → `input/tep/`
**For:** P3 (Predictive Maintenance), P7 (Alarm Rationalization), P10 (APC Self-Tuning)
**Size:** ~150MB — training + 21 fault test sets
**Download:** https://www.kaggle.com/datasets/averkij/tennessee-eastman-process-simulation-dataset
**Alt:** https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/6C3JR1
**Contents:** 52 sensor variables, 21 fault modes from a simulated chemical plant.

### 3. Crude Assay Data → `input/crude_assays/`
**For:** P2 (Crude Selection Optimizer), P5 (Blend Optimizer)
**Size:** Manual collection — PDFs and spreadsheets
**Sources:**
- CrudeMonitor.ca — free registration, Canadian crude assays
- ENI Crude Assay Encyclopedia — search "ENI world oil review crude assay"
- Chevron crude assay library — https://crudemarketing.chevron.com/
- ADNOC — publishes Murban, Das, Upper Zakum assays publicly
- Saudi Aramco — Arab Light/Medium/Heavy in technical papers
**Priority crudes for Jamnagar:** Arab Heavy, Arab Light, Basrah Light, Basrah Heavy, Murban, Upper Zakum, Kuwait Export, Oman, Das, Nigerian Bonny Light, Colombian Castilla
**Normalize into CSV:** columns = `crude_name, api_gravity, sulfur_pct, yield_lpg, yield_naphtha, yield_kero, yield_diesel, yield_vgo, yield_residue`

### 4. DWSIM Source Code → `input/dwsim_source/`
**For:** P1 (DWSIM CLI Engine) → unlocks P2, P5, P6, P10
**Size:** ~2GB with dependencies
**Download:** `git clone https://github.com/DanWBR/dwsim.git input/dwsim_source/`
**Contents:** .NET/VB.NET open-source process simulator. We extract the thermodynamic engine and wrap it as a CLI/Python API.

### 5. PPAC Demand Data → `input/ppac_demand/`
**For:** P11 (Demand Forecasting)
**Size:** Manual PDF downloads
**Download:** https://www.ppac.gov.in/consumption
**Contents:** Monthly "Consumption of Petroleum Products" tables + "Refinery-wise Crude Throughput" tables. Aim for 5+ years of history.

### 6. LBNL Industrial Energy Data → `input/lbnl_energy/`
**For:** P6 (Energy/Steam Optimizer)
**Size:** Varies
**Download:** https://iindustrial.lbl.gov/
**Alt:** Search "LBNL industrial assessment center database"
**Contents:** Refinery energy audit data — steam generation, boiler loads, furnace efficiency, heat exchange.

---

## Build Order

**Phase 1 — Quick Wins (Week 1-2)**
- P4: Soft Sensor (debutanizer dataset, straightforward ML)
- P3: Predictive Maintenance (TEP dataset, classification/anomaly detection)
- P8: Market Intelligence (already built — polish and connect)

**Phase 2 — The Engine (Week 2-4)**
- P1: DWSIM CLI Engine (the hardest engineering — .NET surgery)
- P7: Alarm Rationalization (derived from TEP data)
- P9: Real-Time Risk Dashboard (futures data + VaR math)

**Phase 3 — Optimization (Week 4-6)**
- P2: Crude Selection Optimizer (needs P1)
- P5: Blend Optimizer (needs P1)
- P10: APC Self-Tuning (needs P1 + P3 foundation)

**Phase 4 — Integration (Week 6-8)**
- P6: Energy/Steam Optimizer (needs P1)
- P11: Demand Forecasting (PPAC data)
- P12: Digital Twin Dashboard (integration layer)

---

## Related Projects
- `ai-transformation/` — Systems assessment, 3-layer framework, opportunity mapping
- `oil-dashboard/` — Live geopolitical scenario engine (becomes P8)
