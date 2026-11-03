# TB-J716F (Lenovo TB-J716F / p11_pro_plus) — LOS24 source snapshot manifests

Public manifest repository for reproducible checkouts of the LineageOS 24
(Android 17) source tree used by the TB-J716F development project.

## E9 verified baseline (2026-11-03) — current

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
