# Solana Anchor Claude / Cursor AI rules

🆕 Updated for Anchor 0.32.1

Here's a Claude / Cursor AI rules file I'm using to create and edit my Solana Anchor projects.

## How do I use these files?

Claude: clone this repo, and copy `CLAUDE.md` into your project

Cursor and others: clone this repo, and copy `CLAUDE.md` into `AGENTS.md`

## How do I use Cursor AI to build a Solana Anchor program?

Here's the process I'm currently using to get results I'm happy with.

Install these files as per the instructions above.
Then:

- Make a proof of concept in Excel. Yes Excel. That allows you to think about the economics of your project before you implement them.

- Install Rust Analyzer, use 'Cursor: New Composer' and the Anchor 'multiple' template so it's not a single giant lib.rs. Rename `intructions` to `handlers` (these aren't instructions, they're instruction handlers).

- Write this prompt:
  > I want you to create a (whatever) using Anchor and Rust. But first I want you to to tell me what you think about this design.
  >
  > (Your design - 'Each foo contains many bars, when the doThing instruction handler is run, tokens will be moved from....' etc.)
  >
  > (add the testing scenario you made in Excel to help it understand what you want to build.)

Then:

- Let Cursor AI tell you what it thinks about the design. You may have design issues or missing pieces. Upgrade the prompt and restart from Step 1.
- When it doesn't have much feedback left, let it code.
- Cursor will start coding incrementally and then talk to you. Fix all the Rust Analyzer errors immediately and commit to git before continuing.
- Iterate through the generation process until you're done.
