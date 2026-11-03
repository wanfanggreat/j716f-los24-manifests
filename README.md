# TB-J716F (Lenovo TB-J716F / p11_pro_plus) — LOS24 source snapshot manifests

Public manifest repository for reproducible checkouts of the LineageOS 24
(Android 17) source tree used by the TB-J716F development project.

## Production candidate (P3-E11, 2026-11-03) — current

`manifests/los24-production-20261103.xml` is the production-oriented
candidate derived from the E9 verified baseline. It keeps the device-specific
fixes E9 depends on and normalizes purely diagnostic local deviations:

| Project | Revision | Change |
| --- | --- | --- |
| `device/lenovo/p11_pro_plus` | `1dec41130f5d5fa9ace5108c85ab91245694ec7f` (`j716f/production-e11`) | remove inert LHDC vendor.prop property (0017) and the uninstalled `tools/drm_probe` diagnostic |
| `hardware/interfaces` | `13fc23dae0dcd24fc2411d74f54a407999eaf555` (upstream) | drop deprecated KeyMint `/nonsecure` rename (0003); verified images never installed it |
| `system/vold` | `11d8982f824d52e8122c7aa732207ba66b8f23c8` (upstream) | restore `vold` `reboot_on_failure` (0010) |
| `system/apex` | `ce2bb9f710183c864040401d859874b5723e44d8` (upstream) | restore `apexd`/`apexd-bootstrap` `reboot_on_failure` (0010) |
| `system/security` | `8ccaac9cc1c3d3bc15427e6ed55e0d774a647e70` (upstream) | drop P2-D0-S/P2-D2 diagnostic logging; native KeyMint TEE selection is upstream behavior |
| `system/core` | `65d661b1a536701e8b118be4e9728d7b5ca727c9` (upstream) | drop zero-net-diff diagnostic commits |
| `system/netd` | `fb7e47052620fd1139522e2b3d1343c8680f7c0f` (upstream) | drop zero-net-diff diagnostic commits |

Retained on purpose (device-specific or required by the 4.19 kernel):
bionic `close_range` fallback, kernel `NL80211_WPA_VERSION_3` backport,
wpa_supplicant SAE_PWE revert, libbase/libhidl legacy ABI, display ABI/DPPS,
AHandler compatibility, audio product strategy, LHDC default disable,
WPA3 SoftAP/GeoTZ, Connectivity 4.19 BPF kernel-version exemption, BPF
ringbuf/self-test workarounds and the `[p11-bpfdiag]` per-object loader.
The BPF diagnostics are retained until device evidence proves all five BPF
objects load and `reboot_on_failure` can be restored safely.

```sh
mkdir los24-prod && cd los24-prod
repo init -u https://github.com/wanfanggreat/j716f-los24-manifests.git \
          -m manifests/los24-production-20261103.xml
repo sync -c -j8
```

SHA-256: `8665875fa5b36db92e32c7360469b0acebb48246f398f5d9b814270cbc7fea0e`
(1,241 projects, all full SHAs, no local paths/private remotes).

Status: **SOURCE NORMALIZED / INCREMENTAL BUILD PASS**;
**CLEAN BUILD NOT VERIFIED / DEVICE NOT RE-TESTED**. The E9 manifest below
remains the device-verified baseline and is not modified by this candidate.

## E9 verified baseline (2026-11-03)

`manifests/los24-lock-20261103.xml` pins the exact source tree that produced
the device-verified P3-E9 system:

| Partition | Image SHA-256 (sparse) | Source |
| --- | --- | --- |
| `system_a` | `14ed1c02bc22be6689c67463c0dcea52e1446557aa9ec3f9a179338ac63b098b` | this manifest |
| `vendor_a` | `3301cc936f9389d7b11491a2da224de1961867cb6c000d8080e7e750437e36d9` | this manifest (E8) |
| `boot_a` | `f49fc7c6288f6564763ae1a5502e1d09039b5306b5ab45850d9d0be5afb98757` | this manifest (E8 kernel `38a4166`) |

It is the 2026-10-30 workspace snapshot with exactly three revisions changed:

| Project | Revision | Change |
| --- | --- | --- |
| `bionic` | `1800befe3661960bfe1df66a7539cfc2ea95a459` | `close_range(2)` ENOSYS fallback (`patches/0024`, E9 SoftAP) |
| `kernel/lenovo/sm8250` | `38a416623600883e93bf45daf884036df9ee6116` | `NL80211_WPA_VERSION_3` backport (`patches/0022`, E8) |
| `external/wpa_supplicant_8` | `02657e83c57f96f74242e080a76e21685a2d486c` | revert SAE_PWE workaround (`patches/0023`, E8) |

```sh
mkdir los24 && cd los24
repo init -u https://github.com/wanfanggreat/j716f-los24-manifests.git \
          -m manifests/los24-lock-20261103.xml
repo sync -c -j8
```

The manifest is self-contained: all remotes use absolute URLs, all 1,241
projects are pinned to full 40-character SHAs, and no local path or private
remote is referenced. `bionic` is the 20th project pinned to a public
`wanfanggreat` fork (`android_bionic`).

Status: **SOURCE COMPLETE / FETCHABLE PASS**. A clean rebuild from this
manifest has **not** been executed, and the device is not re-verified by the
manifest itself.

## Workspace snapshot (2026-10-30)

`manifests/los24-workspace-snapshot-20261030.xml` pins the **actual working
tree** of the development checkout after the P3-E7 repair campaign: 1,241
projects, every revision a full 40-character commit that is publicly
fetchable.

This is a *source snapshot*, not a device-verified build and not the
production baseline. It deliberately contains experimental and diagnostic
state (for example the WPA3 SoftAP RRO and the Dolby Codec2 AHandler
compatibility layer) exactly as it is built locally.

```sh
mkdir los24 && cd los24
repo init -u https://github.com/wanfanggreat/j716f-los24-manifests.git \
          -m manifests/los24-workspace-snapshot-20261030.xml
repo sync -c -j8
```

The repositories that carry local changes are pinned to public snapshots
under <https://github.com/wanfanggreat> on branch
`snapshot/local-source-20261030`:

| Project path | Snapshot repository |
| --- | --- |
| `build/make` | `android_build` |
| `device/lenovo/p11_pro_plus` | `android_device_lenovo_p11_pro_plus` |
| `external/wpa_supplicant_8` | `android_external_wpa_supplicant_8` |
| `frameworks/av` | `android_frameworks_av` |
| `frameworks/base` | `android_frameworks_base` |
| `hardware/interfaces` | `android_hardware_interfaces` |
| `hardware/qcom-caf/sm8250/display` | `android_hardware_qcom_display` |
| `kernel/lenovo/sm8250` | `android_kernel_lenovo_sm8250` |
| `packages/modules/Bluetooth` | `android_packages_modules_Bluetooth` |
| `packages/modules/Connectivity` | `android_packages_modules_Connectivity_j716f` |
| `system/apex` | `android_system_apex` |
| `system/bpf` | `android_system_bpf_j716f` |
| `system/core` | `android_system_core` |
| `system/libbase` | `android_system_libbase` |
| `system/libhidl` | `android_system_libhidl` |
| `system/netd` | `android_system_netd` |
| `system/security` | `android_system_security_j716f` |
| `system/vold` | `android_system_vold` |
| `vendor/lineage` | `android_vendor_lineage` |
| `bionic` (E9 baseline only) | `android_bionic` |

Three repositories use a `_j716f` suffix because a same-named private
repository already existed on the account; the project *path* is unchanged.

## Patches are already included

The TB-J716F `patches/` files are kept for history, but their effect is
already contained in the pinned commits. Do **not** apply them again on top
of this manifest.

## Verification status

- Source snapshot: PASS (every snapshot branch HEAD equals the local HEAD).
- Fetchable manifest: PASS (all 1,241 revisions fetched from the pinned
  remotes during verification).
- Build reproducibility: NOT VERIFIED (no clean build was run).
- Device: NOT TESTED.
