# Findings and project outcome

## What was achieved

1. **Separated the authorities.** Android eligibility settings, the bootloader's ordinary unlocked state, an AVB key-trust verdict, OTA package authentication, partition writes and subsequent execution are different decisions. One successful decision does not automatically authorize the next. [S06, S07, S24–S29]
2. **Found a real alternate AVB trust input.** EYA5 LinuxLoader recognizes DeviceInfo `UserKey` as well as its fixed Samsung AVB key. Normal/recovery AVB uses this callback. It supports conditional locked-state acceptance when a legitimate matching key and every other required verification condition are satisfied. Owner enrollment and installation remain unproven. [S05, S06]
3. **Mapped the Android-to-recovery request.** Settings/FotaAgent requests `RecoverySystem.installPackage`; the framework writes the recovery command and requests recovery-update. Recovery consumes the request and independently checks OTA signatures and metadata before package-updater dispatch. The applicable genuine Samsung updater remains missing. [S24–S27]
4. **Improved executable provenance.** XBL contains multiple architecture members. VaultKeeper's malformed earlier extraction was corrected through native read-only F2FS recovery, rather than trimming apparent padding and trusting a broken ELF. [S13, S23]
5. **Prepared U1-specific build inputs.** The staged set contains 3,594 vendor payload files, 3,234 resolved active copy sources and 358 verified stock U1 modules, with the matching 5.10.209 kernel and boot artifacts. These are build inputs, not proof of runtime compatibility. [S33, S34, S36]
6. **Reached real compilation.** A pinned LineageOS 21 generic ARM64 target passed the build graph and compiled ARM64 `toybox`. Real `checkvintf`, `secilc`, and selected framework metadata/policy targets also built. The full `b0quew` product remains separately blocked. [S01–S03]
7. **Ran genuine compatibility tests.** Generated framework inputs plus authentic EYA5 vendor/ODM, kernel and vendor APEX metadata were tested. The overall VINTF check failed, and combined CIL compilation identified two exact filesystem-label collisions. All-stock controls exposed additional checker/compiler differences. [S03]
8. **Closed a useful mount-layout question.** The generic image rule packs nested product/system_ext content. Original EYA5 first-stage init implements the corresponding skip-mount mechanism after successful system-root mounting. This is conditional layout support, not image authorization. [S04]

## What did not become a working capability

There is no demonstrated owner-accessible UserKey provisioning operation, production Engineering Mode authorization, accepted persistent unlock transition, owner-signed OTA installation, accepted alternate ABL on U1, or repeatable custom-OS boot. No security hypothesis reached a demonstrated custom-OS capability. No source or firmware signature private key was recovered or required as a research assumption.

Retaining stock vendor is a useful engineering strategy: it preserves existing proprietary HALs, matching modules and firmware. It does not eliminate framework, policy, APEX, radio/IMS, encryption or installation requirements. The project did not validate a working SM-S908U1 LineageOS implementation.

## Outcome by workstream

| Workstream | Strongest supported result | Remaining limit |
|---|---|---|
| U1 unlock policy | Download Mode gate is constant false in examined LinuxLoader | Other authenticated routes and protected state must be assessed separately |
| UserKey | Conditional alternate AVB trust in normal boot | Authorized enrollment, actual phone contents, image installation |
| Engineering Mode | Authenticated mode-3 path can request ordinary state change | Production issuance, binding, accepted storage and next boot |
| Secure state | VerifiedBootDxe selects protected-service or GPT backend from SCM state | Runtime selection and actual secure handler policy |
| Early ABL authentication | Generic XBL loading/auth interfaces recovered | ABL-specific caller/result/handoff and root acceptance |
| Update system | Request-to-recovery and package authentication mapped | Genuine package updater and concrete partition-writing implementation |
| Full U1 ROM | Exact-build inputs and product scaffold | Ownership conflicts, packaging and runtime integration |
| Generic Lineage GSI | Build graph, real code and metadata compilation | Final image generation failed; compatibility is incomplete |
| Security implementation review | Bounded static observations and reproducible cryptographic checks | No demonstrated authorization bypass or usable device exploit |

## Research progression

Early work mapped U1/S908E policy and storage boundaries. Waves 1–4 recovered actual recovery/update clients, validated F2FS content, compared hardware files and created a U1 build scaffold. Waves 5–8 staged vendor/kernel inputs, pinned Android sources, exercised real build tools and narrowed the AVB contract concern. Wave 9 established the independent generic-GSI compilation track. Wave 10 applied the 48 GB WSL setting and performed real compatibility checks. Closeout records the later image-build failure and pauses further work.

The chronology is context; the corrected final findings take precedence over older checkpoint labels. Source IDs refer to [the evidence index](EVIDENCE_INDEX.md).
