# JRPG POC Run Log

Date: 2026-01-29
Repo: <repo-root>

## Runs Completed
- validate -> pack -> run(mock) -> eval
- run(ollama) -> eval (allow-non-mock)
- run(gemini, allow-remote, route remote) -> eval (allow-non-mock)

## Artifacts
- PACK.json
- RUN.json
- OUTPUT.json
- REPORT-BUNDLE.json

Notes:
- OUTPUT.json is a minimal spec placeholder; provider outputs live in RUN.json under provider_result.raw_response.
