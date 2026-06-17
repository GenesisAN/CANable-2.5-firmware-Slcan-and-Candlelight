# CANable-2.5-firmware-Slcan-and-Candlelight
Two new high quality, speed optimized firmwares for CANable adapters with lots of new features.

![CANable Adapter](https://github.com/user-attachments/assets/061f60ba-14a2-4896-866f-6226fc9123f6)


This is the first project that combines the two CANable firmware's Slcan and Candlelight into one code base.
Dozens of bugs have been fixed.
Dozens of new features have been added.
This is the first Candlelight firmware for the STM32Gxxx processor family that supports CAN FD and works without bugs.
However the new firmware is still 100% backward compatible with the legacy Slcan / Candlelight firmware.
The firmware has been tested on the STM32G431 on the isolated adapters from MKS Makerbase and Jhoinrch up to 10 Mbaud.
It works also on the STM32G473 dual CAN channel board from Oleksii.
The WeActStudio v1 firmware is compiled for the STM32G0B1 processor.
The firmware has been designed to be easily expandable for future processors and boards.
Precompiled binary firmware files can be uploaded to the CANable with the new Firmware Updater.

<img width="577" height="484" alt="CANable STM32 Firmware Updater" src="https://github.com/user-attachments/assets/1364398f-fcd4-430e-aa8b-06cde32ce895" />


Please read the detailed 
- User Manual
- Slcan Developer Manual
- Candlelight Developer Manual
- Firmware Developer Manual

https://netcult.ch/elmue/CANable%20Firmware%20Update

________________________

Latest Updates:
You find the version history here:

https://netcult.ch/elmue/CANable%20Firmware%20Update#Source_Code

## CI/CD firmware releases

GitHub Actions builds all `Make_*` firmware variants automatically on every push, pull request, and manual workflow run.

To publish a versioned firmware release, create and push a tag in one of these formats:

```sh
git tag v26.06.17
git push origin v26.06.17
```

or:

```sh
git tag v260617
git push origin v260617
```

The release tag is converted to `FIRMWARE_VERSION=0xYYMMDD`, so the generated firmware file names and embedded firmware version match the release version. Workflow artifacts keep the full build outputs for debugging, while GitHub Releases publish only updater-friendly `.bin` files.

For HUD ECU Hacker's firmware updater, the workflow also creates updater-friendly `.bin` copies with this naming scheme:

```text
<MCU> - <FirmwareType>2.5 - <Board>.bin
```

Example:

```text
STM32G431 - Candlelight2.5 - WeActStudioV2.bin
```

Copy these `.bin` files into `C:\Program Files (x86)\HUD ECU Hacker\Driver\CANable Firmware Update\Firmware`. The Firmware Updater converts them automatically into `.dfu` files.

The project embeds this version through `GCC_Rules.mk`:

- `BUILD_TRUNK` adds the version to generated firmware file names.
- `-DFIRMWARE_VERSION_BCD=$(FIRMWARE_VERSION)` passes the version into C code.
- Candlelight returns the full version through `GS_ReqGetDeviceVersion`.
- Slcan returns the full version in the `V` command response.
- USB `bcdDevice` stores the year and month portion because that field is only 16-bit BCD.
