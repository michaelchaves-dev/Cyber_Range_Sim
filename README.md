# Cyber Range Simulator (Hardened)

Offline defensive tabletop exercise tool — **0 network requests**, fully local.

Open `cyber-range-simulator-hardened.html` in a browser. No build step, no server, no account.

## What it is

A single-file facilitator aid for authorized defensive training. Teams walk through four fictional incident drills, make response decisions, validate controls, and generate a local after-action report.

## Scenarios

1. **Ransomware readiness drill**
2. **Business email compromise drill**
3. **Cloud account exposure drill**
4. **Third-party vendor incident drill**

Each scenario has five phases with scored decision points, a detection/response map, and control checklists.

## How to use

1. Open `cyber-range-simulator-hardened.html` (double-click or drag into a browser).
2. Enter organization / facilitator details.
3. Pick a scenario and difficulty, then **Start / restart exercise**.
4. For each phase, select a response and confirm.
5. Mark defensive controls you can demonstrate today.
6. **Generate report**, then **Print / save PDF** if you want a portable copy.
7. Optional: **Save scenario locally** / **Load saved scenario** (browser `localStorage` only).

## Security posture

- Content-Security-Policy blocks network fetches, frames, workers, and object embeds.
- No CDNs, fonts, analytics, or third-party scripts.
- Dynamic HTML is escaped; saved state is schema-validated and length-capped.
- Fictional defensive content only — no reconnaissance, exploitation, or live-system actions.

See [SECURITY_AUDIT.md](./SECURITY_AUDIT.md) for the full review.

## Authorized use

For authorized training, detection planning, incident-response practice, and control validation in fictional environments. Do not use this tool against real systems or as an attack guide.
