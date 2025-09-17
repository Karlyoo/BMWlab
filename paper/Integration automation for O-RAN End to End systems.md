# CH1 Introduction
## 1.1 O-RAN E2E Systems
<img width="749" height="373" alt="image" src="https://github.com/user-attachments/assets/b97a5f54-7531-4f6f-a170-23ceeac4dd43" />
<img width="788" height="688" alt="image" src="https://github.com/user-attachments/assets/7c1ec5b5-586d-47dd-acce-68f6ffa27d11" />

- The communication between  CN(Core Netwok) and CU is NG Application Protocal.
- The communication between  DU and RU is Fronthaul interface, which can devide into 4 plane:
   - S-Plane: providing time and frequency synchronization between RU & DU.
   - C-Plane: DU deliver control signal(WHERE OR WHEN) to RU.()
   - U-Plane: send the actual user data.Additionally,it also transport PRACH to randomly find the nearby UE.
   - M-Plane: Handling the fault management in RU. Typically enabling SMO & DU remotly manage and monitor the RU.
## 1.2 Timing Window
- We need to focus on the timing window parameters between DU and RU.
  - On the RU side,the time parameter is defined based on Hardware.
  - We need to adjust the DU's parameters to match the RU's.
    <img width="842" height="448" alt="image" src="https://github.com/user-attachments/assets/183efd47-b5c1-4b41-a37f-266ac4e5dcbf" />
<img width="836" height="696" alt="image" src="https://github.com/user-attachments/assets/5f45e844-254c-4eff-9521-2397fd025862" />

## 1.3 E2E testing flow
<img width="712" height="1218" alt="image" src="https://github.com/user-attachments/assets/18b7e749-584c-459d-ac7d-ec55de540318" />

## 1.4 DU/RU Configuration parameters
### 1.4.1 fronthaul
- DU & RU communicate over Ethenet,which need **MAC addres** and **VLAN IDs**.
- MTU(maximun Transmittion Unit) difine the largest packet size that can be transmitt without fragmentation.
- The IQ sample is large that may occupy a large amount of bandwidth.So we need to configure **compression method**.
<img width="1040" height="714" alt="image" src="https://github.com/user-attachments/assets/35c10e4e-3ff3-49dd-8042-c337e8f341ef" />

### 1.4.2 frequency
- carrier bandwidth:define bandwidth of channel
- absolute frequency point A:at the lower edge of carrier bandwidth serving the reference point for subcarrier indexing. 
- center of channel bandwidth:at the center of bandwidth
- absolute frequency SSB(synchronization signal block): define the location of SSB
- subcarrier spacing:define the space between adjcent subcarriers.
- initial bandwidth part:define the RB range.
 <img width="906" height="662" alt="image" src="https://github.com/user-attachments/assets/bbb0a5ae-01bf-4dc4-8e55-519f0449103d" />
 
# CH2 Architecture
<img width="641" height="802" alt="image" src="https://github.com/user-attachments/assets/4483c9cf-635e-4a1a-8c41-880864353e5d" />

rAPP:RAN Application,runs in Non-RT RIC (Non-Real-Time RAN Intelligent Controller). Doesn't require real-time control, but focuses on strategy, configuration, learning, and optimization.

xAPP:in Near-RT RIC. communicate with rAPP in A1 plane.
flow:
- Input: Test cases → REST API → Integration rApp.
- Process: rApp → Configuration service → configure DU/RU → initiate test.
- Output: Throughput results → Grafana Dashboard visualization.

# CH 3 Proposed Method
## 3.1 System Input Parameters for rApp Execution
<img width="1420" height="1096" alt="image" src="https://github.com/user-attachments/assets/c4acccb3-4e7f-447d-8769-515b9ff0219a" />

- User need provide 3 things to excute the rAPP:
   - gNB Configuration
   - Test Configuration
   - Host Information
<img width="980" height="600" alt="image" src="https://github.com/user-attachments/assets/f4b5dd1f-05eb-41b3-a1ce-1aaa347f2074" />

## 3.2 Automatic Configuration  mapping of gNB & RU
### 3.2.1 Repetable parameters in DU/RU
Some configuration parameters are required to applied to DU & RU consistently.
->The integration rAPP can automatically parse this input and propagate to DU & RU.
### 3.2.2 DU/RU MAC and VLAN Adjustment
- In tradition,retrieving and configuring the MAC address of RU is **logging in terminal, executing commands and writing the value into the configuration file**.
  - It's automated in this system.
- For DU,involve 4 steps:
  - 1. identify the NIC(Network Interface Card)
    2. create a VF(Virtual Function) on NIC and assign it a specific MAC address and VLAN tag.
    3. retrieve the VF's bus information,bind it to DPSK(Data Plane Development Kit)
    4. fill the imformation into OAI gNB configuration file.
<img width="1064" height="796" alt="image" src="https://github.com/user-attachments/assets/6a753e94-40ea-4bcc-8009-7baa58deff56" />

  - PF (Physical Function): Physical function of the NIC, representing actual hardware resources.
  - VF (Virtual Function): Virtual function carved out of the PF, which can be assigned a MAC address and VLAN.
  - Bus Info: Identification information of the VF on the PCI bus, which is later provided to the DPDK for binding.
  - DPDK (Data Plane Development Kit): High-performance packet processing framework required by gNBs to accelerate fronthaul traffic.

- In automated solution,we only provide the NIC interface name.
<img width="900" height="890" alt="image" src="https://github.com/user-attachments/assets/3406641c-ed76-457c-ad87-13eeba67e4df" />

### 3.2.3 MIMO Adjustment
<img width="828" height="1034" alt="image" src="https://github.com/user-attachments/assets/7f0d5bc0-71d0-431c-a8d6-658ad8c99e38" />

### 3.2.4 Frequency Adjustment
- **SubCarrier Spacing (SCS) Conversion**
	- Input: subCarrierSpacing (kHz) → converted to:
		- Numerology (0, 1, 2, 3)
		- SCS identifier string (scs) for DU & RU config
- **Bandwidth Conversion**
   - Input: bSChannelBw (MHz) → converted to Resource Blocks (RBs) based on SCS,using NRB mapping table (3GPP TS 38.101-1 Table 5.3.2-1).
   - The result RB count can be used to configure DU's `carrierBandwidth`
- **SSB Frequency Selection**
   - input:arfcn (Absolute Radio Frequency Channel Number)
    <img width="711" height="387" alt="image" src="https://github.com/user-attachments/assets/2ffa8747-04aa-470c-be1e-c6c71592b180" />
   
- **RU Center Frequency Calculation**
   - Input: number of PRBs (number-of-prb) instead of ARFCN.
   <img width="942" height="240" alt="image" src="https://github.com/user-attachments/assets/a5531801-036f-44b9-aa1c-6194cac4fc80" />

- **BWP (Bandwidth Part) Location and Size**
   - Encode the initial BWP location & size  format using RIV(Resource Indication Value) encoding.
    <img width="782" height="342" alt="image" src="https://github.com/user-attachments/assets/f24014d5-992c-4925-8393-02cfde1d3a77" />

    <img width="658" height="682" alt="image" src="https://github.com/user-attachments/assets/3dd59780-c23e-4ffd-a367-f5c9cb0a7b19" />

    <img width="712" height="474" alt="image" src="https://github.com/user-attachments/assets/27d33ec8-f795-4b67-9aca-2558fe4cbb74" />

## 3.3 Start E2E Component
Each step is monitored and verified by automation rAPP.
<img width="1444" height="1132" alt="image" src="https://github.com/user-attachments/assets/4cabb634-4f17-4e82-a0c9-31b211c1816f" />

<img width="1120" height="766" alt="image" src="https://github.com/user-attachments/assets/7de1ec14-ccfb-4fd6-9bb4-9b425d8dd69f" />

- DU/RU PTP Sync Check
  - rApp queries PTP sync state from gNB (DU).
  - rApp queries RU via M-plane for PTP status.
  - Results stored in Topology Exposure & Inventory Overview (TEIV).
  - rAPP checks PTP states.
* Topology Exposure & Inventory Overview: in OAI O1,
  - Topology Exposure: Provides visibility of the current network topology, including:
    - Relationships between O-RUs, O-DUs, and O-CUs
    - Transport network connectivity
    - Node locations and operational status
  - Inventory Overview : Maintains and exposes the resource inventory of the network, such as:
    - Hardware resources (CPU, memory, radio units)
    - Software versions and capabilities
    - Configuration parameters
    - Functional features supported by each node

<img width="1238" height="894" alt="image" src="https://github.com/user-attachments/assets/e1bcf37f-02a7-43d0-adac-a8687f16bc4c" />

- Capture NG Setup PCAP
  - rApp uses tcpdump to capture NG interface packets.
  - Stores them as PCAP files.
- NGAP Message Exchange
  - gNB → CN: NGSetupRequest
  - CN → gNB: NGSetupResponse
  - rApp analyzes PCAP with pyshark to verify proper NGAP exchange.
* tcpdump: tool can capture the packet data and result PCAP files.
* pyshark: a ppython wrapper for tshark. Reading,analyzing PCAP files or packet with Python.
  =>Integration of automated analysis, reporting, and testing processes.
