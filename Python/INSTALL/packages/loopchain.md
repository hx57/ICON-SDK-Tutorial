# ICON Loopchain Installation

- ⏰ *Approximative completion time : 15 minutes*

`loopchain` package currently requires a Python version >= 3.6.

You should have installed it already, if not, [please follow the ICON SDK Installation Tutorial](iconsdk.md).

#### 1 ▶ Install packages dependencies

We're going to need the following dependencies in order to install loopchain (yes, that is a lot) :

```bash
$ sudo apt install -y git build-essential libssl-dev zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev xz-utils tk-dev autoconf libtool libsecp256k1-dev autoconf automake libtool pkg-config libleveldb1v5 libleveldb-dev openssl lsof
```

#### 2 ▶ Install RabbitMQ Server

We need to install and launch the RabbitMQ server :

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

#### 3 ▶ Install loopchain

```bash
$ git clone https://github.com/icon-project/loopchain.git
$ cd loopchain/
$ python3 -m grpc.tools.protoc -I'./protos' --python_out='./protos' --grpc_python_out='./protos' './protos/loopchain.proto'
$ virtualenv -p python3 ./venv
$ source ./venv/bin/activate
$ pip3 install git+https://github.com/icon-project/icon-service.git
$ pip3 install git+https://github.com/icon-project/icon-commons.git
$ pip3 install git+https://github.com/icon-project/icon-rpc-server.git
$ pip3 install -r requirements.txt
```

#### 4 ▶ Run loopchain

We first need to generate a key :

```bash
$ mkdir -p resources/my_pki
$ cd resources/my_pki
$ openssl ecparam -genkey -name secp256k1 | openssl ec -aes-256-cbc -out my_private.pem
$ openssl ec -in my_private.pem  -pubout -out my_public.pem
$ export PW_icon_dex={ENTER_MY_PASSWORD}
$ cd -
```

And finally, we can run it :

```bash
$ ./run_loopchain.sh
```
