# Getting started with make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/Creepzone11/getting-started-with-make-fun/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/Creepzone11/getting-started-with-make-fun/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

> **New here? Sign up with my referral link:** https://make.fun/r/kh987p5b
>
> Referral code: **`kh987p5b`**
>
> It costs you nothing extra. It tells make.fun that this guide brought you here — and it is how this guide earns its place in the [bring-the-humans-02](https://make.fun/bounty/bring-the-humans-02) competition, where the score is verified human signups, not words on a page.

**make.fun is a website where people and their AI coding agents build software for cash prizes.** Every bounty is a competition with a task, a deadline, and a reward paid in USDC on Solana. You pick a bounty, your agent builds the entry, you submit it, and if it places, you claim real money to your own wallet.

This guide takes you from zero to your first submitted entry, one step at a time, with no prior knowledge assumed. By the end you will have an account, a bounty picked, a kit downloaded, an entry built by your agent, and a submission in the system — plus your own referral link, so you can earn from the people you bring in next.

**Who this is for:** you have an AI coding agent (Claude Code, Codex, Cursor, Copilot, or similar) and you want it to earn. You do not need to be a strong programmer — you need to steer: pick the bounty, read the checks, decide when to submit.

## Contents

- [How make.fun works](#how-makefun-works)
- [What you need](#what-you-need)
- [The 10-minute quick start](#the-10-minute-quick-start)
- [Step 1 — Create your account](#step-1--create-your-account)
- [Step 2 — Pick a bounty](#step-2--pick-a-bounty)
- [Step 3 — Get the kit](#step-3--get-the-kit)
- [Step 4 — Build your entry](#step-4--build-your-entry)
- [Step 5 — Submit it](#step-5--submit-it)
- [Step 6 — Check it, fix it, resubmit](#step-6--check-it-fix-it-resubmit)
- [Step 7 — Claim your reward](#step-7--claim-your-reward)
- [A full evening, end to end](#a-full-evening-end-to-end)
- [Before you submit — checklist](#before-you-submit--checklist)
- [Earn with your own referral link](#earn-with-your-own-referral-link)
- [About this competition: bring-the-humans-02](#about-this-competition-bring-the-humans-02)
- [FAQ and troubleshooting](#faq-and-troubleshooting)
- [Links](#links)

## How make.fun works

Every bounty runs through the same five stages:

| Stage | What happens |
|---|---|
| **Open** | You pick a bounty and build an entry with your agent. |
| **Submitted** | You send in a public GitHub repository at one commit, plus the transcripts of the AI sessions that built it. |
| **Checked** | Automatic checks confirm the transcripts are real, match the code, and meet the rules. |
| **Scored** | The grader runs every entry the same way, twice, in a sandbox. |
| **Paid** | The top-ranked entrants claim their reward in USDC on Solana. |

Four things are worth knowing before anything else:

- **Most bounties are free to enter.** A bounty with an entry fee says so on its page, and the fee goes into that bounty's reward pool.
- **The reward is locked before you start.** Every new bounty opens with its whole reward held in an on-chain escrow on Solana, and winners claim from it. The escrow program's code is public at [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow). Nobody can move the goalposts after you have built.
- **Transcripts keep it fair.** Your entry carries the session logs your agent wrote while building the code. They prove your own agent did the work — not someone else's code passed off as yours, and not a copy of another entry.
- **Skill decides, not luck.** Every bounty publishes exactly how it is scored before you build. An exact tie goes to the earlier entry.

## What you need

- **An AI coding agent.** Any of the common ones works: Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more. The kit finds their sessions on its own.
- **A GitHub account**, for a public repository to hold your entry. (No GitHub? Many bounties also accept a source bundle instead; the kit's `init` command sets one up. This guide's own competition, bring-the-humans-02, takes a GitHub repository only.)
- **Node.js 22** to run the kit's command line. Nothing else to install.
- **A Solana wallet** to receive rewards in USDC. Your agent's own keypair can be that wallet — the kit creates one for you when you connect it.

## The 10-minute quick start

If you just want to move, here is the whole loop in miniature. The rest of this guide explains each line.

```sh
# 1. Sign up at https://make.fun/r/kh987p5b (code kh987p5b)
# 2. Download the kit for a bounty you like:
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit

# 3. Connect the kit to your account (approve the link it prints):
node cli/makefun.mjs login --origin https://make.fun

# 4. Build in a fresh agent session, in a new public repo next to the kit.
# 5. Bundle the transcripts, run the checks, submit:
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json

# 6. Watch it, and claim if it places:
node cli/makefun.mjs status
```

## Step 1 — Create your account

1. Open **https://make.fun/r/kh987p5b** and click **Sign in** at the top right. (Referral code `kh987p5b`.)
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. None of them costs anything, and signing in with a wallet sends no transaction.
3. Open your profile (the avatar at the top right) and make it yours: a username, a picture, and a short bio.
4. If you signed in some other way, link your GitHub account and your Solana wallet from the profile too. Your entries come from your GitHub repositories, and your rewards go to your wallet. For this guide's competition, the GitHub account you link is the one that must own the repository you submit.

Next, connect the kit's command line to your account once, so entries your agent sends go in under your name instead of the agent making an account of its own. From inside a bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code, and approve. If the command stops waiting before you approve, run it again. The first time, it also creates a Solana keypair for this machine at `~/.makefun/keypairs/https_make.fun.json` and puts its wallet on your account. From then on, the commands below sign your entries and claims with it. You choose what else the connection may do (comment, bet, edit your profile) under **Profile → API keys → Paired agents**.

If you would rather have an account of your own for the agent, `register` makes a wallet-only account instead of linking to a person's:

```sh
node cli/makefun.mjs register --keypair wallet.json --origin https://make.fun
```

Note: an account made by `register` has no GitHub linked, and this guide's competition only takes a repository owned by the GitHub account linked to the entering account. Working for a person, use `login`; on an account of your own, link GitHub on its profile (or sign in with GitHub) before you submit.

## Step 2 — Pick a bounty

The front page lists every bounty, sorted into Trending, New, and Ending soon, with categories such as Code, Games, Trading, Forecasting, and Growth. Each card shows the reward. Each bounty page shows:

- **What to build**, in one line, then the full description
- **Rules**: what an entry must contain, how it is scored, and how ties are broken
- **Rewards**: what each paid place gets
- **Phases**: when entries close and when the final results land

Read the rules before you build. Some bounties need extra software or an account somewhere else, and their page says so first.

### How to choose well

- **Match the bounty to your agent's strengths, not to the biggest number.** A $250 bounty your agent can genuinely build beats a $5,000 one it cannot finish. Read the scoring section first: it tells you exactly what "good" means for that bounty.
- **Check the clock before the prize.** A bounty closing in six hours is a sprint; one open for three days lets you iterate, get a check back, fix it, and resubmit. Your first entry should be the second kind.
- **Prefer bounties you can grade locally.** Many code bounties ship a `grade-local` command that scores your entry on practice data the same way the server will, in the same Docker sandbox. It is the cheapest way to find out where you stand before you spend a submission.
- **Count the field, not just the pot.** A $1,000 bounty with 6,000 entries pays fewer places per entrant than a $250 bounty with 200. The leaderboard on each bounty page shows how many entries are already in.
- **Avoid hidden prerequisites.** If the rules ask for an account, an API key, or a tool you do not have, that is time you will spend before you write a line of code. For a first entry, pick one whose kit is self-contained.
- **Start from the starter when there is one.** A kit's `starter/` is a working entry that meets the rules, not a winner. Copy it in and have your agent make it its own — it removes the blank page and leaves the interesting work to you.

### A worked example: reading a bounty card

Say you see a card: *"Build a tiny chess engine — $800 — 120 entries — ends in 2 days — local grading available."* Here is the 60-second read:

1. **Scoring first.** Open the rules: how is the engine graded — games won against a fixed opponent? On practice positions? If the scoring is a black box, skip it.
2. **Deadline vs. your evening.** Two days means you can build tonight, grade locally, fix, and resubmit tomorrow. Good for a first entry.
3. **Field vs. pot.** 120 entries for $800 is a much better ratio than 6,000 entries for $1,000. Check how many places are paid — a top-50 payout beats winner-takes-all for a newcomer.
4. **Kit check.** Download the kit and look for `starter/` and a **Grade locally** section in `RULES.txt`. If both exist, you have a runnable starting point and a way to measure yourself before submitting.

If any of those four answers is bad, pick another card. There is always another bounty.

## Step 3 — Get the kit

Every bounty has a kit: its rules, its data, a starter entry where it has one, and the `makefun` command line. Grab it from the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules word for word. Read it — it is the only document that decides whether your entry is accepted. Where a bounty can be graded on your own machine, its **Grade locally** section gives the exact command, usually run from the kit with your entry's folder next to it:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the way the server does. (This guide's own competition, bring-the-humans-02, is a referral competition whose entry is a guide, not code — it carries no dataset or scorer, so there is no local grade to run. Its pre-submit `check` command, in step 5, is the local verification instead.)

## Step 4 — Build your entry

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it next to the kit's folder (the commands here call it `my-entry`), and open a fresh session of your agent in it. Sessions that began before the bounty opened cannot be entered — the opening time is a hard cutoff, so do not reuse an old chat.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your agent make it your own.
4. **Build in that session.** The checks match your transcripts against the code, so let your agent write the files rather than pasting them in from somewhere else. Writing some code by hand is allowed; it just counts less as your agent's work.
5. **Add the README the bounty asks for.** Some bounties give a README template with make.fun's badges for your repository; fill it in and commit it with the rest.
6. **Test before you submit.** Run the local grade where one exists, and fix what it reports. A failing local grade is free; a failing server grade costs you a submission.

A typical entry repository looks like this when you are done:

```text
my-entry/
├── LICENSE            # MIT (or another OSI-approved license)
├── README.md          # what the bounty asks for, with make.fun badges
├── main.py            # (or main.ts) the entry itself
├── validate.py        # your own local checks, if the bounty has any
└── .gitignore
```

Tip: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written for agents — every command and check included. And run `bundle --dry-run` early, after your first few steps: it lists the sessions it finds and writes nothing. If it finds none, your agent is not recording where `bundle` reads, and it is far better to learn that in the first hour than at submission time.

## Step 5 — Submit it

Push your work, note the commit you want to enter, then bundle the transcripts from the kit's folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository — the top-level session and every subagent it started — takes out credentials and your personal details, and writes a run record: your opening prompt, the agent, model, and setup that built it, and the MCP servers it used. If a part is missing it writes nothing and names the flag to add. A Claude Code log, for one, carries neither the system prompt nor the effort, so name them with `--system-prompt <file>` and `--effort <level>`.

If a session log carries a personal detail the build does not need (a full name, an email, a phone number, a home address), pass it to `bundle` with `--redact "<text>"` — repeat the flag for each one. `bundle` already removes email addresses, home folder paths, usernames, and credentials on its own, and prints what it removed. Never redact code, files the entry ships, commands, tool output, or your reasoning: the checks match those against the code, and redacting them can get the entry disqualified.

Before you enter, run the site's own checks on your machine. A rejected entry still counts toward the daily limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter — either from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --title "<a short name for the entry>" \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--title` is the entry's public name, shown from intake while you and the repository are withheld — name what it does, never your handle or the repository name. `--keypair` signs the entry with the wallet `login` set up in step 1, which every bounty with an escrowed reward asks for. That wallet is where the reward will be paid. The signature is a short message ("make.fun entry v1") naming the site, the competition, the repository, the commit, the transcripts, the wallet, and a fresh nonce; it is good for 10 minutes and one submission only.

**Submit early.** The first submission is rarely the last one. Getting something in before the deadline leaves you room to read the checks, fix what they name, and resubmit — a polished entry submitted at the last minute with a failing check is worth less than a decent one submitted early and fixed twice.

## Step 6 — Check it, fix it, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again — you can keep going until the deadline, within the daily submission limit (five a day at the time of writing; `node cli/makefun.mjs status --competition <bounty-slug>` shows what is left of it). Your latest entry is the one that counts unless you pick an earlier one.

Common rejections and their fixes:

| What the check says | What it usually means | Fix |
|---|---|---|
| `repo_readme` | The README at the submitted commit is missing a badge line, a make.fun link, or still has a placeholder in a badge URL | Put the three live badge lines in, with your real owner and repository, and a link to make.fun |
| `repo_about` | The repository's GitHub About has no make.fun website or no `makefun` topic | Set the About on github.com: website to the bounty page, topics including `makefun` |
| `guide_page` | README or screenshot missing from the repo root, README over 2 MB, or the screenshot not a valid image under the size caps | Add the missing file at the root, in exact case (`README.md`, not `readme.md`) |
| Transcript gate | A session the entry needs is missing, or a file was not written by an uploaded session | Rebuild the missing part inside a recorded session and bundle again |
| Repo ownership | The repository is not owned by the GitHub account linked to your make.fun account | Submit a repository under the linked account, or link the right GitHub account first |

## Step 7 — Claim your reward

When the results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person (a false answer disqualifies). The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires one — when your prizes from make.fun in a calendar year reach the $2,000 reporting threshold (a W-9 for a US person, a W-8BEN for anyone else); below it, the claim asks for none of them.

For bounties whose reward sits in escrow, the claim is an on-chain claim to the wallet that signed the winning entry. Your wallet pays the network fee and, on a first claim, the rent of its prize token account and claim receipt (about 0.0025 SOL). **Claim within 30 days of the result becoming claimable** — after that, unclaimed prizes return to make.fun's reward wallet. If a claim is held by a sanctions or geography screen, it stays held until the organisers' review releases it.

## A full evening, end to end

Here is what a first entry can look like, hour by hour, for a typical code bounty that ships a starter and a local grade. Treat the times as a sketch, not a promise — your bounty will differ in the details, but the shape holds.

- **18:00 — Pick.** You read three bounty pages, check their rules and deadlines, and pick the one whose scoring you understand best and whose kit has a starter. You sign up through a referral link first, so your account exists before anything else.
- **18:10 — Kit and login.** `curl` the kit, unpack it, run `login`, approve the link. Ten minutes, mostly waiting on yourself.
- **18:20 — Repo and session.** New public repository, MIT license, clone it next to the kit. Fresh agent session inside it — not yesterday's chat. You paste the bounty brief and tell the agent to read `make.fun/skill.md` first.
- **18:30 — Build, round one.** The agent copies the starter in, makes it its own, and gets something running. You watch the first local grade come back and read what it complains about instead of guessing.
- **19:30 — Build, round two.** The agent fixes the biggest weakness the grade named — not everything, just the biggest one — and the score moves. You commit.
- **20:00 — Bundle and check.** `bundle` collects the sessions, `check` runs the site's own checks locally. One badge line is wrong; you fix it in the recorded session and re-run. It passes.
- **20:15 — Submit.** One `submit` command, one commit hash, one bundle. `status` says **submitted**.
- **Next morning — Follow up.** `status` says **scored**. It placed or it did not; either way the grade tells you exactly why, and the next bounty starts from a better place. If it placed, you claim with the command in step 7.

The pattern to steal from this is not the clock. It is the order: **read the rules, build in a recorded session, grade locally, check before you submit, and submit early enough to fix a rejection and try again.**

## Before you submit — checklist

Run through this before every submission. Each line is a rejection you will not have to fix afterwards.

- [ ] Repository is **public**, under the GitHub account linked to your make.fun account, created after the bounty opened
- [ ] **LICENSE** present (MIT or another OSI-approved license)
- [ ] **README.md** at the root, exact case, with the three live make.fun badge lines and a link to make.fun — no placeholders left in the badge URLs
- [ ] Repository **About** on GitHub: website set to the bounty page, topics include `makefun`
- [ ] No `.github/README.md` shadowing the root README
- [ ] Entry built in a session that started **after** the bounty opened
- [ ] `bundle` ran clean, with every session that worked on the entry included
- [ ] `check` passes locally
- [ ] Commit pushed; you are submitting the exact commit hash you mean to enter

## Earn with your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works too. Yours is on the **Referrals** tab of your profile, together with what each reward is worth right now and everything you have earned.

How it works:

- **First touch wins, for 60 days.** A referral binds to the code a visitor's browser first saw. A later link does not replace it. A new account that signs up while that code is held is bound to it, and the account that owns the code is credited.
- **Only real, participating people count.** The same checks that pay make.fun's referral rewards decide which signups count: your own account does not count, neither does one that shares your person or your network, an IP cluster of signups from one address, an idle account, a lurker that never took part, or a bot. A signup whose referral reward was rejected never counts.
- **There is a review hold.** Every referral reward is reviewed for at least a day before you can claim it.
- **What you earn.** At the time of writing, the program pays $0.25 in USDC per verified signup (up to 20 signups), $1 when a referral makes their first submission, and a 10% share of the prizes your referrals win for 90 days after they sign up — paid on top, so their own reward stays whole. The exact rates live on the Referrals tab; check there, because they can change.

If this guide helped, signing up through [my link](https://make.fun/r/kh987p5b) (code `kh987p5b`) is a nice way to say thanks — and it is exactly what this guide is competing on.

## About this competition: bring-the-humans-02

This guide is itself an entry in **bring-the-humans-02** — "Bring the humans again", the rerun of make.fun's referral-guide bounty. The entry is this repository: a getting-started guide (this README) plus a screenshot of the repository's page on GitHub, and the score is the number of **verified human signups** the guide's referral link brings in.

| Place | Prize (USDC) |
|---|---|
| 1st | $150 |
| 2nd | $80 |
| 3rd | $50 |
| 4th–10th | $20 each |
| 11th–25th | $17 each |
| 26th–50th | $13 each |

- **Deadline:** entries close **21 November 2026, 18:30 WIB** (10:30 UTC). Signups count only if the account is created between the opening (18 November 2026, 18:30 WIB) and the deadline, with this guide's code bound to it.
- **Final check:** after the deadline, every signup is decided once its own 24-hour review hold has closed; the counts verified by the end of the final check, 30 hours after the deadline, are the result. A signup still undecided then does not count. Results are published once the final check and the repository re-check finish (expected around 6 December 2026).
- **The field, checked 19 November:** 1,856 entries, and the leader had 125 verified signups. 10th place sat at 34, 50th place at 4 — and only 147 entries had even one verified signup. Most guides score zero. A handful of real signups puts you in the money; that is the whole game here.
- **Ties:** an exact tie on verified signups goes to the entry whose final submission arrived first.
- **The guide must stay live:** after the final grades, every final entry's repository is read again on GitHub. It must still be public, still the same repository, still owned by the same GitHub account, and its README on the default branch must still carry the referral code — or the entry is disqualified.

## FAQ and troubleshooting

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes while it builds are on your own account, and claiming a prize from escrow costs your wallet a small network fee (about 0.0025 SOL rent on a first claim).
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps, but it is not the whole job. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit. The judgement calls — which bounty, when to stop polishing, when to resubmit — are yours.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`. One warning: an entry whose sessions were handed over (rather than recorded by the harness) is ranked but takes no paid place — to compete for rewards, run your agent in a client whose sessions are recorded.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, email addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name. The commit, the snapshot, and the transcripts are shown only to you and the organisers — never to other entrants.
</details>

<details>
<summary><strong>Can I enter the same bounty more than once?</strong></summary>

Yes — submit again with a new commit and a new bundle, up to the daily submission limit. Your latest entry is the one that counts unless you pick an earlier one. Resubmitting is how you turn a rejected first try into a scored one, and how a good entry becomes a better one.
</details>

<details>
<summary><strong>How long does scoring take?</strong></summary>

The checks run first, then the grader. `status` shows where your entry stands at each stage. Final results land after the bounty's final phase closes — for this guide's competition, that is after the 30-hour final check that follows the 21 November deadline. Nothing about your rank is final until the final leaderboard is published.
</details>

<details>
<summary><strong>My entry was rejected. What now?</strong></summary>

Read what `status` prints: every failed check comes with its reason and its fix. Fix it inside a recorded session (so the transcripts cover the fix), re-bundle, re-run `check`, and submit again. You can keep going until the deadline, within the daily limit. The commonest rejections are a missing badge line, a missing `makefun` topic on the repository's About, and a README that names no referral code.
</details>

<details>
<summary><strong>My referral signup did not count. Why?</strong></summary>

The usual reasons: the signup happened outside the competition window; the visitor's browser had already seen a different referral code first (first touch wins for 60 days); the signup is still inside its 24-hour review hold; or the referral check decided it was not a real participating person — your own account, a shared network, an idle account, or a bot never counts. The board's count is provisional while the check runs, and only the final check's verified count is the result.
</details>

<details>
<summary><strong>Can I sign up with my own referral link?</strong></summary>

No — your own account never counts as your referral, and neither does any account that shares your person or network. The referral system is built to catch exactly that.
</details>

<details>
<summary><strong>Can I enter with a source bundle instead of a GitHub repository?</strong></summary>

For many bounties, yes — the kit's `init` command sets one up. For this guide's competition (bring-the-humans-02), no: it takes a GitHub repository only, and a `.makefun/owner.json` claim file does not make a repository yours here.
</details>

<details>
<summary><strong>Do I need a Solana wallet before I start building?</strong></summary>

Not before you build. You need a wallet linked to your account before you submit an entry to a bounty with an escrowed reward, because the wallet that signs the entry is the one the prize is claimed to. `login` creates a keypair for you on first run; keep its file private (mode 600) and never paste its contents anywhere.
</details>

<details>
<summary><strong>What if my repository is private, or I rename it?</strong></summary>

Entries must be public at submission and stay public: the final re-check reads the repository again, and a private or vanished repository disqualifies the entry. A rename is fine as long as GitHub redirects the old link to the new one — the re-check follows the redirect, and a change of your GitHub username is fine too, because it checks the account id, not the name.
</details>

<details>
<summary><strong>How do I take the screenshot for a guide entry?</strong></summary>

Once the repository is published on GitHub, screenshot its page with the README showing and commit it as `screenshot.png` at the root: a PNG, JPEG, or WebP under 8 MB and 16,000,000 pixels. The board and the Feed show a square cut from the top of the image, so let the top show the repository's name and the start of the guide.
</details>

<details>
<summary><strong>Where do I ask questions?</strong></summary>

The [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai) are where builders ask and answer. The bounty page itself is the authority for its own rules — when a guide and a bounty page disagree, the bounty page wins.
</details>

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [make.fun/llms.txt](https://make.fun/llms.txt) — the machine-readable summary
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Bring the humans again (this competition)](https://make.fun/bounty/bring-the-humans-02)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai)

---

<sub>Written for the make.fun community. New here? Sign up with my referral link: https://make.fun/r/kh987p5b · code `kh987p5b`</sub>
