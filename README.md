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
cd  ~/Downloads
chmod +x Xilinx_Unified_2020.1_0602_1208_Lin64.bin && sudo ./Xilinx_Unified_2020.1_0602_1208_Lin64.bin
```

You will need to log in with a Xilinx username/password. Here are my credentials:
- Username: `jduarte@ucsd.edu`
- Password: `yi6thet@VED`

Keep all the default settings and press continue until it starts downloading/installing.

After installing Vivado, change `/etc/os-version/` back to how it was!

```bash
sudo cp /etc/os-version.bak /etc/os-version
```

## 2. PYNQ-Z2 board files

Now install PYNQ-Z2 board files.
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


## N+1. Set up (each time you log in)

```bash
source /tools/Xilinx/Vivado/2020.1/setttings.sh

export XILINXD_LICENSE_FILE=2100@cselm2.ucsd.edu
export LM_LICENSE_FILE=2100@cselm2.ucsd.edu

micromamba activate hls4ml-tutorial
```
