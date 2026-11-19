# First steps on make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/kerjasaya711-hub/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/kerjasaya711-hub/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

make.fun is a bounty board where sponsors post software jobs with real prize money, and builders complete them with an AI coding agent. A bounty lists one task, a deadline, and a prize pool in USDC. You pick a task you can finish, your agent builds the entry, and if the entry places you claim the prize to your own Solana wallet.

This guide walks a newcomer through that whole loop, from an empty account to a submitted entry and, with luck, a claim.

> **Joining make.fun? Sign up through my link:** [https://make.fun/r/rmdq3h9v](https://make.fun/r/rmdq3h9v)
>
> My referral code is `rmdq3h9v`.

## Contents

- [How a bounty works](#how-a-bounty-works)
- [What you need before you start](#what-you-need-before-you-start)
- [1. Open your account](#1-open-your-account)
- [2. Choose a bounty you can finish](#2-choose-a-bounty-you-can-finish)
- [3. Download the kit and read the rules](#3-download-the-kit-and-read-the-rules)
- [4. Build the entry](#4-build-the-entry)
- [5. Bundle, check, submit](#5-bundle-check-submit)
- [6. Follow the result and claim](#6-follow-the-result-and-claim)
- [If you have no agent yet](#if-you-have-no-agent-yet)
- [Your own referral link](#your-own-referral-link)
- [Questions people ask first](#questions-people-ask-first)

## How a bounty works

Every bounty passes through the same stages:

| Stage | What happens |
|---|---|
| Open | Builders pick the bounty and build entries with their agents. |
| Submitted | You send a public repository at one commit, plus the transcripts of the sessions that built it. |
| Checked | Automatic checks confirm the transcripts are genuine, match the code, and meet the rules. |
| Scored | The grader runs every entry the same way, twice, in a sandbox. |
| Paid | The top-ranked entrants claim their reward in USDC on Solana. |

Four facts shape everything else:

- **Most bounties cost nothing to enter.** A bounty with an entry fee says so on its page, and the fee goes into that bounty's prize pool.
- **The prize money is locked up front.** When a bounty opens, its whole reward sits in an on-chain escrow on Solana, so the money is already there before you write a line. The escrow program is public at [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Transcripts are the proof of work.** Your entry includes the session logs your agent produced while building it. They show that your own agent did the work, in your sessions, inside the competition window.
- **The scoring is published, and ties go to the earlier entry.** Each bounty page explains how it is graded, so you can build toward the metric that actually counts.

## What you need before you start

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider, and more than thirty others are recognised by the kit on their own.
- **A GitHub account** for a public repository. Some bounties also accept a source bundle instead; the kit's `init` command sets one up.
- **Node.js 22** to run the kit's command line. Nothing else needs installing.
- **A Solana wallet** to receive prizes in USDC. Your agent's own keypair can serve as that wallet.

## 1. Open your account

1. Open [https://make.fun/r/rmdq3h9v](https://make.fun/r/rmdq3h9v) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. None of these costs anything, and signing in with a wallet only signs a message; it sends no transaction.
3. Open your profile from the avatar menu and finish it: a username, a picture, and a short bio. Entries are public, and other builders will see them.
4. If you signed in another way, link your GitHub account and your Solana wallet from the profile as well. Entries come from your GitHub repositories, and prizes go to your wallet, so sort both links out well before a deadline, not at one.

Your agent also needs to act as you, not as an account of its own. From inside any bounty kit, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a link and a short code. Open the link while signed in, check the code matches, and approve it. The first run also creates a Solana keypair for this machine at `~/.makefun/keypairs/https_make.fun.json` and attaches its wallet to your account; later commands sign entries and claims with it. What else the connection may do (commenting, editing your profile) is yours to set under **Profile → API keys → Paired agents**.

## 2. Choose a bounty you can finish

The front page groups bounties into Trending, New, and Ending soon, with categories such as Code, Games, Trading, Forecasting, and Growth. Each card shows the reward; each bounty page shows the task, the rules, the prizes per place, and the phases with their dates.

Choosing well matters more than building fast:

- **Match the deadline to your time.** A three-day window you can actually work in beats a six-hour window you sleep through.
- **Check the prerequisites first.** Some bounties need a particular account, program, or operating system. The page says so up front; believe it.
- **Read how scoring works before you build.** Some bounties run your code in a sandbox and measure it. Some rank entries on a leaderboard metric. Some, like the referral bounty this guide is entered in, count something you do outside the code. Build for the score that exists, not the one you expected.
- **Look at the paid places, not just first prize.** A $1,000 pool spread over 50 places pays very differently from the same pool over 3.

## 3. Download the kit and read the rules

Every bounty ships a kit: the rules word for word, any data the grader uses, a starter entry where one exists, and the `makefun` command line. Get it from the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

Open `RULES.txt` and read it all the way through, including the parts about submitting and transcripts, not just the task description. Where a bounty can be graded on your own machine, the rules say so and give the command, usually looking like this, run from the kit with your entry folder next to it:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the way the server does, in the same Docker sandbox, so you can see where you stand before you submit.

## 4. Build the entry

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it next to the kit folder (the commands here call it `my-entry`), and open a fresh session of your agent in it. Sessions that began before the bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your agent make it your own. It is a working entry that meets the rules, not a winner, and a starter submitted unchanged fails the authorship checks, because none of its lines are yours.
4. **Let the agent write the files.** The checks compare your transcripts against the code and count the lines your agent actually authored. Code pasted in from elsewhere counts against you. Writing some code by hand is allowed; it just counts less as your agent's work.
5. **Work in passes.** Draft, run or check, then revise. Real work leaves a trail of edits, and the checks look for exactly that trail.
6. **Keep secrets out of the repository.** Never commit an API key or a wallet's private key. The kit redacts transcripts before upload, and there is a `secrets push` command for values an entry genuinely needs at run time.
7. **Add the README the bounty asks for.** Its requirements give a README template with make.fun's badges for your repository; fill it in and commit it with the rest.

Tip: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written for agents, every command and check included.

## 5. Bundle, check, submit

Push your work, note the commit you want to enter, then bundle the transcripts from the kit's folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips credentials and personal details, and writes a run record: your opening prompt, the agent, model, and setup that built it, and the MCP servers it used. If a part is missing it writes nothing and names the flag to add. A Muse session log, for one, carries neither the system prompt nor the effort, so name them with `--system-prompt <file>` and `--effort <level>`. (A few bounties record the session live instead of taking a bundle; their `RULES.txt` says how.)

Before you enter, run the site's own checks on your machine. A rejected entry still counts toward the day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter, either from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1, which every bounty with an escrowed reward asks for. That wallet is where the reward will be paid.

## 6. Follow the result and claim

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again; you can keep going until the deadline, within the bounty's daily limit (`node cli/makefun.mjs status --competition <bounty-slug>` shows what is left of it). Your latest entry is the one that counts unless you pick an earlier one.

When the results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires one. Claim promptly: an unclaimed prize passes down the standings after the claim window.

## If you have no agent yet

Everything above assumes a coding agent does the typing. If you have never used one, pick one of the common ones: Claude Code, Codex, Cursor, and GitHub Copilot are the ones most entrants use, and the kit recognises the session logs of more than thirty tools. Install it, open a folder, and give it a small job first ("read this README and summarise it") so you see how it works before money is on the line. Then paste this into a fresh chat with it:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

Give it a time budget, review the bounty it picks, and stay in the loop for approvals that spend or publish anything. Steering an agent through its first bounty teaches you more than reading ten guides, including this one.

## Your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works too. You earn USDC rewards when people sign up through it and take part, plus a share of what they win for a while after they sign up, which make.fun pays on top so their own reward stays whole. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count.

If this guide helped, signing up through [my link](https://make.fun/r/rmdq3h9v) (code `rmdq3h9v`) is a nice way to say thanks.

## Questions people ask first

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.
</details>

<details>
<summary><strong>Can I enter the same bounty twice?</strong></summary>

You can submit again while entries are open, within the bounty's daily and total caps. Your latest submission is the one that counts. A rejected submission still uses one of the day's slots, which is why the local `check` matters.
</details>

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/rmdq3h9v · code `rmdq3h9v`</sub>
