+++
title = "Setup ADALM Pluto on Apple Sillicon"
date = 2024-08-19
updated = 2025-05-05
description = "Setup AD ADALM Pluto on Apple Sillicon based MacBook Pro"

[taxonomies]
tags = ["instruction", "tutorial", "ADALM Pluto", "Apple Sillicon", "MacOS", "brew", "python"]
+++

## For begining

In this tutorial I'll try to collect every options and commands for how to setup ADALM Pluto on typical Apple Sillicon based devices (MacBook's includes Pro versions, iMacs, etc)

## Brew

Update current state of brew to latest one

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

Use `iio_info` utitlity to check that device completed booting  process and it's ready to be used.

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

So system installed python or provided by `brew` have some restrictions about install packages from pip source.

```zsh
pip3 install pyadi-iio
error: externally-managed-environment

× This environment is externally managed
╰─> To install Python packages system-wide, try brew install
    xyz, where xyz is the package you are trying to
    install.

    If you wish to install a Python library that isn't in Homebrew,
    use a virtual environment:

    python3 -m venv path/to/venv
    source path/to/venv/bin/activate
    python3 -m pip install xyz
```

Instead of that I highly recommend to use virtual env based installation or conda.

For example you have your directory of `venv` named `Develop/bin` and `Develop` placed in your home directory.

Let's create virtual env for this path:

```zsh
python3 -m venv ~/Develop --system-site-packages
```

After installation of package will be simply as expected:

```zsh
~/Develop/bin/pip install pyadi-iio
Collecting pyadi-iio
  Downloading pyadi_iio-0.0.19-py3-none-any.whl.metadata (4.5 kB)
Requirement already satisfied: numpy>=1.20 in /opt/homebrew/lib/python3.13/site-packages (from pyadi-iio) (2.2.5)
Collecting pylibiio>=0.25 (from pyadi-iio)
  Using cached pylibiio-0.25-py3-none-any.whl.metadata (5.9 kB)
Downloading pyadi_iio-0.0.19-py3-none-any.whl (188 kB)
Using cached pylibiio-0.25-py3-none-any.whl (11 kB)
Installing collected packages: pylibiio, pyadi-iio
Successfully installed pyadi-iio-0.0.19 pylibiio-0.25

[notice] A new release of pip is available: 25.0.1 -> 25.1.1
[notice] To update, run: pip3 install --upgrade pip
```

## Let's talks with ADALM Pluto

Run python from our `venv` where we already installed `pyadi-iio` package:

```zsh
~/Develop/bin/python
```

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
```

Sometimes python crashes on internals of pyiio library. I hope it was on C part.

### Links

- [main tap](https://github.com/tfcollins/homebrew-formulae)
- [pyadi-iio](https://github.com/analogdevicesinc/pyadi-iio)
