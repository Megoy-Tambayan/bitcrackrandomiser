# Run on Windows

Download and install .NET 6.0.x runtimes for Windows from [Microsoft](https://dotnet.microsoft.com/en-us/download/dotnet/6.0).

1 - Go to [ilkerccom/bitcrackrandomiser/releases](https://github.com/ilkerccom/bitcrackrandomiser/releases/tag/v1.0.6) and download latest released "bitcrackrandomiser.zip" file.

2 - Unzip downloaded file.

![bitcrackrandomiser](https://i.ibb.co/S0MNrx1/1.png)

3 - Edit `settings.txt` file from "bitcrackrandomiser" app folder.

a. Enter "user_token" value. Get your "user_token" from [btcpuzzleinfo.com/dashboard](https://btcpuzzleinfo.com/dashboard) website.

b. Enter "wallet_address" value. You should use your BTC wallet address.

c. Save the file.

4 - Run the "**BitcrackRandomiser.exe**"

![bitcrackrandomiser](https://i.ibb.co/PgFkHSd/5.png)

# Run on Windows as Docker Image

<ins>NOTE: ONLY FOR NVIDIA DEVICES!</ins> The relevant docker image only builds for "cuda devices". For AMD and OpenCL devices, use the "BUILD_OPENCL=1" argument in the dockerfile. (or go to Long Way section)

[Download and install Docker](https://docs.docker.com/desktop/install/windows-install/) app

There are two Docker images you can use for Bitcrackrandomiser.

## Manual Launch

`ilkercndk/bitcrackrandomiser:latest` will be used for manual launch.

1 - Open a new Command Prompt console and write below code.

```
docker run --gpus all -it ilkercndk/bitcrackrandomiser:latest
```

2 - Press enter and bingo!

![docker console](https://i.ibb.co/kckRTwJ/adaad.png)

3 - Edit settings file. You can use following code to edit on console.

```
$ nano settings.txt
```

Or you can edit "settings.txt" file from Docker app in currently active container.

![docker edit settings file](https://i.ibb.co/L8kZQsk/sdff.png)

4 - Enter following command end press ENTER.

```
$ dotnet BitcrackRandomiser.dll
```

## Auto Launch

`ilkercndk/bitcrackrandomiser:autorun` will be used for auto launch.

1 - Open a new Command Prompt console and write below code.

```
docker run --gpus all -e BC_USER_TOKEN={your_user_token} -e BC_WALLET={your_wallet_address} ilkercndk/bitcrackrandomiser:autorun
```

You can use more settings for this docker image. See below;

```
-e BC_PUZZLE=66
-e BC_USER_TOKEN=0
-e BC_WALLET=1eosEvvesKV6C2ka4RDNZhmepm1TLFBtw
-e BC_APP=/app/BitCrack/bin/./cuBitCrack
-e BC_APP_ARGS="-b 896 -t 256 -p 256"
-e BC_SCAN_TYPE=includeDefeatedRanges
-e BC_CUSTOM_RANGE=none
-e BC_TELEGRAM_SHARE=false
-e BC_TELEGRAM_ACCESS_TOKEN=0
-e BC_TELEGRAM_CHAT_ID=0
-e BC_TELEGRAM_SHARE_EACHKEY=false
-e BC_UNTRUSTED_COMPUTER=false
```

2 - No more step. The application started automatically.

# Run on Vast.ai (Bitcoin accepted)

<ins>NOTE: ONLY FOR NVIDIA DEVICES!</ins> The relevant docker image only builds for "cuda devices". For AMD and OpenCL devices, use the "BUILD_OPENCL=1" argument in the dockerfile. (or go to Long Way section)

Register [Vast.ai](https://cloud.vast.ai/?ref=69296) to rent GPU(s). 

## Short Way

Use custom docker image `ilkercndk/bitcrackrandomiser:latest` from [dockerhub](https://hub.docker.com/r/ilkercndk/bitcrackrandomiser) in "Template Slot" and run any instance.

Connect instance. Vast.ai doesnt support `--it` interactive arguments for docker image on SSH. You must go to main folder;

```
$ cd /app/bitcrackrandomiser
```

Then edit `settings.txt` file and run the app.

```bash
$ dotnet BitcrackRandomiser.dll
```

(Optional) **Auto-start bitcrackrandomiser** on instance starts; enter your parameters to on-start script on vast.ai. You can use all the variables in the <ins>settings.txt</ins> file.

```
$ cd /app/bitcrackrandomiser
$ dotnet BitcrackRandomiser.dll app_path=/app/BitCrack/bin/./cuBitCrack app_arguments="-b 896 -t 256 -p 256" user_token=xxxx wallet_address=1eosEvvesKV6C2ka4RDNZhmepm1TLFBtw;
```

You can use env variables for docker entrypoint with `ilkercndk/bitcrackrandomiser:autorun` image:

```
-e BC_PUZZLE=66
-e BC_USER_TOKEN=0
-e BC_WALLET=1eosEvvesKV6C2ka4RDNZhmepm1TLFBtw
-e BC_APP=/app/BitCrack/bin/./cuBitCrack
-e BC_APP_ARGS="-b 896 -t 256 -p 256"
-e BC_SCAN_TYPE=includeDefeatedRanges
-e BC_CUSTOM_RANGE=none
-e BC_TELEGRAM_SHARE=false
-e BC_TELEGRAM_ACCESS_TOKEN=0
-e BC_TELEGRAM_CHAT_ID=0
-e BC_TELEGRAM_SHARE_EACHKEY=false
-e BC_UNTRUSTED_COMPUTER=false
```

Example docker create/run options

```
-e BC_USER_TOKEN=xxxx -e BC_WALLET=1eosEvvesKV6C2ka4RDNZhmepm1TLFBtw -e BC_SCAN_TYPE=default -e BC_UNTRUSTED_COMPUTER=true
```

## Long Way

(1) Select custom docker image `nvidia/cuda:12.0.0-devel-ubuntu20.04` in "Template Slot" and run any instance. (3090 or 4090 is fine)

(2) Connect instance via SSH client (Like PuTTY). Follow commands:

```bash
$ sudo apt install nano
$ wget https://dot.net/v1/dotnet-install.sh -O dotnet-install.sh
$ sudo chmod +x ./dotnet-install.sh
$ ./dotnet-install.sh --version latest
$ export DOTNET_ROOT=$HOME/.dotnet
$ export PATH=$PATH:$HOME/.dotnet:$HOME/.dotnet/tools
$ export DOTNET_SYSTEM_GLOBALIZATION_INVARIANT=1
$ git clone https://github.com/brichard19/BitCrack
$ cd BitCrack
$ make BUILD_CUDA=1 COMPUTE_CAP=86
$ git clone https://github.com/ilkerccom/bitcrackrandomiser
$ cd bitcrackrandomiser/BitcrackRandomiser
```

Edit settings.txt file. You can edit settings.txt file with `nano settings.txt` or connect with WinSCP and download-edit *settings.txt* file. Example below:

```
target_puzzle=66
app_type=bitcrack
app_path=/root/BitCrack/bin/./cuBitCrack
app_arguments=-b 896 -t 256 -p 256
user_token=xxxxxxxxxxxxxxxxxx
wallet_address=1eosEvvesKV6C2ka4RDNZhmepm1TLFBtw
... other settings
```

Finally, run the app.

```
$ dotnet run
```

You can press "<ins>CTRL+B</ins>" and then "<ins>D</ins>" to leave terminal without closing app.
