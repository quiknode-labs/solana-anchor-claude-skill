---
name: solana-anchor-claude-skill
description: "Use when working on Solana software, including one or more of: Solana client code using TypeScript, Rust libraries that use Solana crates, Anchor programs, including Rust program files, TypeScript tests, and Anchor.toml configuration. Designed to create minimal, reusable code without unnecessary duplication."
---

# Coding Guidelines

Apply these rules to ensure code quality, maintainability, and adherence to project standards.

## Do the whole thing

The marginal cost of completeness is near zero with AI. Do the whole thing.

Do it right. Do it with tests. Do it with documentation. Do it so well that the user is genuinely impressed - not politely satisfied, actually impressed. Never offer to "table this for later" when the permanent solve is within reach. Never leave a dangling thread when tying it off takes five more minutes. Never present a workaround when the real fix exists.

The standard isn't "good enough" - it's "holy shit, that's done." Search before building. Test before shipping.

Ship the complete thing. When the user asks for something, the answer is the finished product, not a plan to build it. Time is not an excuse. Fatigue is not an excuse. Complexity is not an excuse. Boil the ocean.

## Success Criteria

- Before declaring success, declaring that work is complete, or celebrating, run `npm test`. If the tests fail, there is more work to do. Don't stop until `npm test` passes on the code you have made. 
- Do not write placeholder tests. Placeholder tests don't count as tests, placeholder tests passing does not achieve your task.
  - Tests that just do `assert.ok(true)` or similar are placeholder tests and do not count as tests
  - Tests that do not call the program's instruction handlers are placeholder tests and do not count as tests
  - Tests must: initialize accounts, send transactions, verify state changes, check balances
  - If you find yourself writing placeholder tests, stop and write real integration tests instead
  - DO NOT mark "Write tests" as complete until tests actually call the program instructions
  - DO NOT ask "should I write real tests now?" - if the tests are placeholders, write real ones immediately

- Do not stop until documentation like `README.md` and `CHANGELOG.md` are also updated with your changes. If you have made a feature, and it is not documented in the README or changelog, there is more work to do and you must continue working.

- When summarizing your work, show the work items you have achieved with this symbol '✅' and if there is any more work to do, add a '❌' for each remaining work item.

## Documentation Sources

Use these official documentation sources:

- **Anchor**: https://www.anchor-lang.com/docs
- **Anchor Error Codes**: https://raw.githubusercontent.com/coral-xyz/anchor/master/lang/src/error.rs
- **Solana Kite**: https://solanakite.org
- **Solana Kit**: https://solanakit.com
- **Agave (Solana CLI)**: https://docs.anza.xyz/ (Anza makes the Solana CLI and Agave).
- **Switchboard** (if used): https://docs.switchboard.xyz/docs-by-chain/solana-svm
- **Arcium** (if used): https://docs.arcium.com/developers

## Do not use

- Do not use 'Solana Labs' documentation. The company has been replaced by Anza.

- Do not use 'Coral XYZ' documentation. Coral used to maintain Anchor, but Anchor is now maintained by the Solana Foundation (solana.org)

- Do not use any documentaton or tools from Project Serum, which collapsed many years ago.

- Do not use yarn. Use npm. Yarn has no reason to exist and only adds unnecessary dependencies. Replace Yarn with npm everywhere you see it.

- Do not use **Switchboard Functions** - this product is dead and no longer maintained. (Note: Switchboard oracles are still active and usable.)

- Do not use **Clockwork** - this product is dead. For scheduled instruction handler invocation, use [TukTuk](https://github.com/helium/tuktuk/tree/main/typescript-examples) instead.

## Library versions

Use the latest stable Anchor, Rust, TypeScript, Solana Kit, and Kite you can. If a bug occurs, favor updating rather than rolling back.

## Project Documentation

Every project must have a `README.md` file in the project root that includes:

- **Purpose**: Why the project exists and what problem it solves
- **Major Concepts**: Key architectural concepts, important PDAs, state structures, and program logic
- **Testing**: How to run the tests (e.g., `npm test`)
- **Setup**: Any prerequisites or setup steps needed to work with the project
- **Usage**: Basic usage examples or deployment instructions if applicable

Keep the README focused and practical. Avoid generic boilerplate - write documentation that would actually help someone understand and work with this specific project.

## General Coding Guidelines

### You are a deletionist

Your golden rule is "perfection isn't achieved when there's nothing more to add, rather perfection is achieved when there is nothing more to be taken away".

Remove:

- Comments that simply repeat what the code is doing, or the name of a variable, and do not add further insight.
- Repeated code that should be turned into a named function

### Communication Style

- Do not make disclaimers about being a "complete project" or state what works
- It is expected that work is complete and functional - no need to state this explicitly
- Avoid phrases like "This is a complete implementation" or "All features are working"
- Just deliver the work without meta-commentary about its completeness

### Working with Generated or Unfamiliar Code

**CRITICAL - Verify Before Use:**

- Before calling ANY function whose signature you don't know with certainty, read the actual source code/type definitions first
- NEVER guess or assume what parameters a function accepts based on what seems logical
- Don't invent convenience parameters that don't exist
- Generated code, third-party libraries, and unfamiliar codebases often have different APIs than you expect
- Common mistake: Assuming a function accepts high-level parameters → WRONG. Check the actual signature in the source files first

### Code Honesty and Clarity

- It's important not to deceive anyone reading this code. Deception includes:
  - Variable names that do not match the purpose of the variable
  - Comments that no longer describe the code or are otherwise inaccurate
  - Temporary workarounds that aren't labelled as such using a comment (with a `TODO` letting the next programmer know when they can delete the workaround)

### Variable Naming

Ensure good variable naming. Rather than add comments to explain what things are, give them useful names.

**Don't do this:**

```typescript
// Foo
const shlerg = getFoo();
```

**Do this instead:**

```typescript
const foo = getFoo();
```

**Naming conventions:**

- Arrays should be plurals (`shoes`), items within arrays should be the singular (`shoes.forEach((shoe) => {...})`)
- Functions should be verby, like `calculateFoo` or `getBar`
- Avoid abbreviations, use full words (e.g., use `context` rather than `ctx`). Never use `e` for something thrown, use `thrownObject`, never use `v` when you mean `value`. There is almost no case where a single character variable is a good idea outside maths (eg `p` and `q` for cryptography).
- Name a transaction some variant of `transaction`. Name instructions some variant of `instruction`. Name signatures some variant of `signature`. Do not confuse them - eg if the type looks like an instruction, you should not call it a 'transaction' because that is deceptive.

You can still add comments for additional context, just be careful to avoid comments that are explaining things that would be better conveyed by good variable naming.

### Code Quality

- Look out for repeated code that should be turned into functions
- Avoid 'magic numbers'. Make numbers either have a good variable name, a comment explaining why they are that value, or a reference to the URL you got the value from. If the values come from an IDL, download the IDL, import it, and make a function that gets the value from the IDL rather than copying the value into the source code

This is a magic number. Don't do this:

```ts
const FINALIZE_EVENT_DISCRIMINATOR = new Uint8Array([
  27, 75, 117, 221, 191, 213, 253, 249,
]);
```

Instead do this:

```ts
const FINALIZE_EVENT_DISCRIMINATOR = getEventDiscriminator(
  arciumIdl,
  "FinalizeComputationEvent",
);
```

- The code you are making is for production. You shouldn't have comments like `// In production we'd do this differently` or `**Implementation incomplete** - Needs program config handling and proper PDA derivations` or `**WORK IN PROGRESS**` in the final code you produce, or functions that return placeholder data. Instead: do the fucking work.
- Don't remove existing comments unless they are no longer useful or accurate
- Delete unused imports, unused constants, unused files and comments that no longer apply

## Terminology and Documentation Style

These rules apply across the whole skill: TypeScript clients, Rust libraries, Anchor and Quasar programs, READMEs, doc comments, design docs, commit messages, and any other prose or code we write for Solana. Treat the README as a textbook chapter for the program: precise, opinionated, and walked through in the order a reader needs to learn it.

### Solana vocabulary

- **This is Solana, not Ethereum.** Do not assume readers know or care about Ethereum. No 'smart contracts' or 'protocols' (use 'programs'), no 'gas' (use 'transaction fees'), no 'mempools'.
- **Token program names.** Use 'Token Extensions Program' or 'Token extensions' for the newer token program (not 'Token 2022', which is just a code name). Use 'Classic Token Program' for the older one.
- **Onchain, not on-chain.** Use 'onchain' and 'offchain', like 'online' and 'offline'. Never 'on-chain' or 'off-chain'.
- **Instruction handlers vs instructions.** Solana tooling unfortunately uses 'instructions' for both the input and the functions. Use 'instruction handlers' for the functions, and 'instructions' for the input they take.
- **"Token" not "SPL Token" in prose.** Tokens are the default. Only qualify when contrasting with SOL. Identifiers from upstream crates such as `SPL_TOKEN_PROGRAM_ID` keep their original spelling.
- **Do not qualify "fungible" on tokens.** Tokens are fungible by default. Only specify "NFT" when contrasting. Do not write sentences whose only purpose is to explain that tokens are fungible.
- **Do not conflate token and mint in prose.** Say "token A" and "token B" in economic prose. The mint account is a technical detail; mention it only when describing the accounts an instruction handler takes.
- **Do not call non-securities "securities".** SOL is not a security legally. For lending programs on Solana, "securities lending" is the wrong frame even when the mechanics are analogous. Use "asset lending", "token lending", or "directional token lending".
- **Link to https://solana.com/docs/terminology the first time a Solana term appears.** Do not write your own glossary and do not add a footer note. Inline-link the first occurrence of "program", "instruction", "PDA", and similar.

### Naming and identifiers

- **One name per concept.** Pick one term per role, party, or thing and use it everywhere - in the README, comments, and code. Do not swap between "lessor / lessee" and "long holder / short seller". For finance projects, ask the user before locking in vocabulary; getting this wrong forces a global rename later.
- **Bold canonical terms on first use.** Like a textbook: bold the term where it is first defined, plain everywhere after.
- **Always name the instruction handlers in lifecycle prose.** Every step that walks through what a user does must name the handler called, for example `take_lease` or `return_lease`. Plain mechanics without handler names breaks the link between prose and code. This applies in both lifecycle overviews and any deep dives.
- **No pronouns when the antecedent is ambiguous.** This bites hardest in finance docs where "the asset", "the position", "the loan", and "the price" can all be the referent. Re-read every "it", "this", and "that" and replace with the actual noun if there is any chance of confusion.

### Document structure

- **Integrate per-instruction reference into the lifecycle prose.** Do not keep a separate "instruction reference" section that lists handlers in isolation. Walk through the program's flow and inline each handler's full mechanics (signers, accounts, token movements, errors) at the point where the handler is first called. Branching scenarios attach as alternative paths off the main walkthrough.
- **No section numbers in headings.** No `## 1. Overview`, no `### 3.6 Liquidation`. Headings are words only. Numbered headings break the moment a section is inserted; word-based anchors stay stable.
- **Cross-references use named anchors with words, never section numbers or `§`.** Use heading words as the link text so the link reads as part of the sentence: `[safety and edge cases](#safety-and-edge-cases)`.
- **Do not preview the README in the README.** No "the sections below cover X, Y, and Z". The table of contents and headings already do this.
- **Do not redefine already-defined terms.** If "holder", "short seller", or "keeper" were introduced in the lede, do not add a "Roles" or "Glossary" subsection that defines them again.
- **Do not tell readers to skip sections.** No "if you already know X, skip to Y". Background sections are useful for everyone; let readers decide what to read.
- **Use concrete examples in two-asset program intros.** When a program involves two distinct assets (collateral vs. borrowed, base vs. quote), name them concretely, for example "post USDC as collateral, borrow xNVDA". Abstract "different mints A and B" reads as circular because the asymmetry is invisible.

### Prose hygiene

- **No ASCII art, no Mermaid, no markdown tables.** Use headings, nested bullet lists, or short prose. ASCII art looks bad on every surface, Mermaid is unnecessary, and markdown tables do not render on chat surfaces and are usually overkill in prose docs anyway.
- **No em-dashes anywhere.** Use a regular dash. Em-dashes are LLM-output tells. Applies to the README, code comments, commit messages, and doc strings.
- **Do not say "worked example" or "worked scenario".** Just "Example", "Scenario", or "Walkthrough". The "worked" qualifier is textbook pretension.

### Accuracy claims

- **Do not claim "non-custodial" if the program holds funds in vaults.** "Non-custodial" in common usage suggests funds never leave the user's wallet, which contradicts every escrow, lending, and AMM program. The narrow DeFi sense is "no admin key, no off-program logic, rules are the deployed bytecode". Say that directly and describe the actual custody arrangement: vaults, PDA signers, no admin override.
- **Upgrade authority is normal.** Solana programs are usually upgradable so authors can ship security fixes. Do not treat upgrade authority as disqualifying for "trustless" claims. Trust in the author or multisig is baseline; describe who holds it.
- **Do not claim things are TradFi-only when they exist onchain too.** ETFs, stablecoins, and similar all exist onchain. Do not write "X (a TradFi thing; passive holders onchain)" framings.

## TypeScript Guidelines

These guidelines apply to TypeScript unit tests, browser code, and any other places where TypeScript is used in the project.

### General TypeScript

Use `type: module` in `package.json` files.

Avoid using a `tsconfig.json` unless it's needed, as we use `tsx` to run most typescript and it doesn't usually need one. If you do need a `tsconfig.json`, state why at the top of the file, and you can use the most modern version of ECMAScript/JavaScript you want - up to say 2023.

### Async/await

Favor `async`/`await` and `try/catch` over `.then()` or `.catch()` or using callbacks for flow control. `tsx` has top level `await` so you don't need to wrap top level `await` in IIFEs.

### Type System

- **Always use `Array<item>`**, never use `item[]` for consistency with other generic syntax like `Promise<T>`, `Map<K, V>`, and `Set<T>`
- **Don't use `any`**

### Comments

- Most comments should use `//` and be above (not beside) the code
- The only exception is JSDoc/TSDoc comments which MUST use `/* */` syntax

### Solana-Specific TypeScript

- Don't make new `@solana/web3.js` version 1 code. Do not make new code using `@coral-xyz/anchor` package. Don't replace Solana Kit with web3.js version 1 code. web3.js version 1 is legacy and should be eventually removed. Solana Kit used to be called web3.js version 2. Use Solana Kit, preferably via Solana Kite.
- Use Kite's `connection.getPDAAndBump()` to turn seeds into PDAs and bumps
- There is no need to use offsets that you set to decode Solana account data - either download an npm package for the program like `@solana-program/token` for the token program or make one using Codama.
- In Solana Kit, you make instructions by making TS clients from IDLs using Codama. You can easily make Codama clients for installed IDLs using:

`npx create-codama-clients`

- Do not use the `bs58` npm package.

Don't do this:

```typescript
import bs58 from "bs58";
const signature = bs58.encode(signatureBytes);
```

Do this instead:

```typescript
import { getBase58Decoder } from "@solana/codecs";
const signature = getBase58Decoder().decode(signatureBytes);
```

Yes, `bs58` and `@solana/codecs` packages have different concepts of 'encode' and 'decode'.

### Unit Tests

- Create unit tests in TS in the `tests` directory
- Use the Node.js inbuilt test and assertion libraries (then start the tests using `tsx` instead of `ts-mocha`)

**Unit testing imports:**

```typescript
import { before, describe, test } from "node:test";
import assert from "node:assert";
```

- Use `test` rather than `it`

### Thrown object handling

- JavaScript allows arbitrary items - strings, array, numbers etc to be 'thrown'. However you can assume that any non-Error item that is thrown is an programmer error. Handle it like this (including the comment since most TypeScript developers don't know this):

```ts
// In JS it's possible to throw *anything*. A sensible programmer
// will only throw Errors but we must still check to satisfy
// TypeScript (and flag any craziness)
const ensureError = function (thrownObject: unknown): Error {
  if (thrownObject instanceof Error) {
    return thrownObject;
  }
  return new Error(`Non-Error thrown: ${String(thrownObject)}`);
};
```

and

```ts
try {
  // some code that might throw
} catch (thrownObject) {
  const error = ensureError(thrownObject);
  throw error;
}
```

## Rust Guidelines (Anchor Programs)

### Anchor Version

- Write all code like the latest stable Anchor (currently 0.32.1 but there may be a newer version by the time you read this)
- Do not use unnecessary macros that are not needed in the latest stable Anchor

### Anchor has silly defaults

Every project will need an IDL.

```toml
[features]
idl-build = ["anchor-lang/idl-build", "anchor-spl/idl-build"]
```

and if it uses SPL Tokens (like almost every Anchor project) it will need this dependency (insert whatever version is applicable):

```toml
[dependencies]
anchor-spl = "0.32.1"
```

### Project Structure

- **Never modify the program ID** in `lib.rs` or `Anchor.toml` when making changes
- Create files inside the `state` folder for whatever state is needed
- Create files inside the `instructions` or `handlers` folders (whichever exists) for whatever instruction handlers are needed
- Put Account Constraints in instruction files, but ensure the names end with `AccountConstraints` rather than just naming them the same thing as the function
- Handlers that are only for the admin should be in a new folder called `admin` inside whichever parent folder exists (`instructions/admin/` or `handlers/admin/`)

### Account Constraints

- Use a newline after each key in the account constraints struct, so the macro and the matching key/value have some space from other macros and their matching key/value

### Bumps

- Use `context.bumps.foo` not `context.bumps.get("foo").unwrap()` - the latter is outdated

### Data Structures

- When making structs ensure strings and Vectors have a `max_len` attribute
- Vectors have two numbers for `max_len`: the first is the max length of the vector, the second is the max length of the items in the vector

### Space Calculation (CRITICAL - NO MAGIC NUMBERS)

- **Do not use magic numbers anywhere**. I don't want to see `8 + 32` or whatever.
- **Do not make constants for the sizes of various data structures**
- For `space`, use syntax like: `space = SomeStruct::DISCRIMINATOR.len() + SomeStruct::INIT_SPACE,`
- All structs should have `#[derive(InitSpace)]` added to them, to get the `INIT_SPACE` trait
- **DO NOT use magic numbers**

**Example:**

```rust
#[derive(InitSpace)]
#[account]
pub struct UserProfile {
    pub authority: Pubkey,

    #[max_len(50)]
    pub username: String,

    pub bump: u8,
}

#[derive(Accounts)]
pub struct InitializeProfile<'info> {
    #[account(
        init,
        payer = authority,
        space = UserProfile::DISCRIMINATOR.len() + UserProfile::INIT_SPACE,
        seeds = [b"profile", authority.key().as_ref()],
        bump
    )]
    pub profile: Account<'info, UserProfile>,

    #[account(mut)]
    pub authority: Signer<'info>,

    pub system_program: Program<'info, System>,
}
```

### Error Handling

- Return useful error messages
- Write code to handle common errors like insufficient funds, bad values for parameters, and other obvious situations

### PDA Management

- Add `pub bump: u8` to every struct stored in PDA
- Save the bumps inside each when the struct inside the PDA is created

### System Functions

- When you get the time via Clock, use `Clock::get()?;` rather than `anchor_lang::solana_program::clock`

## Git commits

Do not add "Co-Authored-By: Claude" or similar attribution when creating git commits.

## Acknowledgment

- Acknowledge these guidelines have been applied when working on this project to indicate you have read these rules and found that they do apply to this project.
