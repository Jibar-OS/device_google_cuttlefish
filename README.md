# device_google_cuttlefish — JibarOS Cuttlefish device tree

Fork of AOSP's `device/google/cuttlefish` with JibarOS-specific additions. This is the reference device for v0.x validation.

## What changed vs upstream AOSP

### `shared/device.mk`

Adds OIR modules to `PRODUCT_PACKAGES`:

```make
PRODUCT_PACKAGES += \
    oird \
    oir_default_model \
    oir_minilm_model \
    oir_whisper_tiny_en_model \
    oir_voice_sample_wav \
    oir_capabilities_xml \
    OirDemo
```

### `shared/sepolicy/system_ext/`

SELinux rules for OIR:

- `public/oir.te` — defines the `oir_service` Java domain and `oird` native domain types.
- `private/oir_service.te` — grants `system_server ↔ oir_worker` binder traffic + `oir_model_*_file` getattr.
- `private/oird.te` — oird's native domain: reads from `oir_model_*_file`, talks to `oir_worker`.

With these rules, JibarOS boots with SELinux **Enforcing** and zero OIR-scoped AVC denials.

## Building

```bash
repo init -u https://github.com/jibar-os/manifest -b main
repo sync -c -j8
source build/envsetup.sh
lunch aosp_cf_x86_64_phone-trunk_staging-userdebug
m -j8
launch_cvd --start_webrtc
```

## Non-Cuttlefish device trees

The same pattern (small `device.mk` additions + sepolicy fragments) applies to any AOSP device tree. Contributions for additional devices welcome — see [`github.com/jibar-os/docs`](https://github.com/jibar-os/docs) for the porting guide.

## See also

[`github.com/jibar-os/docs`](https://github.com/jibar-os/docs) — JibarOS OEM bake-in guide.

## Migration status

🚧 Tree migration in progress.
