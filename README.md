# TB-J716F (Lenovo TB-J716F / p11_pro_plus) — LOS24 source snapshot manifests

Public manifest repository for reproducible checkouts of the LineageOS 24
(Android 17) source tree used by the TB-J716F development project.

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
