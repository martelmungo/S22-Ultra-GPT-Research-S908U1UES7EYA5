# SM-S908U1 EYA5: research closeout

**Status: paused by the owner on 2026-11-15.** This repository is a documentation checkpoint, not a working ROM release or an unlock tool.

We investigated owner-controlled Android/LineageOS on the Snapdragon Galaxy S22 Ultra SM-S908U1 while retaining Samsung's authenticated early boot stages where possible. The target firmware was **S908U1UES7EYA5**, Android 14, bootloader revision 7. The research combined offline firmware analysis, filesystem validation, public-source comparison, and isolated Android builds.

The result is a substantially better map of the authorization boundaries and a real generic LineageOS 21 build environment. **No owner-controlled OS was installed or booted. No complete, repeatable unlock or installation route was demonstrated.** The U1-specific product and the generic ARM64 build must not be confused.

## Start here

| Document | Purpose |
|---|---|
| [Findings](FINDINGS.md) | What was learned, including achievements and limits |
| [Security research](SECURITY_RESEARCH.md) | All investigated security branches, their evidence and remaining boundaries |
| [ROM build and compatibility](ROM_BUILD_AND_COMPATIBILITY.md) | Exact build milestones, current failure and hardware/software dependencies |
| [Reusable methods and AI](REUSABLE_METHODS_AND_AI.md) | Lessons for other devices, engineering projects and AI-assisted research |
| [Corrections and negative results](CORRECTIONS_AND_NEGATIVE_RESULTS.md) | Superseded interpretations and searches that should not be repeated |
| [Open questions](OPEN_QUESTIONS.md) | Exact evidence required before reopening each branch |
| [Resume guide](RESUME.md) | Ordered, non-destructive restart checklist |
| [Evidence index](EVIDENCE_INDEX.md) | Source IDs, file identities and audit limits |
| [Publication notes](PUBLICATION_NOTES.md) | What this GitHub package includes and excludes |
| [Finding template](templates/FINDING_TEMPLATE.md) | A reusable evidence-record format |

Machine-readable records are [claims.json](claims.json), [evidence-index.json](evidence-index.json), and [closeout-status.json](evidence/closeout-status.json). `SHA256SUMS` identifies the packaged files.

## Final observed state

- The generic LineageOS 21 ARM64 target passed lunch, Soong and Kati and compiled real Android target code, including `toybox`.
- The larger image build subsequently **failed with direct exit 1 on 2026-11-12 at 03:09:45 UTC**. Its manifest-generation command attempted to write a repo Git-config cache on a read-only filesystem. The last logged action was 136780/151184; this is not a percentage of a finished or working ROM.
- Both WSL distributions were stopped when checked for closeout. No build was restarted and no shutdown or kill was necessary.
- No completed system image was verified. The stopped Linux filesystem was not reopened merely to inventory partial outputs. Existing objects, source, logs and analysis projects remain preserved locally.
- Real offline VINTF and combined SELinux checks ran. They found specific framework/vendor integration problems; controls limit conclusions about Samsung's shipping runtime.

The earlier Wave 10 handoff said the image build was running. That was accurate at its observation time; the later exit/log evidence above now supersedes it. See the [sanitized final failure](evidence/last-build-failure.txt).

This package contains original synthesis and compact evidence, not Samsung firmware, proprietary blobs, signing secrets, device identifiers, or a full Android source checkout. It has not been published automatically. Detailed local source reports are indexed but not bundled; public readers should distinguish documented results from independently reproducible tests requiring those artifacts.
