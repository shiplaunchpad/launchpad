---
name: license
description: Licensing and provenance — activate a launchpad license, check it, release a machine, and report which build is running. Use when someone asks about buying, activating, "needs a license", an activation limit, or which version of launchpad this is.
---

# launchpad: license

<!-- launchpad:standing-rules -->
## Standing rules — identical in every launchpad skill

1. **Never print a secret value.** Finding a credential — in a `.env`, a key
   file, a keystore, a token in a log or a CI variable — you name the **file**
   and the **key name** and stop. Never the value, never part of one, never a
   prefix, a suffix, a length or a hash, and not "just to show the problem".
   Do not open a `.env` at all: the key names are in `.env.example`, and the
   strongest guarantee is not looking. `launchpad report` is built so a paste is
   safe in public, and a transcript quoting the value throws that away. An
   exposed key is fixed by rotating it, never by displaying it. **Never ask
   for a value in chat either:** a key goes in through Mission Control's key
   field or `launchpad secret set <name>` in the user's own terminal.
2. **"It's broken" / "it's not working" / "help" → `report` first.** Run
   `node "${CLAUDE_PLUGIN_ROOT}/dist/launchpad.mjs" report`, or ask the user to
   run it and paste the output, before any diagnostic question. One paste
   replaces ten questions; it needs no network and is redacted, so it
   is safe to share. One clarifying question alongside it is fine; a list of
   questions instead of it is not.
3. **We decide; they are not asked to choose.** The CI provider, the signing
   model, the bundler, the runner and the trigger shape are launchpad's calls —
   not having to make them is what was bought. Stop only for money, a secret,
   something that already works, or an act that is irreversible or public.
4. **Read the detected state before promising anything.** In a repository
   launchpad already set up (`.launchpad/state.yml` exists), read
   `.launchpad/STATUS.md` first — what is live, what is left, what the owner
   answered — before you change or answer anything. A surface's
   `archetype` and `framework` decide whether a pipeline exists at all
   (`launchpad detect` prints both as JSON and writes nothing;
   `.launchpad/state.yml` holds them after `setup`). Where launchpad refuses a
   surface, relay that refusal — what was found, why there is no pipeline, and
   what the user still gets — rather than paraphrasing it, planning around it,
   or hand-writing the pipeline launchpad declined to write.
5. **Too unconventional for the documented path → stop, and hand it over.**
   When the project does not fit what these skills document — a custom build
   system, bespoke signing, a monorepo laid out like nothing here, a host or
   stack launchpad does not know, anything where the next step would be a
   guess — do not improvise config, workflows or scripts to get past it. Stop,
   and record the step as a manual item:
   `node "${CLAUDE_PLUGIN_ROOT}/dist/launchpad.mjs" manual add "<the step>" --why "<what did not fit>" --steps "<one>; <two>" --link <https page where it is done>`
   (add `--item <checklist id>` when it stands in for one). Tell the user what
   you found and why you stopped. It is on their list — `launchpad needs`,
   Mission Control and `.launchpad/STATUS.md` — as needing them, never as
   done, until they run `launchpad manual done <id>`. A guess in someone's
   release path is how a working app stops working; an honest hand-off is not.
   **A step handed over because of something launchpad itself reported** (a
   warning `apply` printed) names that report — add `--report <id>` (`apply`
   prints the id, e.g. `apply:missing-fastlane-lanes`, with `--report-subject`
   and `--report-names`): it closes by itself once launchpad stops reporting it.
   **A suspected launchpad bug is never the owner's step.** When what stops you
   looks like launchpad being wrong (its check contradicts the files), do not
   hand it over: run `report`, tell the owner in one line that launchpad will be
   fixed, and raise it for this session with `ask add` only if a choice is
   theirs. Never a step that says "send `launchpad report`" or "once launchpad's
   check is fixed" — that is launchpad's chore, not theirs.
<!-- /launchpad:standing-rules -->

## Activating a key needs nothing else

These run from the plugin itself, with or without a key — activating is what installs the rest:

```bash
node "${CLAUDE_PLUGIN_ROOT}/dist/launchpad.mjs" license <key>        # activate this machine and install launchpad
node "${CLAUDE_PLUGIN_ROOT}/dist/launchpad.mjs" license              # what is on this machine; re-check the key
node "${CLAUDE_PLUGIN_ROOT}/dist/launchpad.mjs" license deactivate   # release this machine's seat
node "${CLAUDE_PLUGIN_ROOT}/dist/launchpad.mjs" version              # which build is running, and its pack
```

When the user gives you a key, run the first one with it and relay what it prints. It answers "Unlocked: launchpad <version> pack installed." when it worked. For anything else about licensing, load the full instructions:

## Load the instructions

This skill's full instructions are part of launchpad itself, which is installed on this machine when a license key is activated. Load them now and follow them exactly as if they were written here:

```bash
node "${CLAUDE_PLUGIN_ROOT}/dist/launchpad.mjs" guide license
```

Run every launchpad command exactly as written — `node "<the path>" <verb>`, the path in double quotes, here and in the guide. Never put `node …` in a shell variable: `L="node …"; $L status` is one word in zsh and fails (exit 127). To shorten the path, `L="<the path>"; node "$L" <verb>`.

- **It prints the instructions (exit 0):** they ARE this skill, even if your shell adds an error status after them; nothing below applies. A long guide prints its opening and a numbered contents with each section's size: before acting, print every section the request touches — `guide license <number>`, several in one print `guide license <from>-<to>`, or `guide license <words from its title>` — and follow them as if written here. Looking for one thing (a word, a file, a setting)? `guide license --find "<words>"` prints the section that mentions it, whole, in one print — never page through the parts to search.
- **It says a license is needed (exit 2):** relay what it printed — what launchpad is, how to activate a key (the key is in the receipt email; `/launchpad:license <key>` answers "Unlocked: launchpad <version> pack installed."), and where to get one. Nothing of launchpad runs before that. Do not improvise these steps from general knowledge: a hand-written pipeline is one launchpad cannot keep working, and it is what the user would be buying.
- **It says the pack could not be fetched (exit 1):** relay that. Running `/launchpad:license` again when online fetches it with the stored key, without using another seat.
