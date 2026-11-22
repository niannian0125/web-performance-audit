# 把性能吐出来！

*Give Me My Frames Back!*

English · [简体中文](README.zh-CN.md)

A performance audit skill for mobile websites, PWAs, and WebViews.

By **niannian** · Skill ID: `web-performance-audit`

Original source: [niannian0125/web-performance-audit](https://github.com/niannian0125/web-performance-audit)

Custom license: personal, noncommercial use; sharing and adaptations with attribution; no third-party platform integration. See [License](#license).

Does your chat app feel fast on the first visit and slow after switching conversations a few times? Can a batch of small photo files crash the page? This skill helps a coding assistant trace one of those symptoms from a user action to the code responsible, then show you how to test the fix.

It grew out of building a mobile-first chat PWA. Its eight checks cover image and audio storage, image compression, database reads, upload limits, mounted chat messages, lazy loading and concurrency, visual effects, and background work that survives navigation.

## Install in a project

Download the source, then copy the entire [web-performance-audit](web-performance-audit/) folder into the project you want to inspect. Use the destination for your coding assistant.

| Assistant | Destination folder | How to invoke it |
| --- | --- | --- |
| Codex | `.agents/skills/web-performance-audit/` | `$web-performance-audit` |
| Claude Code | `.claude/skills/web-performance-audit/` | `/web-performance-audit` |

Open your project in the assistant and describe the symptom. If your assistant can read project files but does not discover skills automatically, ask it to read [SKILL.md](web-performance-audit/SKILL.md) and give it the path to the relevant code. The skill follows the language of your request.

## Try it

> Use web-performance-audit to check this project before I change anything. What should I fix first, which user action triggers it, where is the evidence in the code, and how can I retest it?

> Switching conversations makes chat slower. Check whether old views keep polling. How many database rows does each refresh read to show the latest 20 messages?

> Fix the memory spike during photo import. Preserve selection order and the option to keep originals. Compare the number of live bitmaps before and after using the same test images.

## What a finding looks like

A chat view clears its timer on exit, yet the old conversation keeps polling. The clue is a database read that started before navigation. When the read completes, its callback schedules another timer. To verify, delay a read, switch conversations, then let the read finish and count requests from the old view.

An audit ties each finding to a code location, a user-visible impact, and a retest. It distinguishes behavior confirmed in code from a measured result or an open hypothesis. Ask for an audit to get findings, or request a fix to have the assistant change and retest the relevant code. The [five retest scenarios](web-performance-audit/references/scenarios.md) cover long chats, photo imports, cache cleanup, lingering requests, and slow scrolling.

Installation references for [Codex](https://developers.openai.com/codex/skills/) and [Claude Code](https://code.claude.com/docs/en/skills).

## License

Copyright © 2026 niannian. Released under the custom [Personal Noncommercial Use and Redistribution License 1.0](LICENSE), with a copy inside the skill folder for standalone installation.

Individuals may use, copy, share, modify, and republish the skill for personal, noncommercial purposes. Preserve the credit to **niannian**, the original title **把性能吐出来！**, the supplied source links, and the complete license. Clearly describe modifications and distinguish your version from the original. You may add your own contributor credit.

Commercial use and embedding, preinstalling, bundling, or hosting the skill as a feature or built-in resource of a third-party platform are prohibited, whether the platform is paid or free. Installing it into Codex or Claude Code as an individual end user is allowed. A standalone, noncommercial source repository or file release with attribution is allowed; providing only a source link is also allowed.

An attribution example for a modified version:

> Original work: 把性能吐出来！ (`web-performance-audit`), by niannian.
>
> License: Personal Noncommercial Use and Redistribution License 1.0; see the included LICENSE.
>
> Changes: Updated the photo-import retest steps.

Retain any upstream source links supplied with the version you received. This is a custom restricted license, not MIT, Apache, or a standard Creative Commons license. The full [LICENSE](LICENSE) governs; its Chinese text controls if the two language versions differ.
