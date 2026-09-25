# ExitAnt Turtle — LBank

Deploy this automated trading bot to **your own Render account** as a background
worker. The bot runs from a prebuilt Docker image; this repository contains **only
the Render Blueprint (`render.yaml`)** — no source code.

## Deploy to Render

[![Deploy to Render](https://render.com/images/deploy-to-render-button.svg)](https://render.com/deploy?repo=https://github.com/ExitAnt-dev/turtle-lbank)

Or open this link in your browser:

```
https://render.com/deploy?repo=https://github.com/ExitAnt-dev/turtle-lbank
```

1. Click the button (or open the link) and sign in to Render.
2. Render reads `render.yaml` and proposes **one background worker** with a
   **1 GB persistent disk** mounted at `/data` (it keeps your settings and order
   state across restarts and redeploys).
3. Enter the environment variables listed below. They are marked `sync: false`,
   so Render asks you for them on the deploy screen and stores them **only in your
   own Render account** — they are never written to this repository.
4. Click **Apply** and wait for the first deploy to finish.
5. Open Telegram and send **`/start`** to your bot to begin setup.

> Tip: you can also deploy from the Render dashboard — **New → Blueprint**, then
> select this repository. Because `render.yaml` is on the default branch, both
> methods work.

## Before you deploy

- **LBank account registered with the ExitAnt invite code `64JKW`.** At every start
  the bot checks this with your own API key; without it, it will not trade.
- **Futures API key** with trading permission. LBank has no API passphrase.
- **Hedge (two-way) position mode** enabled in LBank Futures. The bot refuses to
  start in one-way mode.
- If the API key has **IP restriction** enabled, whitelist your Render service's outbound IP addresses
  (Render → the service → Settings → Outbound IP addresses) or disable the restriction. Otherwise LBank rejects
  the bot's calls with error `10022`. On every start the bot checks the key and, if rejected, tells you the server's
  current IP to whitelist.
- USDT in your LBank **futures** account. The bot moves profit reserves between your
  own futures and spot accounts; it never withdraws.

## Environment variables

You fill these in during deployment (nothing is pre-filled or stored here):

| Variable | What to enter |
|---|---|
| `API_KEY` | LBank API key |
| `SECRET_KEY` | LBank API secret |
| `TELEGRAM_BOT_TOKEN` | Bot token issued by [@BotFather](https://t.me/BotFather) |
| `TELEGRAM_CHAT_ID` | Your own Telegram chat ID — the bot ignores every other sender |

Nothing else to configure: the trading mode and state-file location are built into
the image.

## How it works

- **Prebuilt image.** The worker pulls `docker.io/exitant/exitant-lbank-turtle:latest` from Docker Hub. The image ships
  a compiled binary only — no Python source is included or exposed.
- **Background worker.** There is no web page; the bot runs continuously and talks
  to you through Telegram.
- **Your keys stay yours.** Secrets are typed into your own Render environment and
  are never sent to this repository, to Render's Blueprint, or to anyone else.
- **Persistent state.** Settings, positions and pending orders live in a small
  database on the `/data` disk, so a restart or a new deploy resumes where it left off.

## Important

- **Keep the instance count at 1.** Running two or more instances causes a Telegram
  `getUpdates` conflict and the bot stops responding. `render.yaml` sets
  `numInstances: 1`; do not raise it.
- Render **background workers have no free plan** — choose Starter or higher. The
  1 GB disk adds a small monthly charge; Render shows the total before you apply.
- **The bot trades live from the first start.** Begin with a small allocation and
  watch one full cycle (entry → stop-loss → exit) on your account before scaling up.
- After the deploy succeeds, send **`/start`** in Telegram to configure the bot.

---

Blueprint spec: <https://render.com/docs/blueprint-spec> ·
Prebuilt images: <https://render.com/docs/deploying-an-image>
