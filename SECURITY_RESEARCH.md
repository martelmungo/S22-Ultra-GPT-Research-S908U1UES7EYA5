# Security research: consolidated disposition

Scope: offline study of the identified EYA5 artifacts and specified comparison builds. Static reachability is not an observed handset execution trace. A hash establishes byte identity, not distribution authenticity or hardware trust. No phone flashing, protected-state update, key enrollment, exploit deployment or persistent owner-OS execution was performed in this research program.

## 1. Android eligibility and the ordinary unlock state

EYA5 LinuxLoader's reviewed Download Mode eligibility routine returns constant false. The ordinary state setters still exist, and the Engineering Mode caller is a separate path. Disabling one UI gate does not prove that every other required authorization is software-only. [S11, S12, S28, S29]

Android-side OEM-lock services and the PersistentDataBlock permission path were examined. The U1 EYA5 and S908E EYB1 compared classes have identical recovered JADX-output hashes, with shared eligibility infrastructure. This is a decompilation-output comparison, not whole-binary identity or proof that the same backend is selected on hardware. Android's permission byte in PERSISTENT is distinct from ordinary operational unlock state. Changing a Settings property would not override the demonstrated loader gate or secure record policy.

## 2. AVB UserKey: recognition without enrollment authority

**Artifact:** EYA5 AArch64 PE32+ `LinuxLoader.efi`, SHA-256 `75ca836b3b0e6759f61294d968d8579ec92275bf79ba36ff9aa836a0956721f1`. Addresses here are reported image-relative Ghidra addresses; outer container offsets are separate. [S05, S06]

| Interface | Established contract |
|---|---|
| `FUN_00038d18` | Loads the 0x9A0-byte DeviceInfo record through VBRwDevice |
| DeviceInfo +0x94 | 32-bit UserKey length |
| DeviceInfo +0x98 | Serialized key bytes; initializer clears a 0x800-byte region |
| `FUN_000393a0` | Returns cached UserKey pointer and length |
| `FUN_0000e240` | Compares candidate length and complete bytes against UserKey; also checks fixed OEM key |
| Fixed key RVA 0xE3A1B | 0x408-byte key blob matching the shipped EYA5 vbmeta key blob |
| `FUN_0000f798` | Installs the relevant AVB callbacks, with ordinary lock-state retrieval separate |

The fixed key blob SHA-256 is `fb11010b168c869f83a2cd8ba7d0e5e00adfd9aff5c9cc92ad5f2044ffae75b1`. A matching UserKey can satisfy the trust callback while ordinary state remains locked. The locked caller still requires successful slot verification, signature, descriptor, rollback and later boot processing. This is a conditional code-level capability, not a validated retail enrollment procedure.

Invalid-record initialization clears UserKey; it is not a dedicated owner enrollment API. `StoreUserKey`/`EraseUserKey` diagnostic strings did not establish reachable writers. Generic full-record transport does not establish that an arbitrary field update is authorized.

The reviewed direct compare lacks a visible independent stored-length cap at that point, while an optional copy branch checks its own bounds. This remains a bounded observation: backend integrity, reachable record values and complete AVB acceptance are not established. No malformed protected record was constructed, and no vulnerability is claimed from this alone.

## 3. VBRwDevice, Keymaster and storage namespaces

**Provider:** `VerifiedBootDxe`, SHA-256 `857a2ea3da3032982fd96078670edc59ce60295156c96eea0d4cc27d6020cbed`. The provider has an SCM-dependent choice between Keymaster/RPMB-backed operations and ordinary GPT `devinfo`. Operations 0x202/0x203 concern a 0x1000-byte provider-side record; the client DeviceInfo structure is 0x9A0 bytes. Those sizes describe different layers. [S07â€“S10]

The runtime handle is requested under the name `keymaster`, but the actual authenticated executable and dispatcher remain unidentified. Reviewed `skeymast`, `fabrickeymaster` and `skm` candidates were not proven matches; the `skm` numeric 0x202 operation has different semantics.

TzDxeLA's conditional fallback uses **local EFI selector 0x606**, not QSEE subcommand 0x416. It selects a logical Block I/O device by a metadata label, reads its logical range and passes the buffer to a type-7 loader. The GUID `FE2555BE-D716-4686-B9D0-79DB5921B70D` is publicly named `gEfiPartitionRecordGuid`; EYA5 reads a UTF-16 label at +0x38, consistent with a GPT entry name. The full provider contract, runtime partition object and matching authenticated payload were not recovered. Matching one offset does not establish the entire structure. Samsung PIT data is not a complete runtime GPT inventory.

The already-registered handle path is independent of fallback loading. Neither normal production fallback execution nor authenticated name-to-image binding is established. Image authentication, name registration, command authorization, integrity/replay protection, persistence and next-boot acceptance remain distinct questions. The exact 0x202/0x203 handler is the missing boundary; repeating filename or GUID scans does not recover it.

Engineering Mode's own RPMB data, STEADY token/ESI storage, PERSISTENT eligibility, ordinary DeviceInfo state, VaultKeeper data and ICCC data are not interchangeable records. Shared storage technology or naming does not establish shared authority.

## 4. Engineering Mode

LinuxLoader contains a signed, device-bound mode-3/bit-3 path that can call the ordinary unlock-state setter independently of the disabled UI gate. Normal and development RSA-2048 anchors differ. The development path still has certificate, token and binding requirements; its mere presence does not authorize a retail owner. [S11, S12, S16]

**Correction:** field ID 0x53 is sourced from the parsed server certificate object, not a proven direct hardware-state read. Certificate policy and key acceptance precede token/mode evaluation. Production issuance, accepted U1 provisioning, device binding, protected write acceptance and persistence are unproved. Synchronization can also request a false ordinary state when authorization is absent; invoking a setter is not a one-way, durable unlock demonstration.

## 5. XBL and pre-execution ABL authentication

The XBL package contains ARM32, RISC-V and AArch64 members. The AArch64 member starts at container offset 0x44F94, length 822192; its separate analysis recovered generic image-loading/authentication machinery. Function counts alone were never proof of encryption or a complete representation. [S13]

Configuration key 0x25 is pointer-valued. `FUN_1481fa04` retrieves it into auth-context +0x330; `FUN_1481fc7c` calls interface slot +0 with image metadata and segment ranges. Startup `FUN_14821610` initializes a 19-slot `boot_seclib_s` table, registered under key 0x25 by `FUN_14829680`. **0x98 is its 152-byte allocation size, not a provider selector or root index.**

The specific constructor context is not bound to that startup instance. No ABL-specific selection-to-helper-to-result-to-handoff transaction was established. Lower operation 0xA4 is not thereby proven to be a full signature/TrustZone/RISC-V verification command. The first ARM32 member's decoding remains incomplete; absent callers in the reviewed representation do not prove dead code.

Unchanged S908E DXC3 ABL is a comparison candidate, not a demonstrated U1 bootloader replacement. ROOT 2/ROOT 7 are certificate labels, not proven hardware slot numbers. MRC index 0, revision metadata and valid mathematics do not establish U1 root, product, role or rollback acceptance. This route is an alternative to using stock ABL, not a prerequisite for every custom-OS route. [S14, S15]

## 6. Cryptographic provenance and MBN discrepancy

The two EYA5 OTA certificate wrappers contain the same RSA public key despite different DER certificate hashes. That key differs from the FotaAgent APK signer, both Engineering Mode anchors and the fixed AVB key. These are public-key identity findings; no private signing key was reconstructed. [S16]

MBNv7 framing, certificate/signature boundaries and sampled ELF segment-hash entries were reproduced. DXC3 verifies with its leaf ECDSA P-384 key/SHA-384 over the tested 432-byte contiguous message. EYA5 and EYB1 do not verify under that construction or the tested alternative. A positive control narrows parser questions but does not establish the exact Samsung signing contract for every sample. **EYA5/EYB1 remain unresolved, not proven invalid.** Authentic distribution provenance and the exact signing/verifier implementation remain material gaps. [S15]

The cross-component method was useful for tracing producers and transformations. It did not find software components that combine into a Samsung firmware-signing secret. The earlier, separate symmetric-decryption project was methodological inspiration, not evidence that asymmetric signing keys live in this firmware.

## 7. Android update, recovery and partition installation

The confirmed chain is:

```text
Settings / FotaAgent -> RecoverySystem.installPackage
 -> recovery command + recovery-update request
 -> recovery command parser / --update_package
 -> InstallPackage -> verify_package
 -> TryUpdateBinary, only after accepted verification
 -> package-provided update-binary [actual applicable implementation missing]
 -> partition writer [not established]
 -> next-boot AVB/rollback [independent requirement]
```

Recovered recovery functions include RVAs 0xEEB98 (`InstallPackage`), 0xEF8A4 (`verify_package`) and 0xF04D0 (`TryUpdateBinary`); references and architecture identity are in the source catalog. EYA5 normally uses the non-A/B update arrangement. Package signature rejection prevents updater dispatch in the reviewed path. An optional Android precheck does not replace recovery's independent verification. [S24â€“S27]

Privilege to request an update is not privilege to install arbitrary contents. No demonstrated update-path call enrolls DeviceInfo UserKey. A small CSC fota.zip was not the actual OTA installer. No provenance-validated applicable package updater was acquired, so partition-write implementation and permitted targets remain unresolved.

The stock recovery-maintenance service is a different mechanism: its script checks recovery and conditionally asks applypatch to reconstruct it from stock boot plus a BSDIFF40 patch. Script sizes/SHA-1 tuples match the extracted stock boot/recovery images. That does not prove execution, accepted writes or permission to install a different OS. [S04, S38]

## 8. Odin and secondary EFI execution

Class-0 behavior includes skipped signature/enhancement routines and a conditional later write path, but no accepted unauthorized write was demonstrated. The earlier independent auth-name/destination mismatch hypothesis was closed in the reviewed worker: both labels are derived from the same selector. The selector's initialized value and absent in-image writer do not establish arbitrary destination selection. [S19â€“S21]

The GUID previously described as an ExtractImage provider is used by a shared multicore work/synchronization interface; ExtractImage is a registered task label. Its implementation and some runtime scheduling details remain unavailable. No reason was established to repeat the same selector/provider scan.

A vendor Guided-FV launch path reaches Odin; a separate filesystem/volume read helper has boot-header/DTBO callers. Neither establishes a generally reachable unsigned EFI OS loader. App-specific authentication and a relevant alternate execution path remain unresolved. [S18, S22]

## 9. VaultKeeper, ICCC and Android-side candidates

Native read-only F2FS extraction recovered coherent original VaultKeeper ELFs, correcting misleading zero-prefixed exports. The examined native/QSEE path operates in a distinct protected-data namespace. No relationship that grants DeviceInfo UserKey enrollment or ordinary boot-state write authority was demonstrated. ICCC's separate DeviceInfo terminology likewise did not establish the bootloader record contract. [S23, S06, S10]

These are bounded negative results, not a proof that no undocumented factory/service component exists anywhere. A new authenticated client tied to the actual record offsets and backend would be new evidence; repeating generic key strings would not.

## 10. Parser, execution-consistency and vulnerability screening

The reviewed normal Android boot path reuses AVB-loaded buffers; no verify-then-storage-reread substitution point was demonstrated. This does not cover every possible memory actor or prove every loader path secure. [S18]

For LinuxLoader's post-AVB header concern, descriptor `image_size` propagates to the loaded record/data_size. AVB operations slot +0x18 binds to the real partition-read callback at RVA 0xDA80, and the reviewed caller requires the expected transferred count. The allocation wrapper is consistent with an EFI pool call, but the full guaranteed accessible-extent contract remains partly unproved. Stock samples are large enough. A short authenticated input and reachable harmful overread were not demonstrated. The concern remains **Level 1**, not a bypass. [S17]

A separate VerifiedBootDxe short-buffer/emulation assignment was stopped by automatic review. It did not produce dynamic validation or an exploitability result; identical component hashes alone are not defect reproduction. Do not describe it as a successful emulator experiment. [S39]

Historical vulnerability screening covered kernel rtmutex/UVC/mremap, Samsung PACM, HDCP, image-codec and older Download Mode research. Recorded IDs include CVE-2026-43499, CVE-2024-53104, CVE-2025-0088, CVE-2026-20980, CVE-2025-20936, CVE-2025-21043 and CVE-2024-20865. These are pointers to the archived screening, not newly verified advisories or claims of target exploitation. Exact source/backport state, LTO mappings, entry reachability and downstream authority remained unresolved or bounded. Android/kernel privilege does not itself authorize secure pre-boot state or repeatable custom boot. [S30â€“S32]

## Evidence strength and final security conclusion

The project used Level 0 hypothesis; Level 1 target-specific suspicious implementation; Level 2 demonstrated software defect; Level 3 demonstrated security consequence; Level 4 capability satisfying a custom-OS requirement. Later build-policy incompatibilities are engineering findings, not newly discovered firmware vulnerabilities. No reviewed security result establishes a Level 4 owner-OS capability or complete exploit chain.

The central remaining security evidence is an authorized producer of UserKey or an accepted production state transition, plus an independently authorized installation path and durable next-boot acceptance. Missing evidence must not be converted into either guaranteed impossibility or a presumed bypass.
