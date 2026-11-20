# aml_bt — Amlogic Bluetooth drivers/userspace

Amlogic Bluetooth stack: SDIO/USB BT kernel drivers (`sdio_driver_bt`,
`w1u`, `w2`, `w2l`), coexistence driver, firmware blobs and the
`aml_hciattach` HCI bring-up tool.

## Verified platform

X96 Max Plus Ultra (Amlogic S905X4 / SC2, kernel `6.18.59-meson64`):

- `sdio_bt.ko` creates `/dev/stpbt` (BT-over-SDIO channel)
- `aml_hciattach` loads `w1_bt_fw_uart.bin` and registers `hci0`
- **Note:** `aml_hciattach` uses `UART_DEV_PORT_BT` = `/dev/ttyS1`
  (hardcoded), while the BT UART on SC2 boards is `ttyS2`
  (`serial@80000`). Create the alias before starting:
  `ln -sf /dev/ttyS2 /dev/ttyS1`
- BD address: `/etc/bluetooth/aml/bt_addr`
- Config: `/usr/lib/firmware/aml/aml_bt.conf` (`BtChip=1` for W1)

## Releases

- [`x96-bt-6.18.59`](../../releases/tag/x96-bt-6.18.59) — packaged
  `sdio_bt.ko` + `aml_hciattach` + firmware + systemd units +
  `modules-load.d` config, verified on hardware (`hci0` up).

## Integration

Board support lives in
[AmakerGame/amlogic-s9xxx-armbian](https://github.com/AmakerGame/amlogic-s9xxx-armbian)
(`x96-max-plus-ultra` board files). Kernel packages are distributed via
the `kernel_s905x4` release tag there.
