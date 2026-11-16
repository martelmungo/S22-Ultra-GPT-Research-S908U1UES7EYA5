# ROM build, hardware and compatibility checkpoint

## Two strategies, not one completed ROM

| Track | Demonstrated | Not demonstrated |
|---|---|---|
| Full `b0quew` U1 product | U1 scaffold, exact-build vendor/kernel inputs, lunch/Soong | Kati completion, complete policy or device image |
| `lineage_gsi_arm64-ap2a-userdebug` | Pinned source, lunch/Soong/Kati, ARM64 code and real host-tool compilation | Completed verified system image, U1 hardware functionality, installation/boot authorization |

The full product has real duplicate owners for vendor applypatch and awk. A separate stock-evidenced vendor partition changes output paths, not competing ownership. The 563 inventoried overlap candidates are not 563 reproduced errors. Do not remove them in bulk. [S01, S37, S38]

## Preserved source and build identities

- LineageOS 21 / Android 14; manifest commit `121f4ec5f11dc98ff2c427bc9d23165e914c54bb`.
- Resolved manifest SHA-256 `659341b8c3cafa8d05d60fc25762a662be1c0eb7155ff11e87d1550af83abd74`.
- Source/build output on native WSL ext4, not a full NTFS-based Android checkout.
- WSL restart applied the existing 48 GB setting: measured MemTotal 49,328,404 kB, custom kernel `6.18.58.1-microsoft-standard-WSL2` retained. These are last-run measurements, not a promise about future hosts.
- Exact scoped host unzip/zip packages, logs and scripts remain locally recorded. Six build jobs were used after restart. No blanket security-validation bypass was added.

## Real build results and final failure

| Stage | Result |
|---|---|
| Generic lunch and `m nothing` | Exit 0; Soong/Kati success |
| Generic ARM64 toybox | Exit 0; installed AArch64 ELF, 562,832 bytes; SHA-256 `ea1041588771c73ac4ca82e3378435317d0164638593b9ce0e60ae3467f8acb7` |
| `checkvintf secilc` | Exit 0, 1776 actions; real pinned-branch tools |
| Narrow generated VINTF/policy inputs | Exit 0, 1306 actions |
| Larger generic systemimage | Exit 1 at 03:09:45 UTC on 2026-11-12; last logged action 136780/151184 |

The final failing target is `obj/ETC/build-manifest_intermediates/build-manifest.xml`. Its rule runs `python3 .repo/repo/repo manifest -o - -r`, filters proprietary project names and writes the build manifest. The repo implementation tries to save/remove `<BUILD_USER_HOME>/.repo_.gitconfig.json`; the build sandbox reports errno 30, read-only filesystem. The exact sanitized traceback is [preserved](evidence/last-build-failure.txt).

**No fix was attempted during closeout.** This traceback does not show that the WSL disk is corrupt or physically read-only; it is an observed write restriction in this build action. The next investigation should resolve the cache/output contract narrowly and preserve deterministic manifest generation. Do not substitute a fabricated manifest or make the entire source tree writable just to pass. Existing objects should be reused, not cleaned or resynchronized unnecessarily.

No completed system image has been verified. Some target objects, libraries and packages were compiled during the larger build, but the closeout did not reopen WSL to catalog partial outputs. A failed action count cannot establish a usable image.

## Actual framework layout

The evaluated generic image recipe packs `system/product` and `system/system_ext` within system.img, with root symlinks. It uses an explicit 6,974-path input filter. A diagnostic target list contained 114 available metadata rules; only 59 were product-selected. The compatibility snapshot excluded the others. Broad staging contents are not proof of image contents. [S02–S04]

Original EYA5 boot init reads the matching skip-mount configuration after successful system-as-root switching. This supports retaining original vendor-side partitions while the generated framework replaces the mounted product/system_ext views, conditionally on earlier mount/authentication success. It is not a flash recipe. The realized image and final filter still require verification after a successful build.

| EYA5 logical partition | Recorded stock extent bytes |
|---|---:|
| system | 6,294,384,640 |
| system_ext | 159,895,552 |
| product | 1,338,212,352 |
| vendor | 2,201,178,112 |
| odm | 21,389,312 |
| vendor_dlkm | 57,610,240 |

All six recorded logical partitions use F2FS in stock. Super metadata's group limit is 12,660,506,624 bytes. The generic image recipe selects dynamically sized ext4 with a 64 MiB reserve; these source/build rules do not establish final size or fit. Stock fstab/kernel evidence supports ext4 as a filesystem option, not authorization of a replacement image. [S04, S37]

## Genuine offline compatibility tests

The test set used generated selected framework files, exact EYA5 vendor/ODM metadata, actual kernel release/config and five extracted vendor APEXes. The APEX map modeled their factory-active state; it did not observe activation on a handset. No vendor APEX contained an additional etc/vintf directory. [S03]

| Scenario | Overall pinned checkvintf result |
|---|---|
| Generic framework + taro | Exit 65, 74 uncovered device HAL instances |
| Generic framework + cape or diwali | Exit 65, 92 uncovered instances each |
| Empty SKU fallback | Exit 70; no default stock vendor/ODM manifest |
| All-stock taro control | Exit 65, 14 uncovered instances |
| All-stock cape/diwali control | Exit 65, 18 uncovered plus required Samsung Bluetooth 2.0 mismatch |

The three generic named-SKU scenarios report no core manifest/matrix or supplied-kernel mismatch; the final failure is the added unused/uncovered-HAL check. This is not an overall pass. The all-stock control prevents treating every host-tool rejection as proof of Samsung runtime refusal. Actual SKU remains unobserved. The host's synthetic SELinux kernel-policy ceiling is another explicit test limit.

Combined generated-framework/stock-vendor CIL compilation exits 255: Lineage system_ext labels ISO9660 and UDF roots with their dedicated types, whereas stock vendor labels both `vfat`. Both definitions cannot coexist unchanged. No neverallow disabling or permissive workaround was applied. An all-stock control also fails, with 161 neverallow diagnostics but no genfscon conflict; it is not a full positive control. Correcting two labels alone is not a demonstrated complete policy solution.

Stock precompiled policy cannot automatically cover the new framework: its expected platform/system_ext/product policy hashes differ. The generic userdebug policy default and missing-kernel OTA warning were documented rather than hidden. The build uses development/test signing inputs, not Samsung authorization.

## U1 hardware dependencies

- 358 original U1 modules are paired with kernel `5.10.209-android12-9-29544585-abS908U1UES7EYA5`; matching source and Module.symvers were not recovered. Boot/vendor_boot header version is 4; eleven DTBO entries are present. Their runtime board selection remains untested.
- U1/S908E comparison found 3,716 byte-identical files among 4,674 shared regular-file paths. This is a bounded file-set statistic, not a working-ROM percentage. Compared module names match but their bytes/vermagic differ; S908E is not a drop-in U1 firmware replacement.
- Radio/RIL/IMS, carrier provisioning, camera/tuning, connectivity, display/GPU/audio, KeyMint/Gatekeeper, biometrics and FBE retain framework/native/policy dependencies. Stock vendor reduces reconstruction work without proving those dependencies satisfied.
- The 52 reviewed init declarations reference 27 absent expected paths; many are disabled/factory/conditional. Declarations are not proof that all 52 run or are mandatory for normal boot.
- qcv selects taro/diwali/cape from SoC information. Taro camera provider differs from the other manifests. Do not force taro solely from a platform label.
- Complete stock vendor includes its matched recovery maintenance patch. The staged full-product vendor tree does not. A Lineage generated recovery arrangement is a deliberate design alternative requiring consistent images, paths, policy and ownership, not a drop-in Samsung-equivalent substitution.

The build can resume independently of the authorization research. No compatibility result grants partition-write permission or accepted, repeatable locked-device execution.
