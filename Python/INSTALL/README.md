# Windows Installation

- ⏰ *Approximative completion time :  5 minutes*

In order to install the Python `iconsdk` package on Windows, we need the appropriate development environment for doing so. Unfortunately, the `iconsdk` package has a dependency with `secp256k1`, which is quite hard to compile on Windows OS, as it hasn't been ported to Windows development environment.

The purpose of this tutorial is not about installing a complicated cross-platform compilation environment. Fortunately, Microsoft has developed **WSL**, that allows us to run the Linux development tools directly on **Windows 10**.

Before continuing, **please install WSL** following these instructions :

- **[[Show Video](https://gfycat.com/WholeSmartAttwatersprairiechicken "Show Video")]** Open a Powerwhell as **Administrator** type the following command and **reboot** :

    `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux` 
- **[[Show Video](https://gfycat.com/CircularFearlessHylaeosaurus "Show Video")]** Open the **Windows Store**, and chose your favorite Linux distribution - I prefer **Debian**. Once installed, chose any username / password you want.

Once it is done, you can follow the Linux installation instructions below.

# Linux installation

- ⏰ *Approximative completion time : 15 minutes*

The `iconsdk` Python package currently requires a Python version >= 3.6.

At the time of writing this tutorial, you may notice that if you type in your console `sudo apt search python3.6`, you will have no result.
Indeed, these packages are still in testing in most Linux distributions. 
As this tutorial needs to be as generic as possible, we're going to **compile the latest Python 3.6 version** from source :

#### 1 ▶ Compile Python 3.6

```bash
# Make sure we're updated
$ sudo apt update && sudo apt upgrade
# Install all building dependencies
$ sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev  libncursesw5-dev xz-utils tk-dev autoconf libtool libsecp256k1-dev
# Download and extract Python
$ cd /tmp
$ wget https://www.python.org/ftp/python/3.6.6/Python-3.6.6.tgz --no-check-certificate
$ tar xvf Python-3.6.6.tgz && cd Python-3.6.6
# Compile Python 3.6.6
$ ./configure --enable-optimizations --with-ensurepip=install
$ make -j8 build_all && sudo make -j8 install
# Cleanup
$ sudo rm -rf /tmp/Python-3.6.6 && cd /tmp
```

Once your Python environment is installed, run the following command to check your python installation was successful :

**[[Show Video](https://gfycat.com/GoodRadiantAmurminnow "Show Video")]**
**[[Show Video](https://gfycat.com/AmpleHeftyDrever "Show Video")]**

```bash
$ python3.6 # Type Ctrl+D for exiting
$ pip3.6
# Make sure we're using the latest pip version
$ sudo pip3.6 install --upgrade pip
```

#### 2 ▶ Download and install ICON SDK Python

We are now ready to install the `iconsdk` package.
Let's grab the latest version (it may takes few minutes) :

```bash
$ sudo pip3.6 install iconsdk
```

#### 3 ▶ Optional testing

You did it ! Now let's do some testing in order to check that everything is working well.
We're going to test the `get_block` function, and get the latest block generated on the ICON **testnet** V3.
Please note that I used an unofficial API V3 provider `http://13.209.103.183:9000/api/v3`, as there is no official one yet.

```bash
from iconsdk.icon_service import IconService
from iconsdk.providers.http_provider import HTTPProvider

# Creates an IconService instance using the HTTP provider and set a provider.
icon_service = IconService(HTTPProvider("http://13.209.103.183:9000/api/v3"))

# Gets the latest block
block = icon_service.get_block("latest")
print(block)
```

At the time of writing this tutorial, this script returns me the following block, but you should observe another result.
```
{'version': '0.1a', 'prev_block_hash': '25457b45f6d77764e09dc1097a7b67fba61931835c44926e86124047cce1a70c', 'merkle_tree_root_hash': 'e043bad8f14b04b247b7ee6286fc7551bf164c37612fada6d5a8f258e1871d82', 'time_stamp': 1536321057283295, 'confirmed_transaction_list': [{'from': 'hx23ada4a4b444acf8706a6f50bbc9149be1781e13', 'to': 'hx04d669879227bb24fc32312c408b0d5503362ef0', 'value': '0xa968163f0a57b400000', 'version': '0x3', 'nid': '0x3', 'stepLimit': '0xf4240', 'timestamp': '0x575469eda6a60', 'signature': 'mxXaOEBUFOhpQAs/SNWjV/UZpJufoUZ+8r4kWv7gEtAwynaVrS7e9cBazoyF+tm038XMgcZDx+t+1mo8Zx63/gA=', 'txHash': '0xe043bad8f14b04b247b7ee6286fc7551bf164c37612fada6d5a8f258e1871d82'}], 'block_hash': 'f9c90a4cceaf1ff294c8e389ffaf962960e07cb85b017f155424279f49909d7f', 'height': 5, 'peer_id': 'hx86aba2210918a9b116973f3c4b27c41a54d5dafe', 'signature': 'a+4Oh6Bh3dGYuhY5NesW8Fb9qLdLpnmA0xiEs28BWT97krt2gCyOZ4Yk5BvOYrqTRkV0WgasRoJi0fIVQsYmmAE='}
```

