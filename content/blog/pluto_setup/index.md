+++
title = "Setup ADALM Pluto on Apple Sillicon"
date = 2024-08-19
updated = 2024-08-19
description = "Setup AD ADALM Pluto on Apple Sillicon based MacBook Pro"

[taxonomies]
tags = ["instruction", "tutorial", "ADALM Pluto", "Apple Sillicon", "MacOS", "Brew", "Python"]
+++

## For begining

In this tutorial I'll try to collect every options and commands for how to setup ADALM Pluto on typical Apple Sillicon based devices (MacBook's includes Pro versions, iMacs, etc)

## Brew

Update current state of brew to last one

```zsh
brew update
brew upgrade
```

Install ADALM Pluto related tap. Don't worry that origin address, this guy works for AD officially.

```zsh
brew tap tfcollins/homebrew-formulae
```

Than install components for ADALM Pluto.

```zsh
brew install libiio libad9361-iio
```

## Pluto

Plug the device to your Mac, on the desktop will showned PlutoSDR disk.

use `iio_info` utitlity to check that device complete booting and ready to use.

```zsh
iio_info -s

Library version: 0.23 (git tag: v0.23)
Compiled with backends: xml ip usb
Unable to create Local IIO context : Function not implemented (78)
Available contexts:
	0: 0456:b673 (Analog Devices Inc. PlutoSDR (ADALM-PLUTO)), serial=1...0 [usb:2.4.5]
	1: 192.168.2.1 (Analog Devices PlutoSDR Rev.C (Z7010-AD9364)), serial=1...0 [ip:pluto.local.]

iio_info -u 'usb:2.4.5'
Library version: 0.23 (git tag: v0.23)
Compiled with backends: xml ip usb
IIO context created with usb backend.
Backend version: 0.24 (git tag: v0.24)
Backend description string: Linux (none) 5.10.0-98231-g9dfba10b795d #54 SMP PREEMPT Mon Jul 11 14:38:48 CEST 2022 armv7l
IIO context has 15 attributes:
	hw_model: Analog Devices PlutoSDR Rev.C (Z7010-AD9364)
	hw_model_variant: 1
	hw_serial: 1...0
	fw_version: v0.35
	ad9361-phy,xo_correction: 40000046
	ad9361-phy,model: ad9364
	local,kernel: 5.10.0-98231-g9dfba10b795d
	uri: usb:2.4.5
	usb,idVendor: 0456
	usb,idProduct: b673
	usb,release: 2.0
	usb,vendor: Analog Devices Inc.
	usb,product: PlutoSDR (ADALM-PLUTO)
	usb,serial: 1...0
	usb,libusb: 1.0.27.11882
IIO context has 4 devices:
...
```

## Python

I use python3 originally used in system. (for me this is python3.9)
Intall needed package for python

```zsh
pip install pyadi-iio
```

## Let's talks with ADALM Pluto

```python
import adi

# Create device from specific uri address
sdr = adi.ad9364(uri="usb:2.4.5")
# Get data from receiver channel
# Should return like this
# array([-15.-42.j, -60.-68.j, -44. -6.j, ...,  24. -5.j, -20. +1.j,
#        -4.-28.j])
# for every calls 
sdr.rx()
