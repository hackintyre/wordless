# Wordless 2.1.0

Offline desktop writing studio — source and automated Windows build kit.
**This archive does not contain a precompiled EXE.**

## Build and run
1. Extract the ZIP to a NEW local folder on Windows 10/11 x64 (Intel/AMD).
2. Double-click `BUILD_WORDLESS_EXE.bat`. Keep internet connected for dependency/build-tool downloads.
3. Wait for SUCCESS. Run the newly produced `Wordless.exe` beside the batch file.
4. Test on copies of documents, then disconnect networking and test again.

## What's new in 2.1.0

### Local version history
File → Version history opens timestamped snapshots with a formatted, read-only preview and Restore selected. Snapshot now makes a manual version. Automatic versions are taken after idle recovery saves, at most once per minute, and before save, replacing text, opening/new-document operations with dirty content, and history restore.

Defaults: 30 days, 50 snapshots per document, 64 MB total history. The panel exposes days (1–365), snapshots/document (2–500) and total storage MB (8–512), plus current usage. Applying smaller limits prunes oldest/expired entries; pruning is irreversible. Limits apply to the combined history JSON, not to user documents, recovery files or sibling backups. An oversized individual snapshot is rejected explicitly. A corrupt history file fails closed rather than being silently replaced.

History is stored locally under the app's user-data directory. Saved documents are associated with their local paths; first save migrates the draft history, and Save As copies saved-document history while keeping the original's versions. Renaming a document outside the app does not automatically migrate its path association. Unsaved draft identity is persisted between launches, but creating a new draft starts a new identity. Restoring a version preserves the active file association, snapshots the current content first, and leaves restored content UNSAVED until you save it. Snapshot restore is not guaranteed to be a native Undo action; use history to return to the pre-restore version.

### Find and Replace
The persistent Find panel has up/down navigation, wraparound, match count, case-sensitive and Unicode-aware whole-word options, and optional highlight-all using Chromium's CSS Highlight API. Highlight markup is not inserted into documents. Enter/F3 goes forward; Shift+Enter/Shift+F3 goes backward.

Preview replace and Preview replace all show the count and context (up to 50 preview lines). Applying requires an unchanged document revision, first makes a safety snapshot and commits the group with one native insertHTML edit transaction. Unchanged formatting and explicit page breaks are carried through a cloned DOM; inserted replacement text uses the browser's surrounding formatting. Cross-style/cross-paragraph replacements can necessarily merge or alter the replaced structure. Use the preview, native Undo and safety snapshot; actual browser undo grouping and complex rich-text edge cases still require Windows acceptance testing. Replacement is literal, not regex.

### Safer document I/O
Before any overwrite, the exact previous file is copied to a timestamped sibling named `filename.wordless-backup-...`. If backup fails, overwrite is aborted. These backups are intentionally NOT automatically deleted or counted against the history cap; review disk usage and manage them separately. File writes still use the existing temporary-file/rename path. Backups and snapshots are not substitutes for external backups and do not guarantee protection against drive failure.

DOCX import warns about approximation, omitted complex layout/images/headers/footers and source preservation. Native sanitization changes and plain-text limitations are explained. Saving DOCX/TXT still warns about conversion loss. Native `.wordless` is recommended for supported formatting, page breaks and bookmark metadata. DOCX conversion remains approximate; automatic Word pagination and complex layouts are not promised.

### Focus / typewriter mode
The bottom-right toggle, beside the page/view controls, hides header/ribbons/sidebar while leaving the paper's dimensions and margins unchanged. The caret is kept near 45% of the visible writing area during typing/navigation. Toggle it off to return to the full interface. This is a viewport mode, not a document-formatting change.

### Session goals and local statistics
Opening or creating a document presents an optional goal/timer question. Choose Set a goal / timer to expand the sidebar panel, or Not now to collapse it. Start/reset begins a session with an optional words-to-add target. The timer pauses when the document workspace is hidden or the app is minimized. The panel shows elapsed visible seconds, net words added and goal completion; lifetime totals are local only.

Statistics are checkpointed every 15 seconds with an idempotent session ID. Document switches and session reset also flush them. A crash/forced exit may lose the last checkpoint interval; shutdown IPC is best effort. Word counts are net additions, not a keystroke counter, and edits/deletions are not a detailed analytics history. Session settings/progress are not resumed after restarting the app. The optional launch prompt applies to New/Open, not to simply returning from the landing page to the same active document.

### Outline, bookmarks and collapsible sections
Headings 1–4 are indented by level; each has a navigation button and expand/collapse arrow. Collapsing hides following sibling blocks through the next equal/higher-level heading and hides descendant outline rows. This view-only state is removed from snapshots/exports and expanded for print. Counts include collapsed text; Find reveals folded content. Section collapse is intended for top-level heading blocks; deeply nested imported headings do not collapse arbitrary wrappers.

Bookmark selection attaches a named bookmark to the containing top-level block. The bookmark list can navigate/remove it. Bookmarks persist in native files and snapshots; TXT and DOCX are not promised to retain custom bookmark metadata. This is block-level bookmarking, not multiple arbitrary range bookmarks within one paragraph.

## Preserved features
The text-only logo still opens the landing page without clearing the active document. Selected-text colour application and reset remain. Autumn retains shaded oval/pointed/maple sprites, smooth perspective flutter and bottom-to-top lifecycle. Stars remain stationary, visibly glimmer on 8–10 second cycles with gradient/flares, and retain occasional 0.22–0.34 second side-sky shooting stars at random 22–55 second intervals. No remote animation/font assets or services were added.

## Privacy, validation and limitations
History, recovery, bookmarks and statistics are unencrypted local data. User-data files remain after deleting the portable EXE. Files stored in a cloud-synced directory are outside the app's network restrictions. Renderer CSP denies connections and the Electron local-protocol allowlist remains; no new external service is used. A certified air-gap claim requires real Windows network monitoring.

The corrected build kit passed 53 automated tests with zero failures and zero skipped tests, including all three real DOCX integration tests, using the pinned converter packages. Syntax checks and electron-builder configuration-schema validation passed. Tests cover real filesystem history/pruning/backups, session checkpoints, search matching, fixtures and mock-DOM selection/find behaviour. No Windows/Electron GUI, EXE build, PowerShell execution, icon resource stamping, actual replacement undo, native picker or performance validation ran here. Real DOCX converter integration did run. Packaged-source verification was exercised with an ASAR and synthetic PE fixtures, NOT an actual Windows build. The Windows build installs required converters and runs the integration tests. See `validation.txt`, `TESTING.md`, and the included fixtures. Keep original documents and validate the Windows build before relying on it for important work.

## Corrected 2.1.0 build kit
A release-blocking DOCX issue was reproduced after installing the pinned conversion dependencies: html-to-docx 1.8.0 drops semantic `<em>` italic styling, so the formatted fixture test failed and the build stopped before creating dist. The export bridge now converts equivalent `<em>` tags to `<i>` only for DOCX conversion. The test was NOT removed or weakened. A further integration test covers semantic, literal, nested and uppercase italic tags.

The exact cause on an individual PC still requires its build log; this was a confirmed reproducible blocker in the old kit. The pinned Node ZIP checksum was rechecked against nodejs.org's official SHA-256 manifest.

The batch filename and bytes remain unchanged. The PowerShell pipeline now records raw child-command output to Wordless-build-steps.log, identifies the failing stage in Wordless-build-status.json and requires DOCX tests rather than allowing them to skip. After packaging it validates the portable launcher PE, x64 application PE, app version and packaged source equality before copying the verified portable EXE beside the batch file. NSIS portable launchers may be x86 even with an x64 app payload; this is explicitly supported by the validator. The Windows CI workflow runs the same checked pipeline.

All 11 other runtime files are byte-identical to the previous 2.1.0 source. Version history, Find/Replace, focus/typewriter, session goals/statistics, outline/bookmarks, offline controls and animation code were not removed or rewritten. See FEATURE_PRESERVATION.json for the source comparison.

Extract the corrected ZIP into a NEW local folder, then run BUILD_WORDLESS_EXE.bat. A successful build should produce dist/Wordless.exe, dist/win-unpacked/Wordless.exe, dist/Wordless-build-verification.json, Wordless.exe beside the batch file and Wordless.exe.sha256. The source ZIP deliberately contains no fabricated dist folder or precompiled EXE. If it still fails, provide both Wordless-build.log and Wordless-build-steps.log plus Wordless-build-status.json; do not disable security policy or run an old EXE as this release.
