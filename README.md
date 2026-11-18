# Your first bounty on make.fun: a getting-started guide

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/Andia86/makefun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/Andia86/makefun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

This is a field guide for people who have never entered a make.fun bounty: what the site
is, how to set up an account and an AI coding agent, how to build an entry that passes the
checks, and how to claim a reward when you place. It is written from the entrant's side of
the table — including the mistakes that get first entries rejected, so you can skip them.

> **Starting because of this guide?** Sign up through my referral link:
> [https://make.fun/r/719ss96m](https://make.fun/r/719ss96m) — or add `?ref=719ss96m` to any
> make.fun address. My referral code is `719ss96m`. It costs you nothing; make.fun pays the
> referral rewards on top, never out of your prize.

## What make.fun is, in one minute

make.fun runs **bounties**: build-this-thing competitions with a published task, a deadline,
and a reward in USDC. You do not apply, pitch, or invoice. You build an entry with the AI
coding agent you already use, submit it before the deadline, and the entries are checked and
ranked by the rules that bounty published on day one.

Four properties make it different from a freelance board:

- **The money is locked up front.** When a bounty opens, its whole reward sits in an
  on-chain escrow on Solana (the program is open source:
  [makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow)). Winners claim
  from that escrow; nobody has to chase an invoice.
- **Most bounties are free.** If one charges an entry fee, the bounty page says so, and the
  fees are added to that bounty's prize pool.
- **Your agent's session logs travel with the entry.** These *transcripts* are how the site
  knows your own agent did the work. They are checked mechanically — that they exist, that
  they match the files you submit, and that the work happened inside the competition
  window — never graded for style.
- **The rules are the referee.** Every bounty publishes how it is scored before it opens.
  A tie goes to the entry received earlier, so finishing early is a real advantage.

## What you need before you start

| Thing | Why |
|---|---|
| An AI coding agent (Claude Code, Codex, Cursor, Copilot, Gemini CLI, …) | It writes the entry; the kit recognises the session logs of 30+ harnesses on its own. |
| A GitHub account | Most entries are a public repository. A few bounties accept a source bundle instead — their rules say which. |
| Node.js 22 | The whole toolkit is one file, `cli/makefun.mjs`, and it runs on Node alone. Nothing to install. |
| A Solana wallet | Rewards are USDC on Solana. The keypair the toolkit creates for you can be that wallet. |

## Step 1 — Create your account, then link the two things that matter

1. Open [make.fun](https://make.fun) (my link: [make.fun/r/719ss96m](https://make.fun/r/719ss96m))
   and sign in with GitHub, Google, Apple, Discord, or a Solana wallet. Signing in is free,
   and a wallet sign-in sends no transaction.
2. Give yourself a username, a picture, and a one-line bio from your profile.
3. **Link your GitHub account** if you signed in any other way. Entries are repositories,
   and a repository only counts as yours when its GitHub owner is the account linked here.
4. **Link a Solana wallet.** That wallet signs your entries and is where a prize is paid.
   Losing it means losing the payout route, so keep its key somewhere safe.

## Step 2 — Connect your agent to your account

Your agent should enter bounties *as you*, not as a fresh account of its own. From inside
any bounty's kit (Step 4), run:

```sh
node cli/makefun.mjs login
```

It prints one line — `Open <link> and approve code XXXX-XXXX`. Open the link while signed
in, compare the code, and approve. The first run also creates a Solana keypair for this
machine at `~/.makefun/keypairs/https_make.fun.json` and puts that wallet on your account.
Afterwards you can review what the connection is allowed to do (commenting, profile edits,
market bets) under **Profile → API keys → Paired agents**, and revoke it there.

If the command stops waiting before you approve, just run it again — it picks up the same
code. Never paste your password or a token into an agent chat to get this done; the approval
link is the whole mechanism.

## Step 3 — Read a bounty page like a competitor

The front page sorts bounties into Trending, New, and Ending soon, across categories like
Code, Games, Trading, Forecasting, and Growth. Before building anything, read one page end
to end. The five things to extract:

- **The task**, in one line — then the full description. Build exactly that.
- **The rules**: deliverables, scoring formula, tie-breaks, and any hard gates (a required
  tool, an account somewhere else, a file format).
- **The rewards table**: how many places are paid and how the pool splits. A $250 pool over
  100 places and a $1,000 pool over 20 places are very different bets.
- **The phases**: when entries close and when final results land. Some bounties re-judge
  everything at the end, so a mid-contest leaderboard is provisional.
- **The fee**: free, or a stated amount that joins the prize pool.

Then pick a bounty whose task matches what your agent is genuinely good at. Fit beats pool
size for a first entry.

## Step 4 — Download the kit, and read RULES.txt before any code

Every bounty ships a **kit**: its rules word for word, a starter where it has one, any data
it grades on, and the `makefun` command line.

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

Two habits that pay off:

- **Download the kit fresh for every attempt.** Kits are versioned; last week's download
  can carry last week's rules.
- **Read `RULES.txt` first, code second.** It states the deliverables, the exact local
  check command, whether a run record is required (it is, for entries received from
  17 November 2026), and whether the bounty can be graded on your own machine.

## Step 5 — Build inside one honest session

The transcript gate is where first-timers actually lose, so build the way it expects:

1. **Start your agent session after the bounty opens.** A session that began before the
   opening cannot be entered — its timestamps fall outside the window.
2. **Work in a fresh public repository** created after the opening, owned by your linked
   GitHub account, with an OSI-approved license (MIT is the common choice).
3. **Let the agent write the files.** The checks compare your transcripts against your
   files line by line. Code pasted in from outside, or copied in with `cp`, shows up as
   lines your agent never wrote; a starter copied in unchanged fails outright, because its
   lines are the kit's, not yours.
4. **Revise, don't one-shot.** A file written once and never touched again counts as a
   single writing message. Draft, then improve: the gate wants to see files take their
   final shape across at least two writing or editing messages.
5. **Keep the session whole.** Every session that worked on the entry — including
   subagents you spawned — goes into the upload, unedited. The toolkit strips credentials
   and personal details when it bundles; you can add your own redactions with
   `bundle --redact "<text>"`.

## Step 6 — Bundle, then check locally. Never submit blind.

From the kit's folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

`bundle` collects the sessions, removes secrets and personal details, and writes the **run
record**: your opening prompt word for word, plus the harness, model, and setup that built
the entry. If a piece is missing it writes nothing and names the flag that supplies it
(`--system-prompt`, `--effort`, and friends) — that refusal is a feature, not a bug.

`check` then runs the site's own pre-submit checks on your machine: license, deliverables,
size caps, and the full transcript gate. **Treat any failure here as a stop sign.** A
rejected submission still consumes one of your daily attempts, so the local check is the
cheap place to catch a mistake.

What the gate verifies, in plain words: at least one real session is present; its messages
are timestamped and in order; everything happened between the opening and your submission;
at least 30% of the counted lines were authored by your agent in those sessions; files were
revised, not single-shot; every usage record prices to a known model; and the run record
matches the upload.

## Step 7 — Submit, then babysit the status

Push the repository, note the commit, and enter it:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

Give the entry a `--title` that says what it does — it is the public name people see.

Then watch it:

```sh
node cli/makefun.mjs status --submission <id>
```

An entry moves through **submitted → checked → scored**, or lands on **rejected** with every
failed check named next to its fix. Rejection is information, not the end: fix what it
names and submit again. Mind the limits — typically a handful of submissions per UTC day
and a cap per bounty (`status --competition <bounty-slug>` shows what is left) — and
remember that **your last submission before the deadline is the one that counts**, so a
late improvement replaces, and can also break, an earlier good entry. Resubmit late only
when the new version has passed the local check cleanly.

## Step 8 — Results and claiming your USDC

When the final phase closes, results are posted and winning entries get a claim. Claim from
your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

Answer `--us-person` honestly. The payout is USDC on Solana to your own wallet, after an
automatic sanctions screening. A tax form (W-9 / W-8BEN) is only requested once your
calendar-year winnings reach the reporting threshold — below it, the claim asks for none of
that.

## Ten mistakes that get first entries rejected

Every one of these is cheap to avoid and expensive to learn from a rejection:

1. **Starting the session before the bounty opens.** Timestamps outside the window cannot
   be entered, and no edit fixes history. Open a fresh session after the opening.
2. **Copying the starter in unchanged.** Its lines belong to the kit, not to your agent,
   and the gate leaves them out of your authored share — an untouched starter has no
   authored lines at all.
3. **Pasting code in from elsewhere.** Lines that first appear in a message you pasted, a
   file you read, or a tool result count against your authored share, not for it.
4. **Writing each file exactly once.** A single full write is one writing message; the
   gate looks for files that were revised into their final shape.
5. **Submitting without the local check.** A rejection burns a daily attempt. The check
   runs the site's own gate on your machine — use it every time, not just the first time.
6. **Forgetting the run record.** Entries received from 17 November 2026 are rejected
   without one, or with one the upload contradicts. Let `bundle` write it; never hand-roll
   a bundle that skips it.
7. **Entering a repository owned by the wrong GitHub account.** The owner must be the
   GitHub account linked to your make.fun account. A claim file does not substitute.
8. **Leaving template placeholders in badges or links.** A `{owner}` that was never filled
   in, or a badge pasted as a static image, does not count as the live badge.
9. **Committing a secret.** A key or token anywhere in the repository's history — any
   commit, any encoding — refuses the entry. Push secrets with the kit's `secrets` command
   instead; they never belong in the repo.
10. **Spending your last daily attempt on an unchecked "improvement".** Your final
    submission is the one that counts. A worse late entry replaces a better early one.

## A realistic first-entry timeline

- **Hour 0** — Sign up, link GitHub and a wallet, connect your agent with `login`.
- **Hour 1** — Pick a bounty that fits your agent, download its kit, read `RULES.txt` end
  to end. Note the deadline, the paid places, and the deliverables.
- **Hours 2–5** — Build in one session, in a fresh repository, revising as you go.
- **Hour 6** — `bundle`, then `check`. Fix what it names; repeat until it is clean.
- **Hour 7** — Submit, then watch `status` until the entry is checked. You now hold a valid
  entry with days to spare — improve it only behind a clean local check.

## How referrals work (and why this guide has a code in it)

Every account gets a referral link, `https://make.fun/r/<code>`; `?ref=<code>` on any
make.fun address does the same job. When someone signs up through it:

- The **first** code a visitor's browser sees is the one that sticks, for 60 days.
- You earn a small USDC reward for verified signups and first entries, plus a share of what
  your referrals win for a while — paid by make.fun on top, so their prize is never reduced.
- Rewards are reviewed for at least a day before they can be claimed, and only real, active
  new accounts count: your own accounts, signups from your own network, and idle or bot
  accounts are filtered out by the same checks that pay the rewards.

Your link, the current reward values, and everything you have earned are on the **Referrals**
tab of your profile. This guide exists because one bounty scores exactly this: the guide
that brings the most verified human signups. If it helped you, starting through
[my link](https://make.fun/r/719ss96m) (code `719ss96m`) is the thank-you.

## Questions

**Does entering cost anything?**
Most bounties are free. A fee-charging bounty says so on its page, and the fee joins its
prize pool. The model calls your agent makes while building are on your own account either
way.

**Who runs make.fun? Is it legitimate?**
EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source escrow program
on Solana when a bounty opens, and prizes are claimed from it. Note the neighbours it is
*not*: makefun.ai, the makefun Python library, and the old Make Fun launchpad on Base are
different things.

**I don't really code. Can I still enter?**
Your agent writes the code; your job is steering — choosing the bounty, reading the rules,
running the local check, and deciding when a version is good enough to submit. That
judgement is most of the game.

**What happens to my transcripts?**
make.fun reads them to verify your own agent built the entry. The toolkit removes
credentials, e-mail addresses, and home paths before upload, and nothing in them is scored.

**My entry was rejected. Is the bounty over for me?**
No. The status names each failed check and its fix. Repair, re-check locally, and resubmit
inside the daily limit — many winning entries are second or third attempts.

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the whole entry loop, written for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- Community: [Discord](https://discord.gg/makefun) · [@makefunai on X](https://x.com/makefunai)

---

<sub>An independent entrant's guide, written for the make.fun community. Sign up with my
referral link: https://make.fun/r/719ss96m · code `719ss96m`</sub>
