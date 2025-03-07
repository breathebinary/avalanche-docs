# Upgrade Your AvalancheGo Node

{% embed url="https://youtu.be/o4Fww-sHoaQ" caption="" %}

## **Backup your node**

Before upgrading your node, it is recommended you backup your staker files which are used to identify your node on the network. In the default installation, you can copy them by running following commands:

```text
cd
cp ~/.avalanchego/staking/staker.crt .
cp ~/.avalanchego/staking/staker.key .
```

Then download `staker.crt` and `staker.key` files and keep them somewhere safe and private. If anything happens to your node or the machine node runs on, these files can be used to fully recreate your node.

If you use your node for development purposes and have keystore users on your node, you should back up those too.

## Node installed using the installer script

If you installed your node using the [installer script](set-up-node-with-installer.md), to upgrade your node, just run the installer script again.

```text
./avalanchego-installer.sh
```

It will detect that you already have AvalancheGo installed:

```text
AvalancheGo installer
---------------------
Preparing environment...
Found 64bit Intel/AMD architecture...
Found AvalancheGo systemd service already installed, switching to upgrade mode.
Stopping service...
```

It will then upgrade your node to the latest version, and after it's done, start the node back up, and print out the information about the latest version:

```text
Node upgraded, starting service...
New node version:
avalanche/1.1.1 [network=mainnet, database=v1.0.0, commit=f76f1fd5f99736cf468413bbac158d6626f712d2]
Done!
```

And that is it, your node is upgraded to the latest version.

If you installed your node manually, proceed with the rest of the tutorial.

## **Stop the old node version**

After the backup is secured, you may start upgrading your node. Begin by stopping the currently running version.

### Node running from terminal

If your node is running in a terminal stop it by pressing `ctrl+c`.

### Node running as a service

If your node is running as a service, stop it by entering:

`sudo systemctl stop avalanchego.service`

\(your service may be named differently, `avalanche.service`, or similar\)

### Node running in background

If your node is running in the background \(by running with `nohup`, for example\) then find the process running the node by running `ps aux | grep avalanche`. This will produce output like:

```text
ubuntu  6834  0.0  0.0   2828   676 pts/1    S+   19:54   0:00 grep avalanche
ubuntu  2630 26.1  9.4 2459236 753316 ?      Sl   Dec02 1220:52 /home/ubuntu/build/avalanchego
```

In this example, second line shows information about your node. Note the process id, in this case, `2630`. Stop the node by running `kill -2 2630`.

Now we are ready to download the new version of the node. You can either download the source code and then build the binary program, or you can download the pre-built binary. You don’t need to do both.

Downloading pre-built binary is easier and recommended if you're just looking to run your own node and stake on it.

Building the node [from source](upgrade-your-avalanchego-node.md#build-from-source) is recommended if you're a developer looking to experiment and build on Avalanche.

## **Download Pre-built Binary**

If you want to download a pre-built binary instead of building it yourself, go to our [releases page](https://github.com/ava-labs/avalanchego/releases), and select the release you want \(probably the latest one.\)

Under `Assets`, select the appropriate file.

For MacOS:  
Download: `avalanchego-macos-<VERSION>.zip`  
Unzip: `unzip avalanchego-macos-<VERSION>.zip`  
The resulting folder, `avalanchego-<VERSION>`, contains the binaries.

For Linux on PCs or cloud providers:  
Download: `avalanchego-linux-amd64-<VERSION>.tar.gz`  
Unzip: `tar -xvf avalanchego-linux-amd64-<VERSION>.tar.gz`  
The resulting folder, `avalanchego-<VERSION>-linux`, contains the binaries.

For Linux on RaspberryPi4 or similar Arm64-based computers:  
Download: `avalanchego-linux-arm64-<VERSION>.tar.gz`  
Unzip: `tar -xvf avalanchego-linux-arm64-<VERSION>.tar.gz`  
The resulting folder, `avalanchego-<VERSION>-linux`, contains the binaries.

You are now ready to run the new version of the node.

### Running the node from terminal

If you are using the pre-built binaries on MacOS:

```cpp
./avalanchego-<VERSION>/build/avalanchego
```

If you are using the pre-built binaries on Linux:

```cpp
./avalanchego-<VERSION>-linux/avalanchego
```

Add `nohup` at the start of the command if you want to run the node in the background.

### Running the node as a service

If you're running the node as a service, you need to replace the old binaries with the new ones.

`cp -r avalanchego-<VERSION>-linux/* <DIRECTORY_WITH_OLD_BINARIES>`

and then restart the service with `sudo systemctl start avalanchego.service`.

## **Build from source**

First clone our Github repo \(you can skip this step if you’ve done this before\):

```text
git clone https://github.com/ava-labs/avalanchego.git
```

Then move to the avalanchego directory:

```text
cd avalanchego
```

Pull the latest code:

```text
git pull
```

NOTE: if the master branch has not been updated with the latest release tag, you can get to it directly via first running `git fetch --all --tags` and then `git checkout tags/<tag>` \(where `<tag>` is the latest release tag; for example `v1.3.2`\) instead of `git pull`. Note that your local copy will be in a 'detached HEAD' state, which is not an issue if you do not make changes to the source that you want push back to the repository \(in which case you should check out to a branch and to the ordinary merges\).

Check that your local code is up to date. Do:

```text
git rev-parse HEAD
```

and check that the first 7 characters printed match the Latest commit field on our [Github.](https://github.com/ava-labs/avalanchego)

NOTE: if you used the `git checkout tags/<tag>` then these first 7 characters should match commit hash of that tag.

Now build the binary:

```text
./scripts/build.sh
```

This should print:

```text
Build Successful
```

You can check what version you’re running by doing:

```text
./build/avalanchego --version
```

You can run your node with:

```text
./build/avalanchego
```

