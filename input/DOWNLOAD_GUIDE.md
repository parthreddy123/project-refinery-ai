# Project Dhurandhar — Input Datasets Download Guide

## 1. Tennessee Eastman Process (TEP) → `tep/`
**What:** 52 sensor variables from a chemical plant, 21 fault modes. For P3 (Predictive Maintenance) and P7 (Alarm Rationalization) and P10 (APC Self-Tuning).
**Where:** https://www.kaggle.com/datasets/averkij/tennessee-eastman-process-simulation-dataset
**Alt:** https://dataverse.harvard.edu/dataset.xhtml?persistentId=doi:10.7910/DVN/6C3JR1
**Files:** Download all CSVs — training set (fault-free) + test sets (21 faults). ~150MB total.

## 2. Debutanizer Column → `debutanizer/`
**What:** 7 process variables + butane content target from a real refinery distillation column. For P4 (Soft Sensor).
**Where:** https://www.kaggle.com/datasets/algorithmiaio/debutanizer-column-dataset
**Alt:** Search "Fortuna debutanizer dataset" — originally from Fortuna et al. (2007)
**Files:** Single CSV, ~2300 samples. Tiny download.

## 3. Crude Assay Data → `crude_assays/`
**What:** Crude oil quality profiles (API gravity, sulfur, TBP distillation curves, yields by cut). For P2 (Crude Selection Optimizer) and P5 (Blend Optimizer).
**Where:**
- CrudeMonitor.ca — free registration, download Canadian crude assays
- ENI Crude Assay Encyclopedia — search "ENI world oil review crude assay"
- Chevron crude assay library — https://crudemarketing.chevron.com/
- ADNOC publishes Murban, Das, Upper Zakum assays publicly
- Saudi Aramco publishes Arab Light/Medium/Heavy assays in technical papers
**Priority crudes for Jamnagar:** Arab Heavy, Arab Light, Basrah Light, Basrah Heavy, Murban, Upper Zakum, Kuwait Export, Oman, Das, Nigerian Bonny Light, Colombian Castilla
**Files:** PDFs or spreadsheets — normalize into a single CSV with columns: crude_name, api_gravity, sulfur_pct, yield_lpg, yield_naphtha, yield_kero, yield_diesel, yield_vgo, yield_residue

## 4. LBNL Industrial Energy Data → `lbnl_energy/`
**What:** Refinery energy audit data — steam generation, boiler loads, furnace efficiency, heat exchange. For P6 (Energy/Steam Optimizer).
**Where:** https://iindustrial.lbl.gov/
**Alt:** Search "LBNL industrial assessment center database"
**Files:** Download refinery-specific audit reports and data tables.

## 5. PPAC Historical Demand Data → `ppac_demand/`
**What:** Indian monthly fuel consumption by product (diesel, petrol, LPG, ATF, naphtha, fuel oil). For P11 (Demand Forecasting).
**Where:** https://www.ppac.gov.in/consumption
**Files:** Monthly "Consumption of Petroleum Products" tables. Download as many months as available — aim for 5+ years of history. Also grab the "Refinery-wise Crude Throughput" tables.

## 6. DWSIM Source Code → `dwsim_source/`
**What:** Open-source process simulator. We will extract the thermodynamic engine for P1 (CLI Engine).
**Where:** https://github.com/DanWBR/dwsim
**How:** `git clone https://github.com/DanWBR/dwsim.git dwsim_source/`
**Note:** .NET/VB.NET codebase, ~2GB with dependencies. Also install DWSIM desktop app for visual flowsheet building: https://dwsim.org/index.php/download/

## Priority Order
1. **Debutanizer** — smallest download, quickest win (P4)
2. **TEP** — medium download, high value (P3, P7, P10)
3. **Crude assays** — manual collection, needed for P2
4. **DWSIM source** — git clone, foundation for P1
5. **PPAC demand** — manual PDF downloads
6. **LBNL energy** — lowest priority, P6 can wait
