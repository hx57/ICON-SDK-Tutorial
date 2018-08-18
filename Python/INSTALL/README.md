# Windows Installation

- ⏰ *Approximative completion time :  5 minutes*

In order to install the Python `iconsdk` package on Windows, we need the appropriate development environment for doing so. Unfortunately, the `iconsdk` package has a dependency with `secp256k1`, which is quite hard to compile on Windows OS, as it hasn't been ported to Windows development environment and [nobody has fixed it yet](https://github.com/ludbb/secp256k1-py/issues/28 "nobody has fixed it yet").

The purpose of this tutorial is not about installing a complicated cross-platform compilation environment. Fortunately, Microsoft has developed **WSL**, that allows us to run the Linux development tools directly on **Windows 10**.

Before continuing, **please install WSL** following these instructions (also detailed here : [https://docs.microsoft.com/en-us/windows/wsl/install-win10](https://docs.microsoft.com/en-us/windows/wsl/install-win10 "https://docs.microsoft.com/en-us/windows/wsl/install-win10"))

- **[[Show Video](https://gfycat.com/WholeSmartAttwatersprairiechicken "Show Video")]** Open a Powerwhell as **Administrator** type the following command and reboot :

    `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux` 
- **[[Show Video](https://gfycat.com/CircularFearlessHylaeosaurus "Show Video")]** Open the **Windows Store**, and chose your favorite Linux distribution - I prefer **Debian**. Once installed, chose any username / password your want.

Once it is done, you can follow the Linux installation instructions below.

# Linux installation

- ⏰ *Approximative completion time : 15 minutes*

The `iconsdk` Python package currently requires a Python version >= 3.6.

At the time of writing this tutorial, you may notice that if you type in your console `sudo apt search python3.6`, you will have no result. The same goes for `python3.7` package.
Indeed, these packages are still in testing in most Linux distributions. 
As this tutorial needs to be as generic as possible, we're going to **compile the latest Python version** from source :

#### Compile Python 3.7

**[[Show Video](https://gfycat.com/SlimThreadbareInsect "Show Video")]**

```bash
# Make sure we're updated
$ sudo apt update && sudo apt upgrade
# Install all building dependencies
$ sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev  libncursesw5-dev xz-utils tk-dev autoconf libtool libsecp256k1-dev
# Download and extract Python
$ cd /tmp
$ wget https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tgz --no-check-certificate
$ tar xvf Python-3.7.0.tgz && cd Python-3.7.0
# Compile Python 3.7
$ ./configure --enable-optimizations --with-ensurepip=install
$ make -j8 build_all && sudo make -j8 install
# Cleanup
$ sudo rm -rf /tmp/Python-3.7.0 && cd /tmp
```

Once your Python environment is installed, run the following command to check your python installation was successful :

**[[Show Video](https://gfycat.com/GoodRadiantAmurminnow "Show Video")]**
**[[Show Video](https://gfycat.com/AmpleHeftyDrever "Show Video")]**

```bash
$ python3.7 # Type Ctrl+D for exiting
$ pip3.7
# Make sure we're using the latest pip version
$ sudo pip3.7 install --upgrade pip
```

#### Download and install ICON SDK Python

We are now ready to install the `iconsdk` package.
Let's grab the latest version on Github :

**[[Show Video](https://gfycat.com/ComplicatedFloweryBlueandgoldmackaw "Show Video")]**
```bash
$ sudo apt install git && git clone https://github.com/icon-project/icon_sdk_for_python.git
$ cd icon_sdk_for_python
```

And we can finally install it (it will take few minutes) :

**[[Show Video](https://gfycat.com/ThunderousNeighboringLabradorretriever "Show Video")]**
```bash
$ sudo python3.7 setup.py install
```

#### Optional testing

You did it ! Now let's do some testing to check everything is working well.
We're going to use the "get_balance" utils function, and get the balance of the address **`hxe0ce109f237ef5265f3fc548e5d5ea50ed0fa93e`** on the **testnet**.

**[[Show Video](https://gfycat.com/HonestBareFlyingsquirrel "Show Video")]**
```bash
$ python3.7
Python 3.7.0 (default, Aug 18 2018, 22:42:36)
[GCC 6.3.0 20170516] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import icx.utils
>>> icx.utils.get_balance("hxe0ce109f237ef5265f3fc548e5d5ea50ed0fa93e", "https://testwallet.icon.foundation/api/")
100000000000000000000000000
```

`100000000000000000000000000`is the amount of "loops" (1 loop = 10^-18 ICX), so converted to ICX, the balance of the address `hxe0ce109f237ef5265f3fc548e5d5ea50ed0fa93e` is 100,000,000 ICX.

According to the [testnet tracker](https://trackerdev.icon.foundation/address/hxe0ce109f237ef5265f3fc548e5d5ea50ed0fa93e "testnet tracker"), it is correct.

![A lot of ICX!](https://i.imgur.com/4Ds0apv.png "A lot of ICX!")


