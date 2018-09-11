# ICON T-Bears Installation

⏰ *Approximative completion time : 10 minutes*

⚠ *You should have installed the `iconsdk` already, if not, [please do](iconsdk.md)*

#### 1 ▶ Install ICON T-Bears dependencies

```bash
$ sudo apt install autoconf automake libtool pkg-config lsof
```

Then, we need to install and launch the RabbitMQ server :

 - On Windows :
    - At the time of writing this tutorial, the package manager in WSL don't provide a recent enough `rabbitmq-server` executable, so you will need to download the Windows native executable on [Github directly](https://github.com/rabbitmq/rabbitmq-server/releases). I chose `rabbitmq-server-3.7.8-rc.3.exe`, please chose the latest one. Once Erlang and RabbitMQ installed, launch the `rabbitmq-server.bat` executable (it should be located in `<rabbitmq_root_install_directory>\rabbitmq_server-3.7.8-rc.3\sbin\rabbitmq-server.bat`).

 - On Linux : 
    - Make sure your version is >= 3.7.x :
    ```bash
    apt-cache policy rabbitmq-server
    ```

    - If it is, install it : 
    ```bash
    sudo apt install rabbitmq-server
    ```

    - If it is not, follow the same instructions than on Windows with the Linux executable.

    - Launch the RabbitMQ server :
    ```bash
    sudo service rabbitmq-server start
    ```


#### 2 ▶ Download and install ICON T-Bears package

Once the RabbitMQ server is launched and listening, we can grab and install the latest `tbears` version :

```bash
$ sudo pip3.6 install tbears
```

Now we can start the `tbears` service :

```bash
$ tbears start
Started tbears service successfully
```

#### 3 ▶ Optional testing

You did it ! Now let's do some testing in order to check that everything is working well.
We're going to use the same script than in the `iconsdk` tutorial, but use our `tbears` as a provider instead.

```bash
from iconsdk.icon_service import IconService
from iconsdk.providers.http_provider import HTTPProvider

# Creates an IconService instance using tbears
icon_service = IconService(HTTPProvider("http://localhost:9000/api/v3"))

# Gets the latest block
block = icon_service.get_block("latest")
print(block)
```

This script should return a similar result for you if everything went well :
```json
{
    "version": "tbears",
    "prev_block_hash": "",
    "merkle_tree_root_hash": "tbears_block_manager_does_not_support_block_merkle_tree",
    "time_stamp": 1536517839613220,
    "confirmed_transaction_list": [
        {
            "nid": "0x3",
            "accounts": [
                {
                    "name": "genesis",
                    "address": "hx0000000000000000000000000000000000000000",
                    "balance": "0x2961fff8ca4a62327800000"
                },
                {
                    "name": "fee_treasury",
                    "address": "hx1000000000000000000000000000000000000000",
                    "balance": "0x0"
                },
                {
                    "name": "test1",
                    "address": "hxe7af5fcfd8dfc67530a01a0e403882687528dfcb",
                    "balance": "0x2961fff8ca4a62327800000"
                }
            ]
        }
    ],
    "block_hash": "66ec73e1f7031b05506f56d35cb19dd1c17a0f09a921e223acb787ec9bac0d84",
    "height": 0,
    "peer_id": "",
    "signature": ""
}
```

