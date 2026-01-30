# Stella JRPG POC (Outputs Showcase)
This repo captures the artifacts produced by Stella when generating a small proof-of-concept
for a 4-phase JRPG/Yu-Gi-Oh inspired battle loop. It exists to show how Stella structures
outputs across local and remote runs (Ollama and Gemini).

## Purpose
- Preserve the exact inputs and outputs for this POC run.
- Showcase the structured responses produced by Stella.

## Inputs (authoritative)
These files define the intent and constraints Stella uses:
- `README.md` (this file) - public intent snapshot
- `CONSTRAINTS.md` - invariants and non-negotiables
- `TODO.md` - active intent
- `CONTRACTS/` - schema + intent contracts
- `decisions/` - authority records (may be empty)

## Outputs (generated)
Key artifacts produced by Stella in this run:
- `PACK.json` - packaged, normalized intent bundle
- `RUN.json` - execution record
- `OUTPUT.json` - structured response output
- `REPORT-BUNDLE.json` - artifact report bundle
- `GEMINI_RESPONSE.md` - remote provider output (Gemini)
- `OLLAMA_RESPONSE.md` - local provider output (Ollama)
- `MEMORY/` - memory entries and index (when written)

## How these were generated (example)
From the Stella repo root, run against this repo:
```powershell
python -m stella_engine.cli validate --repo <repo-root>
python -m stella_engine.cli pack --repo <repo-root> --output <repo-root>\PACK.json
python -m stella_engine.cli run --provider ollama --pack <repo-root>\PACK.json --output <repo-root>\RUN.json
python -m stella_engine.cli run --provider gemini --allow-remote --pack <repo-root>\PACK.json --output <repo-root>\RUN.json
python -m stella_engine.cli eval --run <repo-root>\RUN.json
python -m stella_engine.cli report --output <repo-root>\REPORT-BUNDLE.json
```

Notes:
- Gemini runs require `--allow-remote` and a valid `GEMINI_API_KEY`.
- Ollama runs require a local Ollama server and `OLLAMA_HOST` / `OLLAMA_MODEL`.

## Core loop constraints (from CONSTRAINTS.md)
- Loop phases are Declare -> Commit -> Resolve -> Recover (exactly 4).
- Commitment types: Instant / Charged / Channeled.
- Disruption degrades output; committed skills resolve unless a unit is defeated.
- Keep scope to the battle loop only.
