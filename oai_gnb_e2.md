# Run OAI gNB with e2

## OAI RAN install
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
- `-I`option is to install pre-requisites, you only need it the first time you build the softmodem or when some oai dependencies have changed.
- `-w` option is to select the radio head support you want to include in your build. Radio head support is provided via a shared library, which is called the "oai device" The build script creates a soft link from liboai_device.so to the true device which will be used at run-time (here the USRP one, liboai_usrpdevif.so). The RF simulatorRF simulator is implemented as a specific device replacing RF hardware, it can be specifically built using `-w` SIMU option, but is also built during any softmodem build.
- `--build` e2 option is to use the E2 agent, integrated within RAN
- `--ninja` is to use the ninja build tool, which speeds up compilation.

## run gnb
start the gNB-mono
```
cd oai/cmake_targets/ran_build/build
sudo ./nr-softmodem -O ../../../targets/PROJECTS/GENERIC-NR-5GC/CONF/gnb.sa.band78.fr1.106PRB.usrpb210.conf --rfsim --sa -E
```

## run nrUE and connect to gnb
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







