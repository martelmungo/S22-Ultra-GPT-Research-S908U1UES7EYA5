# Resume guide

**Do not resume automatically.** The owner paused this project on 2026-11-15. Preserve the current artifacts until the owner requests more work.

## Read in this order

1. [README](README.md) and [closeout status](evidence/closeout-status.json).
2. [ROM build checkpoint](ROM_BUILD_AND_COMPATIBILITY.md) and [final failure](evidence/last-build-failure.txt).
3. [Corrections](CORRECTIONS_AND_NEGATIVE_RESULTS.md), then the relevant [security branch](SECURITY_RESEARCH.md).
4. [Open questions](OPEN_QUESTIONS.md), [evidence index](EVIDENCE_INDEX.md), then the actual local source reports/exports.

The separate private local-resume note maps the portable names below to existing paths. It is intentionally absent from this GitHub package.

## Before any new build

- Inspect distribution state and actual processes. Do not reuse historical PIDs. The closeout observed WSL stopped and the last build exit 1; it did not restart Linux.
- Resolve SOURCE_ROOT, OUT_DIR, product and build user from the local checkpoint. Verify the pinned manifest hash and current source changes before changing anything.
- Inspect Git metadata. A copied .git pointer or hardlinked editable source is not an isolated tree. Preserve the U1 scaffold and generic track independently.
- Check real memory, swap, disk and any competing workload. The prior 48 GB setting is historical until remeasured. Preserve custom kernel and unrelated projects.
- Preserve the failed log/direct status. Never run clean/clobber or resync the entire source solely because a single generated manifest action failed.
- Diagnose the repo cache write contract narrowly. Retain sandboxing, provenance and security checks. Do not publish or use a fabricated build-manifest as a successful output.
- Use one build owner, the existing generic OUT_DIR, scoped host tools and a new log/status filename. Capture the build process's direct exit. The old launcher also requested extra diagnostic targets; review its target list rather than blindly rerunning it.

## After a successful image build

Verify the exact artifact's freshness, SHA-256, format, filesystem size, actual contents, symlinks and signing metadata. Check the realized authoritative image filter rather than all staging files. Enumerate which partition images really exist. Do not call a development-signed generic image U1-compatible, installable or bootable.

Rerun offline compatibility with a stable generated snapshot, original vendor/ODM, complete APEX inputs, all relevant SKU scenarios and the stock kernel configuration. Retain all-stock controls and compiler/version assumptions. Resolve actual policy/interface differences without permissive domains, blanket ignore flags or dummy services.

## Security resumption gates

Choose one exact missing artifact from OPEN_QUESTIONS; do not run another whole-program audit. Preserve the distinction between owner authorization, image installation, execution and durable state. Existing Ghidra/REA work is reusable; no reinstall or change to unrelated MCP services is required. A stopped/blocked historical experiment is not permission to evade its limitation.

## Short handoff prompt

> Resume the paused SM-S908U1 EYA5 project from this closeout and its local path map. First verify state without starting a build. The latest generic build failed at build-manifest.xml because repo tried to write its Git-config JSON cache on a read-only filesystem. Preserve the pinned LineageOS 21 source and existing OUT_DIR. Propose and validate only a narrow evidence-backed correction, then continue incrementally if authorized. Keep full b0quew, generic compilation, offline compatibility, hardware and boot/install authority separate. Do not interact with the phone or reopen parked security searches without new evidence.
