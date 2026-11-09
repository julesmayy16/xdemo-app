# Ledgerly — customer education workshop

Repo: [`xai-org/customer-education-workshops`](https://github.com/xai-org/customer-education-workshops). The app is the top-level folder `ledgerly/`, not the repo root.

Welcome! **Ledgerly** is a fictional B2B billing and collections app. You will work inside a running product: a dashboard, invoices, collections, disputes, and settings. The operator is **Avery Quinn**, on the **Fieldnote Workspace** book. Catalog prices are Starter **$49**, Growth **$99**, and Scale **$249**. The demo clock is frozen at **23 August 2026**, so overdue math never drifts mid-demo.

Everything here is synthetic. No real companies, no real people, no real data.

The tracks are Grok Build **101**, **201**, **301**, and **401**. After the app is up, those steps and speaker notes are in `ledgerly/README.md` and `ledgerly/demo-howto.md`.

## Requirements

You need **Node.js 20** only. `node -v` must print `v20.x.x`. npm comes with it.

No Python, Docker, Postgres, or required `.env`. Data is a SQLite file, `file:./dev.db` (created at `ledgerly/prisma/dev.db`). The nodejs.org button labeled LTS is not Node 20. `OpenJS.NodeJS.LTS` is not Node 20.

## Install Node 20

**macOS**

1. Install the 20.x installer from the [Node previous-releases page](https://nodejs.org/en/about/previous-releases), or `nvm install 20 && nvm use 20`.
2. Verify: `node -v` prints `v20.x.x`.

**Windows (native)**

1. Same 20.x installer, or `winget install --id OpenJS.NodeJS.20 --exact`. Do not use `OpenJS.NodeJS.LTS`. That is the current LTS line, not Node 20.
2. Close and reopen PowerShell, then `node -v`.

**WSL (Debian/Ubuntu)**

Inside WSL, `nvm install 20 && nvm use 20`, or:

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Clone under `~/customer-education-workshops`, not `/mnt/c`.

## Setup

```bash
git clone https://github.com/xai-org/customer-education-workshops.git
cd customer-education-workshops/ledgerly
npm i
npx prisma generate
npx prisma db seed
npm run dev
```

`npx prisma generate` belongs in that sequence. Seed runs `prisma db push --skip-generate`, so it does not create the Prisma client.

Open **http://localhost:43173**. Leave the terminal open. Stop with Ctrl+C.

`npm test` is **1 failed / 33 passed** on a clean tree. The red file is `tests/suggested-credit-api.test.ts`. That failure is planted. Leave it unless the facilitator points you at it.

Empty dashboard: `npm run db:reset`. To put the planted demo state back, `npm run demo:reset`, then finish only the MCP cleanup that script reports. `reset-demo-state` does not replace `npm run demo:reset`.

## What's in the box

- **The app** — Next.js + TypeScript + Prisma/SQLite. Chrome: Dashboard, Invoices, Collections, Disputes, Settings. The header has a light/dark control.
- **Tracks** — 101, 201, 301, and 401. Copy-paste prompts live in `runbooks.html` and the Runbooks canvas (`runbooks.canvas.tsx`). `present-runbooks` opens that canvas. `start-ledgerly` starts the app and opens the same canvas. Presenter notes are `demo-howto.md`.
- **Skills** in `ledgerly/.cursor/skills/` include `start-ledgerly`, `present-runbooks`, `add-dashboard-widget`, `draft-collection-email`, `write-prisma-query`, `reset-demo-state`, and `stage-linear`.

## Prompts — follow along

Paste these as-is, from the running app.

**1. Ask — prices, overdue invoices, and the dispute flow.**

```text
What are Ledgerly's only plan prices, and which seeded invoices are overdue? Cite lib/plans.ts, prisma/seed.ts, and prisma/extra-accounts.ts.

Explain the dispute flow end to end. What is intentionally unfinished? Cite the resolve helper, the resolve API route, and the dispute page. Do not edit any files.
```

**2. Cmd-K (Ctrl-K on Windows) — one calm sentence.** Select the settings-page description input's default value, then press Cmd-K:

```text
Rewrite this input's default value as one calm sentence explaining that the demo clock is frozen on 23 August 2026 so overdue math never drifts during a meeting. Keep it under 15 words.
```

**3. Agent — suggested-credit API migration.**

```text
Diagnose why dsp_1043 shows a $400 suggested credit even though v2 caps it at $249. Switch lib/disputes/suggested-credit-api.ts from v1 to v2. Preserve both API routes, the $400 dispute claim, and tests/suggested-credit-api.test.ts. Run the relevant tests and verify the page shows $249 from v2.
```

**4. Rule.**

```text
/create-rule Future code must never call /api/v1/disputes/*/suggested-credit. It must use /api/v2/disputes/*/suggested-credit. Create the project rule at .cursor/rules/suggested-credit-api-v2.mdc and show me the file before I keep it.
```

**5. Design Mode — restyle the KPI cards.** With Design Mode active on the dashboard:

```text
Restyle the four KPI cards on this dashboard using only the existing design tokens in app/globals.css: a soft indigo accent on each card, stronger emphasis on the value, and a subtle hover lift. No new hex colors, no layout rewrite, no data or price changes — $49, $99, and $249 stay exactly as rendered. Touch components/kpi-card.tsx, and app/page.tsx only if you must. Two files max, nothing under lib/ or tests/. Show me the diff — I am undoing this after the demo.
```

No Design Mode on your build? Paste the same prompt to the Agent. It names the files and constraints either way.

## Ground rules

- Synthetic data only. Nothing here is real, and no external or client data comes in.
- The catalog prices ($49 / $99 / $249) and the frozen demo clock stay fixed.
- Do not edit `tests/suggested-credit-api.test.ts`, either suggested-credit route, or the seed to make the suite green.
- "Not found" beats making it up.

## Troubleshooting

- **Port 43173 busy** — a previous dev server is still running. Stop it, or `npm run dev` fails.
- **Dashboard empty** — from `ledgerly/`, run `npm run db:reset`.
- **Planted demo state changed** — from `ledgerly/`, run `npm run demo:reset`, then only the MCP cleanup that script reports.
- **Seed says the Prisma client is missing** — `npx prisma generate`, then `npx prisma db seed` again.
- **Windows / WSL** — native Windows is supported. If you use WSL, clone under the Linux home (`~/customer-education-workshops`), not `/mnt/c`. Line endings in `ledgerly/` are LF.
- **`npm test` is red** — one failure is shipped on purpose. Exactly **1 failed / 33 passed** is healthy. The red file is `tests/suggested-credit-api.test.ts`.
