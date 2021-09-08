# Kernel 5.4 MHI backport

This repo contains backports of the drivers for the MHI bus, the MHI network interface driver and the new wwan framwork.
MHI is required driver for (among others) for using  SDX55-based modems (like Quectel RM500Q or Telit FN980) together
with PCI. Please note that the steps for enable PCI between each modem, and that you hardware (m.2 slot) has to support
PCI (often in addition to USB).

The repo is structured as follows:

* `drivers/bus/mhi`: Code for the MHI bus.
* `drivers/net`: Network interface driver and wwan framework.
* `include/linux`: MHI/wwan framework header files.
* `patches/kernel`: Kernel patches. For the individual commits, please refer to the repos I have based my backport on
  (`linux-next` and `net-next`).
* `patches/openwrt`: Contains a patch adding OpenWrt packages for the different drivers/framework.

I will try to keep this repo reasonably up to date, but PRs are more than welcome. The drivers are tested and works fine
on kernel 5.4.144. When testing, I used Quectel RM502Q-AE modem, and was able to communicate with the module and
establish a data connection (using `qmicli`) + send and receive data.

## List of changes

The backport is made up of the following changes: 

### MHI
* Based on `linux-next` `999569d59a0aa2509ae4a67ecc266c1134e37e7b`.
* Copied the `driver/bus/mhi` directory.
* Added `include/linux/mhi.h`.
* MHI-related changes `driver/bus/Kconfig`, `drivers/bus/Makefile` and `include/linux/mod_devicetable.h`.
* Backported `fsleep` to `delay.h`.

### mhi\_net
* Based on `net-next` `626bf91a292e2035af5b9d9cce35c5c138dfe06d`.
* Added the MHI network driver.
* Replace `u64_stats_t` + functions with `u64` and regular increment operations. `u64_stats_t` does not exist in kernel
  5.4.

### wwan framework
* Based on `net-next` `626bf91a292e2035af5b9d9cce35c5c138dfe06d`.
* Copied the `drivers/net/wwan` directory.
* Replaced `u64_stats_t`.
* Backport two `rtnetlink`-changes required by the wwan framework (separate patches).
