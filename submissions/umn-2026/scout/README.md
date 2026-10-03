# Scout

## Team

**Team name:** Scout

| Name | GitHub |
|---|---|
| Kent Nguyen | Not provided |
| Harish Lingam | Not provided |
| Marwan Almania | Not provided |
| Hyunjoong Kim | Not provided |
| David Feng | @DavidFeng0718 |
| Abdirahman Ahmed | Not provided |

## Summary

Scout helps undergraduates turn an initial research interest into concrete research directions, a manageable weekly preparation plan, evidence-backed faculty matches, and an editable outreach email. Its local web interface and command-line agent use OpenClaw to create and revise saved work through validated tools; students can confirm background information extracted from a PDF resume, compare directions, adjust their time budget, and export their work. The prototype includes eight sourced University of Minnesota Twin Cities faculty profiles, makes catalog gaps explicit, and leaves final review and email sending to the student.

An English research exploration agent for undergraduates. It connects an initial interest to a few research directions, an achievable weekly plan, relevant faculty, and a truthful outreach draft.

## Run the research agent without a website

From this submission folder, use Node.js 24.16+ within version 24, or 26.1+, and npm.

The new OpenClaw agent can update an evidence-backed student profile, author and revise saved plans, start with a named professor, and write or shorten actual email drafts. It no longer uses the guided-demo templates for these outputs. Use your ChatGPT subscription through your explicitly authorized local OpenClaw account:

```sh
npm install
npm run openclaw:setup
npm run openclaw:login
npm run openclaw:start
# In a second terminal:
npm run agent
```

CLI turns are saved privately under `.research-agent/`; the local website saves its separate workspace in your browser. See [agent behavior and verification](docs/agent.md). The published cloud preview and direct API-key adapter remain separate older paths. The local website is now connected to this agent.

## Private cloud preview

[Open Research Matchmaker](https://research-matchmaker-fyl.ipigeonkingi.chatgpt.site) — deployed privately with ChatGPT Sites on October 3, 2026. Sign in with the owning ChatGPT account if prompted.

The cloud preview runs the complete guided demo, with UMN faculty examples and browser-local progress. Live AI and OpenClaw are not connected. The Sites-compatible source is in `sites/research-matchmaker/`; its hosting manifest records the existing Site identity for updates.

## How to run

From this submission folder (`submissions/umn-2026/scout/`):

Use Node.js 24.16+ within version 24, or 26.1+, and npm for the pinned OpenClaw runtime.

```sh
npm install
npm run dev
```

Open **http://127.0.0.1:5173**. The API runs on port 3001. The local Scout website defaults to **Research agent**, using the same OpenClaw tool loop as `npm run agent`. Keep your authenticated local gateway running (setup below); no API key is needed. Previous Live AI selections migrate to Research agent, and an explicit demo choice is retained. Missing configuration and model errors stay visible without falling back to templates. Guided demo is available for an empty workspace; export and clear agent-authored work before switching to demo.

For a production build on your own computer:

```sh
npm run build
npm start
```

Then open **http://127.0.0.1:3001**. Keep the server on its default localhost address. This version is a personal/local prototype, without public user authentication or a hosted database.

## Use the OpenClaw version

The local Scout website uses the **Research agent** through OpenClaw by default, with Guided demo for offline exploration. It keeps the English research workflow: interests → directions → preparation → faculty → editable outreach. The university is always supplied by the student.

Use Node.js **24.16+ within version 24**, or **26.1+**, for the pinned OpenClaw 2026.9.8 runtime.

```sh
npm run openclaw:setup
npm run openclaw:login
npm run openclaw:start
```

Keep the gateway terminal open. In another terminal run `npm run dev`, open **http://127.0.0.1:5173**, then select **Settings → Research agent → Check OpenClaw connection**. A successful connection check verifies the gateway, not model access; send a message after signing in to verify the model.

Setup installs OpenClaw inside this project and creates a private local gateway. Login opens OpenClaw's official ChatGPT/Codex OAuth flow. The app does not read your Codex credentials or ask for a password. Subscription usage depends on the account you authorize and its available models. An OpenAI Platform API key is not required for this mode.

OpenClaw faculty matching currently uses the eight verified UMN examples. Other schools still receive interest exploration and preparation guidance, with an explicit catalog-gap message at matching. See the [OpenClaw setup guide](docs/openclaw.md) for account options, troubleshooting, and testing.

## Legacy API adapter

`server/live.ts` remains available for existing API clients and regression tests, but Scout's website no longer offers this older API-key path. It still uses template-based state updates and should not be used for agent-authored work. The current website and CLI both use `server/openclaw.ts` and `server/research-agent.ts`.

## What Scout does

- Describe interests in English and edit the student profile.
- Explore a few research directions and compare questions, activities, and starter resources.
- Select a direction and create a weekly plan that respects available time.
- Complete tasks, ask for a simpler plan, and return to an earlier stage.
- Tell the agent your university when you are ready to find faculty. There is no default school.
- Find faculty in the curated University of Minnesota Twin Cities sample catalog, with an explicit coverage gap for other schools.
- Read each professor's evidence, source links, and source-check or retrieval date. Research relevance does not imply an available position.
- Draft, edit, copy, or download an outreach email. The app never sends email.
- Read and confirm a PDF resume or enter background information, save progress in the browser, and export or clear your workspace.

Scout authors and revises concrete research-direction cards, including topics outside the starter catalog. The catalog supplies eight optional resource categories and eight University of Minnesota Twin Cities faculty profiles; it does not limit the proposed questions. Faculty/resource retrieval still uses this catalog and does not search the web or verify current recruitment. See [source notes](docs/sources.md) for catalog coverage and verification details.

## Data and privacy

The browser retains your confirmed profile, conversation, plan, and draft so you can continue later. Clear the workspace to remove the saved session. Imported text becomes part of your profile only when you confirm it. PDF resumes are parsed locally in the browser (up to 5 MB, 15 pages, and 16,000 extracted characters). Review the editable preview, confirm it, and save your profile. No PDF is uploaded. Confirmed text is included in subsequent agent requests. Scanned PDFs require OCR first.

Demo mode stays on your local app server. With the research agent, your conversation and profile pass through the local gateway to the configured model provider; OpenClaw may retain transcripts and authentication data in `.openclaw-local/`. Clearing browser data does not erase those gateway records. The app keeps API keys and gateway tokens out of browser code and never sends messages to professors. Avoid adding personal information that is unnecessary for research exploration.

The catalog records source verification dates; it does not continuously monitor recruitment. Confirm current availability and application requirements directly with a lab.

## Check the app

```sh
npm test
npm run build
npm run test:e2e
```

Install the isolated Playwright Chromium browser before running browser tests:

```sh
npx playwright install chromium
npm run test:e2e
```

The tests cover the research tools, streamed HTTP responses, browser plan/draft round trips, PDF confirmation, errors, persistence, and the optional guided demo. Automated fixtures do not establish subscription access; send a real message through your authenticated gateway to verify that account.

## Project layout

- `src/`: React interface and local workspace state.
- `server/`: Express API, research agent, OpenClaw adapter, PDF file reader, legacy adapters, and curated catalog.
- `scripts/openclaw*.mjs`: isolated OpenClaw setup and CLI launcher.
- `openclaw/runtime/`: pinned gateway dependency and its lockfile.
- `shared/types.ts`: the shared request, response, and workspace contract.
- `tests/`: automated checks.
- `docs/sources.md`: evidence for catalog entries and update notes.

The original Chinese and English design documents remain in the project root.

The live integration follows the official [function calling guide](https://developers.openai.com/api/docs/guides/function-calling) and [structured outputs guide](https://developers.openai.com/api/docs/guides/structured-outputs).

## License

This submission uses the repository default MIT license. Bundled third-party components retain their included license notices.
