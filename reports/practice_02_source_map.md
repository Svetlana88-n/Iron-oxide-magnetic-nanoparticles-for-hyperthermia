# Practice 2 — Source map

> Document how you found sources and maintain `specs/source_map.json` as the machine-readable authority.

## Source search strategy

**Search platforms used:**
- PubMed 

**Search keywords:**
- `"magnetic hyperthermia" AND "iron oxide nanoparticles" AND SAR`

**Snowballing method:**
Citation tracking from Vilas-Boas et al. 2020 (review containing Tables A1–A3 with primary studies)

## Source groups

| Source group | Description | Examples from provided files |
|--------------|-------------|------------------------------|
| **scientific_papers** | Peer-reviewed research articles reporting experimental SAR/ILP measurements | Vilas-Boas et al. 2020 (review with compiled data); GA-статья 2022 (experimental) |
| **clinical_studies** | Human clinical trials with magnetic hyperthermia | Thiesen & Jordan 2008 (Nanotherm, glioblastoma, prostate cancer) |
| **simulation_studies** | Numerical simulations, not experimental | Rytov & Usov 2023 — **not used for experimental dataset** |
| **review_articles** | Summaries of multiple studies | Fatima et al. 2021; Soetaert et al. 2020 |
| **databases** | Compound identifiers and properties | PubChem, ChEMBL |

## Priority sources

| Priority | Source | Rationale |
|----------|--------|-----------|
| 1 | Vilas-Boas et al. 2020 (Molecules) | Contains Tables A1–A3 with f, H, SAR, core diameter, coating from multiple primary studies |
| 2 | GA-статья (Materials 2022) | Primary experimental data: f=530 kHz, H=12.5–25 kA/m, SAR~200 W/g, core=11 nm |
| 3 | Thiesen & Jordan 2008 | Clinical data: f=100 kHz, H=3.8–13.5 kA/m, core=15 nm, coating=aminosilane |
| 4 | Soetaert et al. 2020 | Review with physics background and safety limits |
| 5 | Fatima et al. 2021 | Review with SAR formula and general parameters |
| 6 | PubChem / ChEMBL | Metadata enrichment only (SMILES, InChIKey) |

**Note:** Rytov & Usov 2023 is **simulation**, not experimental. Excluded from dataset.

## Access conditions

| Source | Access status | Access method | API key required? | Notes |
|--------|--------------|---------------|-------------------|-------|
| Vilas-Boas et al. 2020 (Molecules) | Open (MDPI) | PDF download | No | DOI: 10.3390/molecules25122874 |
| GA-статья (Materials 2022) | Open (MDPI) | PDF download | No | DOI: 10.3390/ma15030788 |
| Thiesen & Jordan 2008 | Subscription | Via journal | No (institutional) | DOI: 10.1080/02656730802104757 |
| Soetaert et al. 2020 | Open (Elsevier) | PDF download | No | DOI: 10.1016/j.addr.2020.06.025 |
| Fatima et al. 2021 | Open (MDPI) | PDF download | No | DOI: 10.3390/nano11051203 |
| Rytov & Usov 2023 | Open (Beilstein) | PDF download | No | DOI: 10.3762/bjnano.14.39 — **simulation only** |
| PubChem | Open | API | No | -|
| ChEMBL | Open | API | No | - |


## Expected data types

| Data type | Example sources | Extraction method |
|-----------|-----------------|-------------------|
| Tables (SAR, f, H) | Vilas-Boas 2020, Tables A1–A3 | Manual extraction from PDF |
| Tables (SAR, f, H) | GA-статья 2022, Table 1 | Manual extraction from PDF |
| Text (clinical parameters) | Thiesen & Jordan 2008 | Manual extraction |
| API JSON | PubChem, ChEMBL | `requests` + JSON parsing |

## Expected conflicts and overlaps

| Conflict type | Resolution rule |
|---------------|-----------------|
| Different SAR values for similar nanoparticles | Keep separate records; do not average |
| SAR in W/g vs. W/kg | Normalize to W/g (divide by 1000) |
| Missing f or H | Exclude record (cannot calculate ILP) |
| Simulation data (Rytov & Usov 2023) | Exclude — not experimental |

**Overlap detection:** Use DOI + core_diameter_mean + frequency + amplitude as composite key.

## Coverage gaps

Based on provided files:

| Gap | Status | Plan |
|-----|--------|------|
| In vivo heating data | Excluded by design | Dataset scope: experimental measurements only |
| Non-iron oxide nanoparticles | Excluded by design | Topic scope: iron oxide only |

**Note:** The only experimental SAR values with complete f, H, and core diameter in provided files come from:
1. Vilas-Boas 2020 (compiled from literature, but not primary)
2. GA-статья 2022 (primary: 200 W/g, 530 kHz, 25 kA/m, 11 nm)
3. Thiesen & Jordan 2008 (clinical: 0.718 W/g, 100 kHz, 3.8–13.5 kA/m, 15 nm)
