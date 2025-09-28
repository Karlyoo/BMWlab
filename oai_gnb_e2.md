# Run OAI gNB with E2 Agent

## OAI RAN install
```
git clone -b 2024.w43 https://gitlab.eurecom.fr/oai/openairinterface5g.git
```
<img width="1447" height="591" alt="Screenshot from 2025-09-27 20-24-00" src="https://github.com/user-attachments/assets/02077e6e-87a6-4511-a39d-54ad6e36812b" />

it means that:
- Your program can see the contents of that commit (e.g., code, files, etc.).
- You can make changes and commit new changes, but these commits won't belong to any branch (unless you manually create a new branch).
- If you switch to a different branch, these new commits may be discarded (because they aren't referenced by any branch).

``
cd cmake_targets/
sudo ./build_oai -I -w SIMU --gNB --nrUE --build-e2 --ninja
``
- `-I`option is to install pre-requisites, you only need it the first time you build the softmodem or when some oai dependencies have changed.
- `-w` option is to select the radio head support you want to include in your build. Radio head support is provided via a shared library, which is called the "oai device" The build script creates a soft link from liboai_device.so to the true device which will be used at run-time (here the USRP one, liboai_usrpdevif.so). The RF simulatorRF simulator is implemented as a specific device replacing RF hardware, it can be specifically built using `-w` SIMU option, but is also built during any softmodem build.
- `--ninja` is to use the ninja build tool, which speeds up compilation.





