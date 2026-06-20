<p align="center">
<img src="/.media/rtl8188eus_logo.png" alt="rtl8188eus logo" width="50%"/>
</p>

Realtek rtl8188eu(s) / rtl8188etv wireless drivers
==================================================

[![Monitor mode](https://img.shields.io/badge/monitor%20mode-supported-brightgreen.svg)](#)
[![Frame Injection](https://img.shields.io/badge/frame%20injection-supported-brightgreen.svg)](#)
[![MESH Mode](https://img.shields.io/badge/mesh%20mode-supported-brightgreen.svg)](#)
[![GitHub issues](https://img.shields.io/github/issues/SimplyCEO/rtl8188eus.svg)](https://gitlab.com/SimplyCEO/rtl8188eus/-/issues)
[![GitHub forks](https://img.shields.io/github/forks/SimplyCEO/rtl8188eus.svg)](https://gitlab.com/SimplyCEO/rtl8188eus/-/forks)
[![GitHub stars](https://img.shields.io/github/stars/SimplyCEO/rtl8188eus.svg)](https://gitlab.com/SimplyCEO/rtl8188eus/-/starrers)
[![GitHub license](https://img.shields.io/badge/License-GPL--2.0-informational)](https://gitlab.com/SimplyCEO/rtl8188eus/-/blob/master/LICENSE)<br>
[![Android](https://img.shields.io/badge/android%20(8)-supported-brightgreen.svg)](#)
[![aircrack-ng](https://img.shields.io/badge/aircrack--ng-supported-blue.svg)](#)

Trying to find a solution? See [troubleshooting](/docs/TROUBLESHOOTING.md).

|   Support         |   Tested  |   Status  |   Description                                     |
|-------------------|-----------|-----------|---------------------------------------------------|
|   Android 7+      |   ❌      |   🟡      |   Depends on which kernel version is installed.   |
|   MESH            |   ❌      |   🟠      |   Not tested yet.                                 |
|   Monitor Mode    |   ✅      |   🔵      |   Tested and working.                             |
|   Frame injection |   ✅      |   🔵      |   Tested and working.                             |
|   Kernel 5.8+     |   ✅      |   🟢      |   Kernel 5.15+ tested.                            |
|   Kernel 6.17     |   ✅      |   🟢      |   Tested and working (see kernel 6.3+ notes).     |

Kernel 6.3+ Compatibility Fixes
--------------------------------

This fork adds fixes required to build and run on kernel 6.3 and newer (tested on **6.17**).


Building
--------

The quickest compile can presume:
```shell
git clone --depth 1 https://github.com/Kobsser/rtl8188eus.git
cd rtl8188eus/
make -j$(nproc)
sudo make install
sudo modprobe --remove rtl8xxxu && sudo modprobe 8188eu
```

The old driver will be kept, but it needs to be deactivated.
Verify if your kernel is equal or newer than `6.3.x`.
If it is, then the driver to blacklist is `rtl8xxxu`. Otherwise it is `r8188eu`.

```shell
# For kernel 6.3+
echo "blacklist rtl8xxxu" | sudo tee /etc/modprobe.d/rtl8xxxu-blacklist.conf
sudo update-initramfs -u
```

Or with DKMS:
```shell
su -c "sh dkms-install.sh"
```

Secure Boot
-----------

If your system has Secure Boot enabled, the module must be signed after every install.
You will need an enrolled MOK key. If you don't have one yet, see [this guide](https://ubuntu.com/blog/how-to-sign-things-for-secure-boot).

Once your MOK key is enrolled:

```shell
sudo /usr/bin/kmodsign sha256 \
  /var/lib/shim-signed/mok/MOK.priv \
  /var/lib/shim-signed/mok/MOK.der \
  $(find /lib/modules/$(uname -r) -name "8188eu.ko")
```

Run this after every `make install`, then reload the module:

```shell
sudo modprobe --remove 8188eu && sudo modprobe 8188eu
```

---
All the instructions and explanations can be found by<br>
[reading the documentation](/docs/BUILDING.md) or by accessing the topics:

- [Building for Kali Nethuner](/docs/BUILD_FOR_NETHUNTER.md);
- [Available Modes](/docs/MODES.md);
- [Configuring NetworkManager](/docs/NETWORKMANAGER.md);
- [Managed/Monitor Mode: toggle-script](/docs/OPTIONAL.md).

Credits
-------

Realtek       - https://www.realtek.com<br>
Alfa Networks - https://www.alfa.com.tw<br>
aircrack-ng  - https://www.aircrack-ng.org<br>
Project contributors - https://gitlab.com/SimplyCEO/rtl8188eus/-/graphs/master?ref_type=heads<br>

And all those who are using, requesting support, or teaching. Thanks!
