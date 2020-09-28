# Chipyard-Tutorial
This will be a step by step guide for building ucb-bar/chipyard.  After building the toolchain we will use the rocket chip generator to generate Verilog source for an FPGA.
- **Machine info:**
  - **OS**: Ubuntu 20.04
  - **RAM**: 8GB
  - **CPU**: i7-4720HQ
  - **SSD**: SAMSUNG 860 EVO 500GB

## Setting up your environment
This step will install all the necessary dependencies needed by Chipyard.  If on a machine that does not have sudo access this will not work.
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
    - > If this has an error that Make cannot be found try setting ```export MAKE=`which make` ```
    - ![alt text](https://github.com/Tswanson-CS/Chipyard-Tutorial/blob/master/screenshots/2_1_clone_done.png?raw=true)
3. ```cd chipyard```
4. ```./scripts/init-submodules-no-riscv-tools.sh```
    - >after complete you can type ```du -sh ../chipyard/``` it should be around 2.1G
    - ![alt text](https://github.com/Tswanson-CS/Chipyard-Tutorial/blob/master/screenshots/3_1_submodule.png?raw=true)
5. ```export MAKEFLAGS=-j8```
6. ```./scripts/build-toolchains.sh riscv-tools```
    - ![alt text](https://github.com/Tswanson-CS/Chipyard-Tutorial/blob/master/screenshots/4_0_0build_toolchain.png?raw=true)
    - >Build should finish after a few hours...  However this will depend on the machine
    - ![alt text](https://github.com/Tswanson-CS/Chipyard-Tutorial/blob/master/screenshots/4_1_1_finish_build.png?raw=true)
7. ```source env.sh``` generated from the step above
## Building a rocket SOC using rocket-chip generator
This step will give basic info on how to build Verilog source using the rocket-chip generator.
We will also generate the FPGA source.  In the current Chipyard there is a bug when generating the FPGA source.  This patch will be included in the tutorial until UCB fixs the bug...
1. ```cd generators/rocket-chip/```
2. ```vim src/main/scala/system/Configs.scala```
    - >This will open up the file for editing
    - > locate the line below and change Orig to New
      - Orig: ```class BaseFPGAConfig extends Config(new BaseConfig)```
      - New:  ```class BaseFPGAConfig extends Config(new BaseConfig ++ new WithCoherentBusTopology)```
        - ![alt text](https://github.com/Tswanson-CS/Chipyard-Tutorial/blob/master/screenshots/6_2_bug_fix_fpga.png?raw=true)
3. ```cd vsim```
4. ```export JVM_MEMORY=6G``` >change 6G to reflect the memory on your system.  6GB = 6G ....
5. ```make -j8 CONFIG=freechips.rocketchip.system.DefaultFPGAConfig```
    - ![alt text](https://github.com/Tswanson-CS/Chipyard-Tutorial/blob/master/screenshots/6_4_done_building_fpga.png?raw=true)
    - Source will be inside the generated-src directory
    - ![alt text](https://github.com/Tswanson-CS/Chipyard-Tutorial/blob/master/screenshots/6_5_done_generating_fpga.png?raw=true)
6.  If you would like to generate a larger rocket chip you can modify the Config.scale
  - ```vim ../src/main/scala/system/Configs.scala```
  - >This will open up the file for editing
  - In this file many different flavours of rocket chip.  I would suggest you review the Chipyard documentation.  However we will start with generating a rocket chip with 16 big cores and the BaseFPGAConfig.
  - At the end of the file insert ```class BigFPGAConfig extends Config(new WithNBigCores(16) ++ new BaseFPGAConfig)```
  - ![alt text](https://github.com/Tswanson-CS/Chipyard-Tutorial/blob/master/screenshots/7_0_nbigcores.png?raw=true)
  - > Before proceeding: >If you want to keep the previous generated source you can ```mv generated-src DefaultFPGAConfig```
7. ```make -j8 CONFIG=freechips.rocketchip.system.BigFPGAConfig```
8. ![alt text](https://github.com/Tswanson-CS/Chipyard-Tutorial/blob/master/screenshots/7_2_done_bigfpga.png?raw=true)
