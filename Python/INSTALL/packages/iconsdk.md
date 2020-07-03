# ICON Python SDK Installation

- ⏰ *Approximative completion time : 15 minutes*

The `iconsdk` Python package currently requires a Python version >= 3.7.

At the time of writing this tutorial, you may notice that if you type in your console `sudo apt search python3.8`, you will have no result.
Indeed, these packages are still in testing in most Linux distributions. 
As this tutorial needs to be as generic as possible, we're going to **compile the latest Python 3.8 version** from source :

#### 1 ▶ Compile Python 3.8

```bash
# Make sure we're updated
$ sudo apt update && sudo apt upgrade
# Install all building dependencies
$ sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev  libncursesw5-dev xz-utils tk-dev autoconf libtool libsecp256k1-dev
# Download and extract Python
$ cd /tmp
$ wget https://www.python.org/ftp/python/3.8.3/Python-3.8.3.tgz --no-check-certificate
$ tar xvf Python-3.8.3.tgz && cd Python-3.8.3
# Compile Python 3.8.3
$ ./configure --enable-optimizations --with-ensurepip=install
$ make -j8 build_all && sudo make -j8 altinstall
# Cleanup
$ sudo rm -rf /tmp/Python-3.8.3 && cd /tmp
```

Once your Python environment is installed, run the following command to check your python installation was successful :

**[[Show Video](https://gfycat.com/GoodRadiantAmurminnow "Show Video")]**
**[[Show Video](https://gfycat.com/AmpleHeftyDrever "Show Video")]**

```bash
$ python3.8 # Type Ctrl+D for exiting
$ pip3.8
# Make sure we're using the latest pip version
$ sudo pip3.8 install --upgrade pip
```

#### 2 ▶ Download and install ICON SDK Python

We are now ready to install the `iconsdk` package.
Let's grab the latest version (it may takes few minutes) :

```bash
# Install virtualenv
$ sudo pip3.8 install virtualenv

# Make the virtual environment using a Python 3.7 version in a folder called "venv"
$ virtualenv -p python3.8 venv

# Activate the newly created environment
$ source venv/bin/activate 

# Install ICON SDK
$ pip install iconsdk
```

#### 3 ▶ Optional testing

You did it ! Now let's do some testing in order to check that everything is working well.
We're going to test the `get_block` function, and get the latest block generated on the ICON **devnet** V3.

```bash
from iconsdk.icon_service import IconService
from iconsdk.providers.http_provider import HTTPProvider

# Creates an IconService instance using the HTTP provider and set a provider.
icon_service = IconService(HTTPProvider("https://bicon.net.solidwallet.io/", 3))

# Gets the latest block
block = icon_service.get_block("latest")
print(block)
```

At the time of writing this tutorial, this script returns me the following block, but you should observe another result.
```json
{
    "version": "0.1a",
    "prev_block_hash": "25457b45f6d77764e09dc1097a7b67fba61931835c44926e86124047cce1a70c",
    "merkle_tree_root_hash": "e043bad8f14b04b247b7ee6286fc7551bf164c37612fada6d5a8f258e1871d82",
    "time_stamp": 1536321057283295,
    "confirmed_transaction_list": [
        {
            "from": "hx23ada4a4b444acf8706a6f50bbc9149be1781e13",
            "to": "hx04d669879227bb24fc32312c408b0d5503362ef0",
            "value": "0xa968163f0a57b400000",
            "version": "0x3",
            "nid": "0x3",
            "stepLimit": "0xf4240",
            "timestamp": "0x575469eda6a60",
            "signature": "mxXaOEBUFOhpQAs/SNWjV/UZpJufoUZ+8r4kWv7gEtAwynaVrS7e9cBazoyF+tm038XMgcZDx+t+1mo8Zx63/gA=",
            "txHash": "0xe043bad8f14b04b247b7ee6286fc7551bf164c37612fada6d5a8f258e1871d82"
        }
    ],
    "block_hash": "f9c90a4cceaf1ff294c8e389ffaf962960e07cb85b017f155424279f49909d7f",
    "height": 5,
    "peer_id": "hx86aba2210918a9b116973f3c4b27c41a54d5dafe",
    "signature": "a+4Oh6Bh3dGYuhY5NesW8Fb9qLdLpnmA0xiEs28BWT97krt2gCyOZ4Yk5BvOYrqTRkV0WgasRoJi0fIVQsYmmAE="
}
```

