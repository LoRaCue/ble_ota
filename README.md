# ESP BLE OTA Component description

## ⚠️ LoRaCue Surgical Modifications

**Upstream:** [espressif/esp-iot-solution](https://github.com/espressif/esp-iot-solution/tree/master/components/bluetooth/ble_profiles/esp/ble_ota)

**Modified Files:**
- `include/ble_ota_integration.h` (NEW) - Integration API
- `src/nimble_ota.c` - Removed `ble_svc_gap_init()` and `ble_svc_gatt_init()` from `ble_ota_gatt_svr_init()`

**Purpose:** Enable integration with existing NimBLE stacks without reinitialization conflicts.

### Sync with Upstream

```bash
# Add upstream remote (one-time setup)
git remote add upstream https://github.com/espressif/esp-iot-solution.git
git fetch upstream

# Create sync branch
git checkout -b sync-upstream-$(date +%Y%m%d)

# Copy latest ble_ota files from upstream
git checkout upstream/master -- components/bluetooth/ble_profiles/esp/ble_ota
mv components/bluetooth/ble_profiles/esp/ble_ota/* .
rm -rf components

# Reapply surgical modifications:
# 1. Keep include/ble_ota_integration.h (already exists)
# 2. Edit src/nimble_ota.c:
#    - Remove ble_svc_gap_init() and ble_svc_gatt_init() calls
#    - Add integration functions at end

git add -A
git commit -m "sync: merge upstream changes from esp-iot-solution"

# Create PR to main
gh pr create --title "Sync with upstream esp-iot-solution" --base main
```

# Create PR to master
gh pr create --title "Sync with upstream esp-iot-solution" --base master
```

---

``esp ble ota`` is a firmware upgrade component for data sending and receiving based on customized BLE Services. The firmware to be upgraded will be subcontracted by the client and transmitted sequentially. After receiving data from the client, packet sequence and CRC check will be checked and ACK will be returned.

## Example

- [examples/bluetooth/ble_ota](https://github.com/espressif/esp-iot-solution/tree/master/examples/bluetooth/ble_ota): This example is based on the ble_ota component, upgrade firmware via BLE.

You can create a project from this example by the following command:

```
idf.py create-project-from-example "espressif/ble_ota^0.1.8:ble_ota"
```

> ![TIP]
>
> When using NimBLE, it may be necessary to increase `CONFIG_BT_NIMBLE_HOST_TASK_STACK_SIZE`。
