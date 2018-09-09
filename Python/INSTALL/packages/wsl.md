# WSL Installation

- ⏰ *Approximative completion time :  5 minutes*

In order to install the Python `iconsdk` package on Windows, we need the appropriate development environment for doing so. Unfortunately, the `iconsdk` package has a dependency with `secp256k1`, which is quite hard to compile on Windows OS, as it hasn't been ported to Windows development environment.

The purpose of this tutorial is not about installing a complicated cross-platform compilation environment. Fortunately, Microsoft has developed **WSL**, that allows us to run the Linux development tools directly on **Windows 10**.

Before continuing, **please install WSL** following these instructions :

- **[[Show Video](https://gfycat.com/WholeSmartAttwatersprairiechicken "Show Video")]** Open a Powerwhell as **Administrator** type the following command and **reboot** :

    `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux` 
- **[[Show Video](https://gfycat.com/CircularFearlessHylaeosaurus "Show Video")]** Open the **Windows Store**, and chose your favorite Linux distribution - I prefer **Debian**. Once installed, chose any username / password you want.

Once it is done, you can follow the Linux installation instructions.
