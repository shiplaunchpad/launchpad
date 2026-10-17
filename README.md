# launchpad

**Ship the app on your laptop: live, secure and measured.**

launchpad is a Claude Code plugin. Point it at a repository and it takes the app through a production checklist
using industry standards: builds in the cloud, signing, a release channel, keys in your vault, crash reports,
analytics, uptime alerts and the store paperwork. Mission Control, a dashboard on your own machine, shows every
project, its progress and its next step.

- **One payment.** $49 once for the first 1,000 users, then $129. No subscription.
  All 1.x updates are included; 2.0 is a paid upgrade, discounted for owners.
- **Runs on your Claude subscription.** Onboarding a project usually fits well under one 5-hour session limit.
- **Runs on your machine.** Your code and your keys stay with you. launchpad asks before anything that costs money,
  changes DNS or uploads to a store.

## What it works on

<!-- launchpad:platforms:start — generated from the platform registry; `npm run readme` rewrites it -->
launchpad ships six kinds of app — iOS app, Android app, macOS app (DMG), Web app, Static site, Backend service — built with any of these:

| Built with | Surface | Where it ships |
|---|---|---|
| Flutter | iOS app + Android app | TestFlight and the App Store for iOS; Firebase App Distribution for Android (a Play upload is not wired — the release build is yours to upload). |
| Swift / Xcode (iOS) | iOS app | TestFlight on a push to your test branch, the App Store on a tag. |
| Swift / Xcode (macOS) | macOS app (DMG) | A signed, notarized, stapled DMG on your own download URL, with Sparkle in-app auto-update. |
| Next.js, Nuxt, SvelteKit, Astro, React Router, TanStack Start, Remix, Angular, Qwik, SolidStart, Gatsby, Vite, Create React App | Web app | Vercel |
| Hugo, Jekyll, Eleventy, MkDocs, Zola, Astro (static), Docusaurus, Hand-written HTML | Static site | Cloudflare Pages |
| Android (Kotlin / Java) | Android app | Firebase App Distribution on a push, and the AAB Play wants kept on every build. |
| React Native | iOS app + Android app | TestFlight and the App Store for iOS; Firebase App Distribution for Android. |
| Expo | iOS app + Android app | TestFlight and the App Store for iOS; Firebase App Distribution for Android — or, on EAS Build, EAS's install links and its submission to both stores. |
| Cloudflare Worker | Backend service | Cloudflare Workers — a deploy on every push, staging as its own Worker, and its own cron triggers and queues. |
| Hono, itty-router | Backend service | Cloudflare Workers or Google Cloud Run |
| Express, Fastify, NestJS, Koa, hapi, Go HTTP server, Python web service, Containerised service | Backend service | Google Cloud Run |

Where a stack has a limit, `launchpad score --detail` says so for your app.

**Recognised, with no pipeline yet.** launchpad scores these and shows them on Mission Control, and says that it has no pipeline to write: Tauri desktop app, Electron desktop app, Swift package, Ruby on Rails app, Django app, WordPress site, Laravel app, Phoenix app, Elixir project, Rust project, Go project, Python package, Ruby gem, .NET project, JVM project, Deno app, Browser extension, Node CLI, Node package.

**Not supported yet:** Capacitor / Ionic.
<!-- launchpad:platforms:end -->

A repository can hold several of these; launchpad finds each app in it. Every pipeline is a GitHub Actions workflow,
so the repository has to be on GitHub (a mirror is enough).

## Install

Inside Claude Code:

```
/plugin marketplace add shiplaunchpad/launchpad
/plugin install launchpad@launchpad
```

Or from a terminal:

```
claude plugin marketplace add shiplaunchpad/launchpad
claude plugin install launchpad@launchpad
```

Installing costs nothing, and nothing runs until you
activate a key. Restart Claude Code and the `/launchpad:` commands are there.

Claude Code warns you to trust a plugin before you install it. launchpad is published by its author on GitHub at
`shiplaunchpad/launchpad`. After you activate, everything it runs is in plain files on your machine (see
[What is on your machine](#what-is-on-your-machine)), so you can read every one of them.

## Activate and start

Buy a key on [shiplaunchpad.com](https://shiplaunchpad.com). It arrives in your receipt email. Then, in Claude Code:

```
/launchpad:license <your key>
```

You see `Unlocked: launchpad <version> pack installed.` Open Claude Code in the repository you want to ship and run:

```
/launchpad:onboard
```

launchpad reads the repository, asks the few questions it cannot answer itself, and tells you what it will write
before it writes anything. To look first and change nothing, run `/launchpad:status`.

A key works on two machines. `/launchpad:license deactivate` frees its seat on a machine you no longer use. A key
that has been checked once keeps working offline.

## Updating

Mission Control shows a banner when a new version is out. You click **Update** to install it, then restart Claude
Code. From a terminal it is two commands:

```
claude plugin marketplace update launchpad
claude plugin update launchpad@launchpad
```

launchpad never updates itself, and `launchpad update` tells you whether a release fixes something important.

## Mission Control

```
/launchpad:dashboard
```

It serves on `http://127.0.0.1:4747`, on your machine only. It shows every project, its progress, its next step,
and why launchpad made each choice it made.

## What leaves your machine

launchpad sends three things to us or our payment provider (the landing page and the privacy page print the same
list):

1. **An update check**, at most once a day and whenever you ask. It reads the public list of releases on our site
   and sends nothing about you.
2. **A licence check**, when you activate your key and at most once a day after that: your key goes to Lemon Squeezy
   to confirm it. Downloading launchpad itself, once per version, sends your key, this machine's activation id and
   the version to our licence server.
3. **launchpad's own usage analytics**, only if you say yes. Onboarding asks once and shows exactly what would be
   sent: the launchpad version, your OS, and which stacks and checklist items it found. It never contains code,
   names, paths or keys. `launchpad telemetry off` turns it off.

What launchpad sends to your own accounts when you ask it to act (GitHub, your host, your stores) is your data going
to your accounts.

## What is on your machine

The plugin you install is a small loader. launchpad itself arrives as a signed pack in
`~/.launchpad/pack/<version>/`, readable only by you: plain JavaScript, the skills and the templates. launchpad
checks the pack's signature before every run and refuses to run an edited file. An update replaces the pack.

In each repository it writes `.launchpad/` (its state, checked in so it travels with the code), a marked block in
your `CLAUDE.md` (the rest of the file stays as it is), and its workflows in `.github/workflows/launchpad-*.yml`. It
lists every file it is about to change before it changes it, and it never overwrites a file you edited.
`launchpad undo` puts back the files it wrote in its last run. Your keys go to your OS keychain, or to an encrypted
file where there is no keychain. They never go to the repository or a `.env` file.

## Windows, Linux and proxies

- **Windows and Linux.** Keys go into an encrypted file, and launchpad needs a passphrase in the environment Claude
  Code starts from: `LAUNCHPAD_VAULT_PASSPHRASE`. Keep it in your password manager; it cannot be recovered. iOS and
  Mac apps build on GitHub's macOS machines.
- **A corporate proxy.** Set `HTTPS_PROXY` and `NODE_USE_ENV_PROXY=1` before you start Claude Code.

## Support

<!-- launchpad:support:start — generated from product.ts; `npm run readme` rewrites it -->
Bugs, questions and license problems: **[support@shiplaunchpad.com](mailto:support@shiplaunchpad.com?subject=launchpad%20support)**.
Attach what `launchpad report` prints. It is safe to send, and it answers most questions before they are asked.

**Sales and support.** Sales are final. If anything goes wrong, write to support@shiplaunchpad.com with the report launchpad prints. Support works through the problem with you, fixes what launchpad got wrong, and can free the seat on a machine you no longer use.
<!-- launchpad:support:end -->

If launchpad broke a working deploy, say so in the subject. We answer those messages first. Never send your whole
licence key: the last four characters are enough to find a purchase.
