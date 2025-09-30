# Near RT RIC
Reference: 

[Summary of OSC [I&K] Near-RT RIC && OAI gNB](https://ntust-bmwlab.notion.site/Summary-of-OSC-I-K-Near-RT-RIC-OAI-gNB-14a1009831438013af8aee6f056f75a2) by Tobby

[openairinterface5g/openair2/E2AP/README.md](https://gitlab.eurecom.fr/oai/openairinterface5g/-/blob/v2.1.0/openair2/E2AP/README.md)
```
+-------------------+       +-------------------+       +-------------------+
|   OAI gNB/CU/DU   | <---> |   E2 Agent        | <---> |   Near-RT RIC     |
|                   |       | (E2AP / E2SM)     |       | (xApps: KPM, RSM) |
|-------------------|  E2   | - Monitors KPIs   |  E2   | - AI Optimization |
|   OAI UEs         | Intf. | - Applies Controls| Intf. | - Slicing Policies|
+-------------------+       +-------------------+       +-------------------+
         |                                                      
         |                          
         v
+-------------------+                                        
|   OAI Core (5GC)  |
+-------------------+                                       
```
## E2AP
### Build the FlexRIC (Near-Real-Time RIC)
FlexRIC (Flexible RAN Intelligent Controller),supporting the E2 interface (E2AP protocol) and various service models (SMs) such as KPM (Key Performance Measurement), enabling xApps (extensible applications) for RAN optimization, AI/ML inference, and performance monitoring.

```
# Download the required dependencies
sudo apt install libsctp-dev python3.8 cmake-curses-gui libpcre2-dev python-dev
### If python3.8 is not available from your current package repository sources. 
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get install python3.8
### If python-dev is not available from your current package repository sources. 
### The python-dev package has been replaced by python3-dev for Python 3. 
sudo apt-get update
sudo apt-get install python3-dev

sudo apt install libsctp-dev python3.8 cmake-curses-gui libpcre2-dev python3-dev

# Install prerequisites: SWIG (at least v.4.1).
cd
git clone https://github.com/swig/swig.git
cd swig
git checkout release-4.1 
./autogen.sh
./configure --prefix=/usr/
make -j8
sudo make install
swig -version

# Clone the FlexRIC repository
cd
git clone https://gitlab.eurecom.fr/mosaic5g/flexric flexric
cd flexric/
git checkout f1c08ed2b9b1eceeda7941dd7bf435db0168dd56
# Build FlexRIC
mkdir build && cd build && cmake .. && make -j8
### gcc-11 is not supported in FlexRIC
sudo apt update -y
sudo apt upgrade -y
sudo apt install -y build-essential
sudo apt install -y gcc-10 g++-10 cpp-10
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-10 100 --slave /usr/bin/g++ g++ /usr/bin/g++-10 --slave /usr/bin/gcov gcov /usr/bin/gcov-10
sudo update-alternatives --config gcc # chose gcc-10

# Installation of Service Models (SMs)
sudo make install
```

`sm_dir` is a critical directory path in FlexRIC, typically defaulting to `/usr/local/lib/flexric/`.

It stores the compiled service model shared libraries, which implement O-RAN-specified SMs (e.g., ORAN-E2SM-KPM, GTP_STATS) for handling communication, data collection, and control logic between E2 nodes.


### OAI RAN install
```
git clone -b 2024.w43 https://gitlab.eurecom.fr/oai/openairinterface5g.git
```
<img width="1447" height="591" alt="Screenshot from 2025-09-27 20-24-00" src="https://github.com/user-attachments/assets/02077e6e-87a6-4511-a39d-54ad6e36812b" width="50" />

it means that:
- Your program can see the contents of that commit (e.g., code, files, etc.).
- You can make changes and commit new changes, but these commits won't belong to any branch (unless you manually create a new branch).
- If you switch to a different branch, these new commits may be discarded (because they aren't referenced by any branch).

```
cd openairinterface5g/cmake_targets/
sudo ./build_oai -I -w SIMU --gNB --nrUE --build-e2 --ninja

cd openairinterface5g/cmake_targets/ran_build/build
sudo ninja nr-softmodem nr-uesoftmodem dfts ldpc params_libconfig rfsimulator
```
-  `-I`option is to instructs the build script to perform an installation step after compilation, copying the built binaries and libraries to the appropriate system directories.
- `-w SIMU` is to specifies the waveform or simulation mode. `SIMU` indicates a simulation environment, likely using the OAI softmodem in simulation mode (e.g., with rfsimulator) rather than hardware-based RF.
- `--build` e2 option is to use the E2 agent, integrated within RAN
- `--ninja` is to use the ninja build tool, which speeds up compilation.

### Run OAI gNB with e2
```
e2_agent = {
  near_ric_ip_addr = "127.0.0.1";
  #sm_dir = "/path/where/the/SMs/are/located/"
  sm_dir = "/usr/local/lib/flexric/"
};
```

**/openairinterface5g/targets/PROJECTS/GENERIC-NR-5GC/CONF/gnb.sa.band78.fr1.106PRB.usrpb210.conf**

To configure E2 agent,we need add or revise the following block in the OAI’s configuration file.


```
%% start the gNB-mono
cd oai/cmake_targets/ran_build/build
sudo ./nr-softmodem -O ../../../targets/PROJECTS/GENERIC-NR-5GC/CONF/gnb.sa.band78.fr1.106PRB.usrpb210.conf --rfsim --sa -E
```

### run nrUE and connect to gnb
```
cd oai/cmake_targets/ran_build/build
sudo ./nr-uesoftmodem -r 106 --numerology 1 --band 78 -C 3619200000 --rfsim --sa --uicc0.imsi 001010000000001 --rfsimulator.serveraddr 127.0.0.1
```
<img width="2381" height="691" alt="Screenshot from 2025-09-28 16-31-10" src="https://github.com/user-attachments/assets/d9b56a88-6835-41c7-a4a8-085c82c0b7db" />

**log of OAI nrUE** 

<img width="2385" height="602" alt="image" src="https://github.com/user-attachments/assets/b3796e7c-8e44-4028-aeb0-381ed5c6e46f" />

**Log of OAI gNB with e2**

To fix this,add a `` -ssb 516 `` in the UE command.

```
sudo ./nr-uesoftmodem -r 106 --numerology 1 --band 78 -C 3619200000 --ssb 516 -E --rfsim --sa --uicc0.imsi 001010000000001 --rfsimulator.serveraddr 127.0.0.1
```

```
Slot offset K2 (2) needs to be higher than DURATION_RX_TO_TX (3). Please set min_rxtxtime at least to 3 in gNB config file or gNBs.[0].min_rxtxtime=3 via command line.
```
**log of OAI nrUE** 

The error indicates that the Slot offset K2 (set to 2) needs to be greater than DURATION_RX_TO_TX (set to 3). This is because K2 represents the slot offset from receiving downlink data to transmitting uplink data, while DURATION_RX_TO_TX is the minimum time interval required between reception and transmission. If K2 is less than DURATION_RX_TO_TX, it can cause scheduling conflicts.

SO we can adjust it in config file:

<img width="1615" height="640" alt="Screenshot from 2025-09-30 17-57-55" src="https://github.com/user-attachments/assets/c3299629-043e-4c40-a6df-6de9f146d00c" />

**/openairinterface5g/targets/PROJECTS/GENERIC-NR-5GC/CONF/gnb.sa.band78.fr1.106PRB.usrpb210.conf**

<img width="2482" height="1381" alt="Screenshot from 2025-09-28 17-32-38" src="https://github.com/user-attachments/assets/69518416-5835-459f-b753-03c0c0fb0b18" />

**log of gNB**

<img width="2422" height="1428" alt="Screenshot from 2025-09-28 23-35-16" src="https://github.com/user-attachments/assets/3dda303c-77bd-4bb9-95a3-99a462b76998" />

**log of nrUE**

NEXT: empoly CN5G BY [Run OAI gNB & CN5G on different machines](https://ntust-bmwlab.notion.site/Run-OAI-gNB-CN5G-on-different-machines-15710098314380cd9cddfda1e2ecd185)

E2SM-KPM [OSC [I] Near-RT RIC && OAI gNB E2SM_KPM](https://ntust-bmwlab.notion.site/OSC-I-Near-RT-RIC-OAI-gNB-E2SM_KPM-13f10098314380c293edf8b9d5b87f4a)

[Welcome to O-RAN SC L Release Documentation Home](https://docs.o-ran-sc.org/en/latest/)






