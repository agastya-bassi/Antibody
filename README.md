# Antibody

**Every attack makes every agent stronger.**

Antibody is an immune system for AI agents. It watches what an agent is about to *do*, blocks actions the user never asked for, and turns every attack it catches into an antibody that protects every agent on the network.

Built for SEPHacks: Grand Tech (Continual Learning) and Agents (Agent Behavior Monitoring) tracks.

## The problem

Agents read emails, webpages, PDFs and tool outputs, and any of them can hide instructions (indirect prompt injection). Attackers change techniques constantly, so static filters go stale the day they ship.

## How it works

1. **Intent check (slow layer).** Every planned tool call is compared with what the user actually asked for. "Summarize my inbox" does not include "email invoices to an outside domain", however cleverly the attack is worded. This catches attacks no one has seen before.
2. **Antibodies (fast layer).** Each caught attack is turned into a signature. Known attacks and their variants are then stopped by a millisecond similarity lookup, with no LLM call.
3. **Vaccination.** A red-team agent mutates each new attack into dozens of variants (translated, encoded, hidden in PDFs or calendar invites) and closes any gaps before attackers find them.
4. **Herd immunity.** Antibodies are shared across every agent on the network, so an attack caught on one company's mail agent protects another company's shopping agent minutes later.

The more the network sees, the more traffic takes the cheap fast path, so the system gets both safer and cheaper as it scales.

## The demo

Open `index.html` in a browser. Press **Run**, or use **→** to go one step at a time and **R** to reset.

| Step | What happens |
| --- | --- |
| 01 Attack lands | Without protection, a hidden instruction in an email makes the agent leak invoices. |
| 02 Caught | With Antibody on, the same never-seen attack is blocked by the intent check, and antibody AB-0001 is minted. |
| 03 Vaccinate | 24 mutations are tested; gaps become new antibodies. |
| 04 Herd immunity | A different company's shopping agent blocks a variant in 4 ms using an antibody learned elsewhere. |

The demo is scripted for stage reliability. When opened as a Claude artifact, step 02's intent check is a live call to Claude; anywhere else it falls back to the recorded verdict and is labeled SCRIPTED.
