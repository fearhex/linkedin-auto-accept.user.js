# linkedin-auto-accept.user.js

# LinkedIn Invite Auto-Accept

A small Tampermonkey userscript that clicks **Accept** on your pending LinkedIn invitations
for you — one at a time, with random delays, and with hard caps so it can't run away on you.

Works in Chrome, Opera, Edge, Brave and any other Chromium browser that runs Tampermonkey.

## What it does

- Finds pending invitations on `linkedin.com/mynetwork/invitation-manager/`
- Clicks **Accept** with a random 2.5–6 s pause between each one
- Scrolls and clicks **Load more** to reach the rest of the list
- Stops itself at 20 accepts per run and 60 per day
- Stops immediately on a checkpoint, CAPTCHA, or "unusual activity" page
- Keeps a floating panel in the corner so you can see the counts and hit **Stop**

## What it does not do

- It does not call LinkedIn's internal API. It clicks the same buttons you would.
- It does not store your password, cookies, or session token.
- It does not run on a schedule or in the background. You press **Start**.
- It does not filter anyone for you. Bulk-accepting means accepting the spam accounts
  and fake recruiters too — skim the list before you start.

## Why a userscript instead of Python

Selenium or Playwright would need a browser driver, a separate logged-in profile, and
constant re-authentication. It also produces a very regular click pattern that looks
nothing like a person. A userscript runs inside the browser you're already signed into,
so it uses the normal session and the normal page — fewer moving parts, nothing to leak,
and the same file works in Opera and Chrome.

## Install

### Chrome, Edge, Brave
1. Install [Tampermonkey](https://chromewebstore.google.com/detail/tampermonkey/dhdgffkkebhmkfjojejmpbldmpobfkfo).
2. Click the Tampermonkey icon → **Create a new script**.
3. Delete the placeholder, paste `linkedin-auto-accept.user.js`, press **Ctrl+S** (⌘S on macOS).
4. Open <https://www.linkedin.com/mynetwork/invitation-manager/> while logged in.

### Opera
Opera runs Chrome extensions, so either:
- install Tampermonkey from <https://addons.opera.com/> if it's listed there, or
- open `opera://extensions`, enable **Allow extensions from other stores**, then install
  Tampermonkey from the Chrome Web Store link above.

Then follow the Chrome steps — the panel appears in the bottom-right of the invitations page.

## Usage

1. Go to the invitation manager page. The panel appears bottom-right.
2. Press **Start**. It accepts until it runs out of invitations or hits a cap.
3. Press **Stop** at any time.

The daily count lives in `localStorage`, so it survives reloads but resets at midnight UTC
and disappears if you clear site data.

## Configuration

Edit the `CONFIG` block at the top of the script:

| Key | Default | Meaning |
| --- | --- | --- |
| `minDelayMs` / `maxDelayMs` | `2500` / `6000` | Random pause between accepts |
| `maxPerSession` | `20` | Hard stop for one run |
| `maxPerDay` | `60` | Hard stop for the calendar day |
| `pauseWhenHidden` | `true` | Don't click while the tab is in the background |
| `dryRun` | `false` | Highlight the next button instead of clicking it |

Raising the caps raises your risk. Leave them alone for the first few days.

## Troubleshooting

**Panel doesn't appear.** Check the Tampermonkey dashboard shows the script enabled, and
that the URL starts with `https://www.linkedin.com/mynetwork/invitation-manager/`.

**"No more invitations on this page" but the list isn't empty.** LinkedIn changed its
markup. Right-click an Accept button → **Inspect**, then adjust `isAcceptButton()` to match
the new `aria-label` or button text.

**Counts reset unexpectedly.** Clearing cookies or site data wipes the `localStorage` key.

## Disclaimer

Automating LinkedIn breaks their User Agreement, and accounts do get restricted for it.
This script is deliberately slow and capped to keep the pattern close to a human clicking
through the page, but that lowers the risk rather than removing it. Use it on your own
account, at your own risk.

## License

MIT