# Corrections and bounded negative results

Later corrections take precedence over historical prose. Keep old evidence intact; do not silently rewrite history into a successful result.

| Earlier interpretation | Final disposition | Source |
|---|---|---|
| Keymaster fallback sends QSEE subcommand 0x416 | Relevant fallback uses EFI-local selector 0x606 | S08 |
| Matching +0x38 proves a full GPT protocol structure | Strong structural match; full provider/layout remains unresolved | S08 |
| A similarly named TA handles 0x202/0x203 | No exact runtime image/dispatcher bound; skm's 0x202 differs | S08–S10 |
| Engineering Mode field 0x53 reads hardware state | Value comes from parsed server-certificate object | S11 |
| XBL 0x98 is a provider/root selector | 152-byte allocation size for a 19-slot interface | S13 |
| Compatible interface signature proves actual ABL use | Instance and ABL caller/result/handoff linkage are missing | S13 |
| ROOT 7 certificate or MRC index proves U1 acceptance | Root trust/provisioning/role policy unresolved | S14, S15 |
| Offline InvalidSignature proves bad Samsung firmware | Tested MBN signing contract remains unresolved | S15 |
| Different OTA certificates imply different OTA keys | Two wrappers contain the same public key | S16 |
| VaultKeeper zero-prefixed export is valid code | Native F2FS extraction recovers coherent original ELF | S23 |
| Odin auth and destination strings can be independently chosen | Reviewed labels derive from the same selector; no independent mismatch shown | S19, S20 |
| ExtractImage names an Odin-only provider | It is a task label on a shared work/synchronization interface | S20 |
| Generic loader/read helper implies alternate OS execution | Relevant reachability and app authentication unproven | S18, S22 |
| Normal boot verifies then rereads replaceable partition bytes | No such second storage-read point demonstrated in reviewed path | S18 |
| Missing local size check proves reachable overread | Signed-input, callback and allocator guarantees must be resolved; Level 1 only | S17 |
| VerifiedBootDxe emulator reproduced a defect | Assignment stopped by automatic review; no dynamic result | S39 |
| Moving vendor output fixes applypatch ownership | Both competing producers move; real collision remains | S37, S38 |
| 563 overlap candidates are 563 build failures | Only applypatch/awk were reproduced in that work | S01, S38 |
| All installable metadata targets belong to the image | Authoritative image filter selects a smaller set | S02–S04 |
| Inheritance means separate product/system_ext images required | Generic generated graph packs them into system.img | S04 |
| Missing APEX mapping is a VINTF mismatch | Initial invocation lacked input; five authentic vendor APEXes were then inspected | S03 |
| Lineage checker failure proves Samsung stock cannot boot | All-stock controls also fail some checks; shipping runtime not reproduced | S03 |
| Wave 10 image build is still running | Later log/direct exit records failure at build-manifest generation; WSL stopped at closeout | Closeout evidence |

## What was not found within reviewed coverage

No authenticated owner UserKey writer was recovered from the reviewed LinuxLoader/Odin/provider and Android/vendor candidate paths. No production-authorized mode-3 issuance artifact was obtained. No applicable genuine Samsung OTA updater was recovered. The exact Keymaster handler and ABL-specific authentication transaction remain unbound. A matching EYA5 kernel source/Module.symvers set was not found. These are bounded coverage results, not universal impossibility claims.

Do not restart those searches without the new artifact or interface specified in [OPEN_QUESTIONS.md](OPEN_QUESTIONS.md). Do not repackage blocked emulation as a renamed assignment. No closeout step attempted new security tests or patched the build.
