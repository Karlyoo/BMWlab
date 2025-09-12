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

flow:
- Input: Test cases → REST API → Integration rApp.
- Process: rApp → Configuration service → configure DU/RU → initiate test.
- Output: Throughput results → Grafana Dashboard visualization.

# CH 3 Proposed Method
## 3.1 System Input Parameters for rApp Execution
