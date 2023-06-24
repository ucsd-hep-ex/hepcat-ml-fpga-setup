# hepcat-ml-fpga-setup

## 0. Prereqs

Install some prerequisites.
```bash
sudo apt install libtinfo5 git curl build-essential
```

## 1. Vivado

Download Vivado 2020.1.
- Archive: https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/vivado-design-tools/archive.html
- Direct download link: https://www.xilinx.com/member/forms/download/xef.html?filename=Xilinx_Unified_2020.1_0602_1208_Lin64.bin

Following instructions from https://danielmangum.com/posts/vivado-2020-x-ubuntu-20-04/, we need to "spoof" 18.04 by changing one line in `/etc/os-version` from
```
VERSION="20.04.6 LTS (Focal Fossa)"
```
to
```
VERSION="18.04.4 LTS (Bionic Beaver)"
```
This can be done as follows:
```bash
sudo cp /etc/os-version /etc/os-version.bak
sudo sed -i 's/VERSION="20.04.6 LTS (Focal Fossa)"/VERSION="18.04.4 LTS (Bionic Beaver)"/g' /etc/os-version
```

Then we can install Vitis/Vivado by opening a terminal and running

```bash
cd ~/Downloads
chmod +x Xilinx_Unified_2020.1_0602_1208_Lin64.bin && sudo ./Xilinx_Unified_2020.1_0602_1208_Lin64.bin
```

You will need to log in with a Xilinx username/password.

Keep all the default settings and press continue until it starts downloading/installing.

After installing Vivado, change `/etc/os-version/` back to how it was!

```bash
sudo cp /etc/os-version.bak /etc/os-version
```

Patch Vivado for the y2k22 bug.
Download `y2k22_patch-1.2.zip` from  https://support.xilinx.com/s/article/76960?language=en_US.
Then follow these instructions
```bash
sudo su
unzip y2k22_patch-1.2.zip -d /tools/Xilinx/ && rm y2k22_patch-1.2.zip
cd /tools/Xilinx
export LD_LIBRARY_PATH=$PWD/Vivado/2020.1/tps/lnx64/python-2.7.16/lib/
Vivado/2020.1/tps/lnx64/python-2.7.16/bin/python2.7 y2k22_patch/patch.py
exit
```
  

## 2. PYNQ-Z2 board files

Now install the PYNQ-Z2 board files.
```bash
git clone https://github.com/Xilinx/XilinxBoardStore/
sudo cp -r XilinxBoardStore/boards/TUL/pynq-z2 /tools/Xilinx/Vivado/2020.1/data/boards/board_files/
rm -rf XilinxBoardStore
```

## 3. Micromamba

Install `micromamba` following https://mamba.readthedocs.io/en/latest/installation.html

```bash
curl micro.mamba.pm/install.sh | bash
```

## 4. Python environment

```bash
git clone https://github.com/fastmachinelearning/hls4ml-tutorial
cd hls4ml-tutorial
micromamba create -f environment.yml
```


## 5. Set up (each time you log in)

```bash
source /tools/Xilinx/Vivado/2020.1/settings.sh

export XILINXD_LICENSE_FILE=2100@cselm2.ucsd.edu
export LM_LICENSE_FILE=2100@cselm2.ucsd.edu

micromamba activate hls4ml-tutorial
```

## 6. Set up the PYNQ-Z2 board

This can be done as part of notebook 7a or before starting the tutorial.
Instructions are here: https://pynq.readthedocs.io/en/latest/getting_started/pynq_z2_setup.html.
In particular, connect the PYNQ-Z2 board to a power source via the USB cable.
Insert the SD card on the board.
Make sure the jumpers are as indicated in the directions.
Then power on the board.
Wait for a minute or so until all the lights blink a few times.

Next, make sure the laptop can get on the WiFi network.
Then, you can connect the PYNQ-Z2 board to the laptop via the ethernet cable.
Set up a static IP address for the laptop's ethernet port in network settings.
Use the IP address 192.168.2.1 and subnet mask 255.255.255.0. 
Note we may have to use different static IP addresses (1, 2, 3, 4, etc.)
The PYNQ board, by default, will try to use 192.168.2.99.

Check everything is working by navigating your browser to http://192.168.2.99.
You should see a JupyterHub running from the PYNQ-Z2 board.
The username and password are both `xilinx`.

## 7. Run the tutorial

Run the hls4ml tutorial. Specifically, do parts 1, 4, 7a, 7b, and 7c.
