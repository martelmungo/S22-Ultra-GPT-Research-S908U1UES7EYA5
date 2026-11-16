# Open questions and evidence gates

The project is paused. This table is a resumption plan, not active scheduled work.

| Priority / branch | Exact next evidence | Why it matters / stopping rule |
|---|---|---|
| Build: generic image | Diagnose repo Git-config cache write during build-manifest generation; make a narrow reproducible correction, then resume existing OUT_DIR | Current real failed action. Do not clean or globally enable source writes. |
| Compatibility | Source-backed ownership of ISO9660/UDF labels; justified Samsung/QTI matrix extensions and explicit handling of all-stock controls | Two real policy collisions and known coverage differences. Passing metadata alone is insufficient. |
| Image verification | Successful direct systemimage exit plus current file hash, format, selected contents, AVB/test-signing and actual size | Staging and compilation do not prove a complete image. |
| Full b0quew | A consistent recovery image/script/patch design and one applypatch/awk owner | Separate from generic build. Do not remove all candidate overlaps. |
| UserKey | Authenticated client/handler that populates the 0x9A0 record's +0x94/+0x98 fields, with caller policy and accepted response | Read/get/compare evidence does not grant enrollment authority. Park without that producer. |
| Engineering Mode | Authentic production SM-S908U1 provisioning/issuance contract and device-bound authorization semantics | A valid nonproduction analogy or setter caller is insufficient. |
| Protected state | Exact runtime backend and authenticated 0x202/0x203 handler with record policy | Needed for accepted persistence, replay/integrity and next boot. Do not repeat candidate-name scans. |
| Genuine OTA updater | Unmodified, provenance-verifiable applicable OTA with update-binary and build metadata | Closes package dispatch to actual partition writer; factory AP archives are not substitutes. |
| Alternative ABL | ABL-specific XBL configuration/caller/result/handoff plus accepted-root/role/rollback inputs | Generic auth helpers and signature validity do not establish U1 acceptance. |
| MBN discrepancy | Exact Samsung/Qualcomm signer/verifier contract or independently authenticated sample/reference verifier | Resolve format assumptions before calling production signatures invalid. |
| Kernel/hardware | Matching EYA5 kernel source/config/symbol versions and U1-specific service/interface evidence | Stock artifacts support a prototype; source reproduction and runtime behavior remain separate. |
| AVB contract concern | Missing allocator/service-table contract or new authentic input provenance | Existing Level 1 analysis is parked; repeating the same disassembly adds no evidence. |

Potential evidence acquisition routes include official source releases and interface documentation, lawfully accessible authentic update artifacts, existing local exports and new version-specific source definitions. Availability is not promised. No physical-device experiment is authorized by this resume plan.

## Candidate routes are conjunctions

| Route | What exists | What must still be satisfied |
|---|---|---|
| Stock bootloader + UserKey | Conditional alternate AVB trust | Enrollment authority, valid image, state/rollback acceptance, installation, compatible OS and fresh-boot persistence |
| Stock bootloader + Engineering Mode | Authenticated caller of ordinary setter | Production issuance/binding, accepted protected update, persistence, installation and OS behavior |
| Official OTA/recovery | Authenticated Samsung package request/verification | Owner-content signing authority or another supported authorization, genuine writer contract, later AVB/rollback |
| Unchanged alternative ABL | DXC3 mathematical signature control | U1 earlier-stage acceptance, role/target/rollback and hardware compatibility, usable downstream authorization |
| Implementation defect | Bounded observations only | Defect reproduction, real input reachability, security consequence and a complete required capability; none established |

No globally minimal solution is proven from this incomplete dependency graph. A compatible ROM is necessary regardless of which authorization route might later become available.
