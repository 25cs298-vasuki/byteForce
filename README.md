# DisasterMesh: Crisis Dispatch Command Center
Zephoria 2K26 Vibecoding, PS-05. Software-only multi-channel disaster dispatch.

## Features
- Citizen SOS (text / voice transcript / image caption) with live NLP preview
- Entity extraction (area gazetteer), urgency classification (weighted keywords), people count
- Spatial clustering: reports of the same category within a set radius merge into one incident
- Triage queue ranked by severity, corroboration, people at risk and wait time
- Nearest suitable unit dispatch with ETA; live unit status (available / en route / on scene)

## Logic (for Q&A)
- `parse()`: normalises text, matches area names, scores severity 1-4 by keyword weight, picks category, extracts people count
- `submit()`: finds an open incident of the same category within 30 map units and merges, otherwise creates one
- `dispatch()`: filters free units by category preference, sorts by preference then distance
- `pri()`: priority = severity*10 + min(reports,6)*3 + people*1.2 + minutes waiting*0.4

## Run
Open `index.html` in a browser, or serve the folder: `python -m http.server 8000`.

## Persistence
`index.html` calls a `db` object when the host provides one and otherwise runs in local in-memory mode.
To persist in your own deployment, replace the `save()`, `del()` and snapshot-subscription code
with calls to your backend (Firebase / Supabase / Express + Postgres). Keep keys server-side or
protected by security rules, never in the client bundle. 

