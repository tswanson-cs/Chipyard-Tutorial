# Chipyard-Tutorial
This will be a step by step guide for building ucb-bar/chipyard.  After building the toolchain we will use the rocket chip generator to generate Verilog source for an FPGA.
- **Machine info:**
  - **OS**: Ubuntu 20.04
  - **RAM**: 8GB
  - **CPU**: i7-4720HQ
  - **SSD**: SAMSUNG 860 EVO 500GB

## Setting up your environment
This step will install all the necessary dependencies needed by Chipyard.  If your on a machine does not have sudo access this will not work.
1. Open a terminal.
    - > You can do this by right clicking the desktop and selecting open terminal.
    - ![alt text](https://github.com/Tswanson-CS/Chipyard-Tutorial/blob/master/screenshots/1_open_terminal.png?raw=true)
    
2. Navigate to a project directory that we can clone into.
    - > I usually use ~/projects
    - ``` cd <projects> ```
    - From this repo source [setup.sh](setup.sh)
    - ``` sudo bash setup.sh ```
## Cloning and Building Chipyard
This step will clone the Chipyard repo, init submodules and build the risc-v toolchain.
1. ```cd <projects>```
2. ```git clone https://github.com/ucb-bar/chipyard.git ```
  - ![alt text](https://github.com/Tswanson-CS/Chipyard-Tutorial/blob/master/screenshots/2_1_clone_done.png?raw=true)
3. ```cd chipyard```
4. ```./scripts/init-submodules-no-riscv-tools.sh```
    - >after complete you can type ```du -sh ../chipyard/``` it should be around 2.1G
  - ![alt text](https://github.com/Tswanson-CS/Chipyard-Tutorial/blob/master/screenshots/2_1_clone_done.png?raw=true)
5. ``````
