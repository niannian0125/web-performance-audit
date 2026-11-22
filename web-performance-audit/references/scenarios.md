# Retest scenarios

English · [简体中文](scenarios.zh-CN.md)

Use a test database or a disposable account. Repeat each run with the same data, device, and build. Compare cold starts separately from warm starts. Adjust the example counts to fit the project.

## Chat slows down over time

1. Create two conversations. Give one a long history, several messages with identical timestamps, and images that load after the text.
2. Open that conversation and append one message. Count database queries, rows returned per query, and message elements mounted in the DOM. If the code reads the entire store, filters by conversation, then takes the last page, identify where it first narrows the result set. Check which index the query actually uses.
3. Inspect the range, limit, ordering, and page boundary. A call to `getAll()` can be bounded. If list rows pull in large attachments, check whether existing keys, indexes, or lightweight summaries can supply the fields the list needs.
4. Scroll back two pages. Let an image load and a new message arrive while reading an older message. Check that the reading position holds. Jump to the latest message, then find the older one again. At a page boundary with tied timestamps, check for gaps, duplicates, and messages from the other conversation.

## Importing a batch of photos crashes the page

1. Try a small encoded file with large pixel dimensions, a batch of ordinary photos, and a supported audio file. Record the file count, individual sizes, and total bytes. Apply inexpensive checks before full decoding, then bound concurrent metadata reads, thumbnail generation, and transcoding.
2. Trace when large objects are allocated and released during reading, decoding, resizing, encoding, and saving. A single 4096 × 4096 image at four bytes per pixel occupies about 64 MiB for the pixel buffer alone. Temporary copies, textures, and the decoder can use more. Record encoded bytes and measured processing time separately.
3. Cancel mid-import, trigger a failure on one file, and retry. Check completed and pending items, verify that previews still open, and check that temporary URLs and bitmaps are released after their last use. Even with bounded concurrency, inspect whether a result array retains every decoded image until the batch finishes.

## What can a user open after cache cleanup?

Seed the test store with an expired thumbnail that can be regenerated, an explicitly saved original, an audio attachment shared by two conversations, and a legacy attachment with no retention flag. Record object counts and bytes before and after cleanup, then reopen the app offline. Verify that saved items, history, and the shared attachment remain accessible and that thumbnails can be regenerated as the product promises. Preserve legacy records whose references or retention status are unclear. Repeat after interrupting cleanup and while a save is in progress.

## Requests continue after leaving a page

Open a chat, wait one polling interval, leave, and reopen it. Repeat five times. Tag starts and cleanups by page instance or conversation and count requests per interval. Track app-wide services separately. Delay one request until after the page closes, then let it finish. Check whether its callback schedules another timer or writes to the old view. If the framework caches pages, trace deactivation and reactivation. For cross-page audio, test continued playback after navigation and cleanup when the user presses Stop.

## Scrolling drops frames on a low-end phone

Use the same chat history, wallpaper, theme, and display refresh setting. Scroll the same distance on each run. Toggle background blur, overlays, and animations one at a time, then compare frame-time distributions or slow-frame counts. Compare the first scroll past an image with later passes to distinguish initial decode stalls from recurring repaint work. If only desktop emulation is available, record its settings and leave device-specific conclusions open until a phone is tested.
