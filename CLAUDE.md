# CLAUDE.md

## Stack

Cloudflare Worker (TypeScript) + React 18 + Vite + Tailwind + shadcn/ui + Cloudflare D1.

Package manager: **pnpm**. Never use npm commands.

## Key Commands

```bash
pnpm run dev             # Worker backend :8787
pnpm -C web run dev      # React frontend :5173 (proxy /api → :8787)
pnpm run dev:remote      # backend with live D1
pnpm run deploy          # build:web + wrangler deploy
pnpm run test
```

## Architecture Boundaries — Never Violate

1. `isPromotionalEmail()` must run **before** any LLM call in `email()` handler.
2. Every incoming email → `raw_mails`. Only AI-extracted results → `code_mails`. Never skip `raw_mails`.
3. `src/index.html` is a live fallback. Do not delete.
4. DOMPurify + sandboxed iframe on all email HTML rendering. Do not relax.
5. Basic Auth gate is first in `WorkerEntrypoint.fetch()`. Do not move or bypass.

## Frontend Component Rules

- UI components: shadcn/ui only. No new UI libraries without discussion.
- Colors: CSS variables in `web/src/index.css`. No hardcoded hex except table hover states.
- Use `text-muted-foreground` for secondary text. `text-muted` is a background color — never use as text.
- All grid children need `min-w-0` to prevent overflow.

## Compact Instructions

When compressing, preserve in priority order:

1. Architecture decisions (NEVER summarize)
2. Modified files and their key changes
3. Current verification status (pass/fail)
4. Open TODOs and rollback notes
5. Tool outputs (can delete, keep pass/fail only)
