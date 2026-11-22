---
name: web-performance-audit
description: Audit mobile websites, PWAs, and WebViews for slow chat, memory spikes during media import, growing storage, repeated database reads, scroll jank, and polling that survives navigation. Trace a user action to code, report evidence, and implement and retest fixes when requested. 适用于小手机卡顿、批量导图闪退、存储膨胀、动画掉帧和幽灵计时器排查。
license: "Custom personal noncommercial license; see LICENSE"
---

# 把性能吐出来！

*Give Me My Frames Back!*

English · [简体中文](SKILL.zh-CN.md)

Original author: **niannian**. Personal noncommercial use and redistribution with attribution only; no third-party platform integration. See [LICENSE](LICENSE).

Original source: [niannian0125/web-performance-audit](https://github.com/niannian0125/web-performance-audit).

Start with an action the user can reproduce. Identify the first change worth making, the work it would eliminate, and a test that would show the difference. Write the report in the user's language.

## Trace the symptom

1. Read the project notes. Find the action's entry point, the data it touches, and where the view or service starts and stops. Follow that path for a specific symptom. For a project-wide audit, cover the applicable checks below and group findings with the same cause. State the coverage when only a log or snippet is available.
2. Pick a metric that captures the extra work. Count rows read per incoming message, requests per minute after leaving a page, or decoded bitmaps held during an import. Collect a baseline with existing tooling and test data. Record the build, device or emulator, data volume, and steps. Use source evidence for code behavior and measurements for timing and memory. Establish the cause of a crash separately.
3. For an audit, deliver findings and retest steps. If the user requests a fix, make the smallest effective change within scope and verify it through the original entry point. Preserve saved media, chat history, and features intended to run across views. Obtain explicit authorization before deleting real user data or running an irreversible migration.

## Eight checks

| Area | What to trace | What to verify |
| --- | --- | --- |
| 1. Image and audio storage | Follow writes through reads, user-selected retention, and expiry. Identify disposable caches, saved originals, sole copies, and attachments shared across records. | Measure growth and bytes recovered. Reopen saved items and shared attachments after cleanup. |
| 2. Image compression | Follow the paths for displaying a thumbnail and opening the original. Compare encoded bytes, source dimensions, display size, and decode time. Choose lossy or lossless encoding with photo detail, sharp text, alpha edges, and animation in mind. | Inspect quality at the target size. Measure bytes read and time to display. Measure decode time and memory separately when changing the codec at the same dimensions. |
| 3. Database reads | Inspect query ranges, indexes, ordering, limits, and call counts for lists, search, and incoming messages. Look for filtering or slicing after full-store reads. Identify intentional full exports separately. | Count rows returned versus rows shown. Check for unnecessary attachment reads. Test pagination with tied timestamps and multiple conversations for gaps, duplicates, and cross-chat results. |
| 4. Upload limits | Follow file selection, paste, and drop through preview, decode, and storage. Check file count, individual and total bytes, pixel count, and audio duration as soon as each value is available. | Verify that oversized input is caught before expensive work where feasible. Check the receiving endpoint when one exists. Base limits on product requirements and target-device tests. |
| 5. Rendered chat history | Count records read, messages held in memory, and mounted message elements separately. Inspect pagination or windowing, its buffer, and access to older messages. | Append messages and track mounted elements. Check CSS-hidden rows, scroll position when an image expands, scrolling back, and jumping to an old message. |
| 6. Lazy loading and concurrency | Trace startup imports, feature initialization, and batch queues. Distinguish persistent storage quota from runtime memory. Defer unused features and size the work queue around the cost of decoding and processing each item. | Count in-flight tasks and live large objects. Look for result arrays that retain a whole batch. Test resource release and item ordering on completion, cancellation, failure, and retry. |
| 7. Visual effects | Replay the same scroll or animation on the same device. Toggle backdrop blur, overlays, animation, and visible large images one at a time. | Compare frame times and slow frames. In an Android host, verify any refresh-rate request with real frame delivery. A higher target frame rate gives each frame less time. |
| 8. Background work | Identify who owns each timer, poll, listener, observer, and animation loop. Follow hiding, unmounting, re-entry, and pending async completions through cleanup or re-registration. | Count tasks and requests after repeated navigation. Page-level work should end with its owner. Verify that music or focus timers intended to survive navigation stop at the appropriate user action. |

For measurements or suspected false positives, pick the matching [retest scenario](references/scenarios.md).

## Report the result

Lead with the highest-priority finding. For each finding, give the trigger, file and function or line, evidence, impact, proposed change, and retest. Specify whether it affects startup, storage, peak memory, or frame time. Prioritize established crash and data-loss risks and stalls on frequently used paths.

Distinguish behavior confirmed in code, behavior reproduced at runtime, and hypotheses that still need a test. Give the conditions for each measurement. Without a device, use counters or existing tests to verify changes in workload and identify outstanding device tests. After a fix, compare the same operation before and after and check the surrounding behavior. If the audit yields no findings, report which paths and conditions it covered.
