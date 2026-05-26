# Practice 1 — Record definition and dataset schema

> Replace template text with your project decisions. Keep this report aligned with `project.json` and `specs/dataset_schema.json`.

## Topic

Aptamer–protein binding dataset (example template).

## Scientific task

Collect experimentally reported aptamer–protein binding measurements for comparing affinity values and assay conditions across sources.

## One-record definition

**One record** = one experimentally reported heating measurement (SAR or SLP value) for one specific iron oxide nanoparticle sample under one defined set of AMF conditions 

## Examples of records

| Example | Why it counts |
|---------|----------------|
| SAR = 705.86 W/g for 13.5 nm Fe₃O₄ nanoparticles (PEG-coated) at f = 267 kHz, H = 27 kA/m, concentration 10 mg(Fe)/mL in water from Yu et al. 2025 | Single measurement with complete metadata: size, coating, field conditions, concentration |
| ILP = 1.49 nHm²/kg calculated from SAR/(f·H²) for the same measurement | Derived metric enabling cross-study comparison |
| Multiple SAR values for the same sample at different frequencies (e.g., 100 kHz, 200 kHz, 300 kHz) | Each unique (f, H) combination is a separate record |

## Non-record examples

| Example | Why it is not a record |
|---------|-------------------------|
| General review paragraph on magnetic hyperthermia without numeric SAR/ILP values | No measurement |
| Table of nanoparticle sizes and Ms without corresponding heating data | No heating performance reported |
| In vivo tumor volume reduction data | Downstream endpoint, not direct heating performance |
| Simulated/in silico heating prediction without experimental validation | Dataset requires experimental data only |
| Measurement at f·H > 5×10⁹ A/(m·s) | Exceeds clinical safety limit |
| Core diameter > 30 nm or < 2 nm | Outside superparamagnetic/stable single-domain range |

## Dataset fields

| field | type | required | description | unit | example |
|-------|------|----------|-------------|------|---------|
| `record_id` | string | yes | Unique identifier per record | — | IONP_HT_001 |
| `doi` | string | yes | DOI of source publication | — | 10.1039/D5RA00728C |
| `core_material` | string | yes | Chemical composition of magnetic core | — | Fe₃O₄ |
| `core_diameter_mean` | float | yes | Mean core diameter | nm | 13.5 |
| `core_diameter_std` | float | no | Standard deviation of core diameter | nm | 1.2 |
| `core_shape` | string | no | Morphology | — | spherical |
| `ms` | float | no | Saturation magnetization | emu/g | 80 |
| `coating_material` | string | no | Surface coating composition | — | PEG2000 |
| `coating_thickness` | float | no | Coating layer thickness | nm | 3.5 |
| `zeta_potential` | float | no | Surface charge | mV | -25 |
| `hydrodynamic_diameter` | float | no | Hydrodynamic diameter | nm | 45 |
| `synthesis_method` | string | no | Preparation route | — | thermal decomposition |
| `frequency` | float | yes | AMF frequency | kHz | 267 |
| `amplitude` | float | yes | AMF amplitude | kA/m | 27 |
| `solvent_medium` | string | yes | Dispersion medium | — | water |
| `concentration` | float | yes | Iron concentration | mg/mL | 10 |
| `sar` | float | conditional | Specific Absorption Rate | W/g | 705.86 |
| `ilp` | float | conditional | Intrinsic Loss Power = SAR/(f·H²) | nHm²/kg | 1.49 |
| `extraction_notes` | string | no | Notes on ambiguity | — | SAR calculated from heating curve |

## Ambiguous cases

| Ambiguous case | Decision |
|----------------|----------|
| SAR reported for multiple concentrations but concentration not specified | Exclude record |
| Only ΔT reported without SAR | Exclude unless SAR can be calculated |
| Range reported as "SAR = 400–600 W/g" | Store `NULL`, record midpoint in `extraction_notes` |
| Core diameter reported without SD | Store mean only |
| f·H > 5×10⁹ A/(m·s) | Exclude record |
| Solvent not specified | Exclude record |
