 ## Advancing 6G Studies by VIAVI Marconi Labs, VIAVI Solutions
### 1. ISAC as a Key 6G Capability
   - Core 6G Feature: ISAC is listed as one of the "New Capabilities" that 6G is expected to enable, alongside immersive experiences and AI services.
   - Testbed Support: The VIAVI 6G testbed is designed to test and validate ISAC algorithms. This involves enabling communication networks to function as intelligent sensors.
     
### 2. The Need for Ray Tracing
#### 2.1 Goal 
- Accurate channel modeling is crucial for planning, testing, and optimizing 5G-Advanced and 6G networks, as well as for digital twin applications.

- Problem with Stochastic Models: Traditional stochastic models use probabilistic distributions (e.g., K-factor, delay spread). This approach often fails to accurately capture the complex interactions of electromagnetic (EM) waves in dense urban environments or at high-frequency bands (cmWave, mmWave, THz).
- Ray Tracing as a Solution: Ray tracing is a deterministic channel modeling technique.
- It uses high-fidelity 3D digital maps with precise material information.
- It simulates all possible geometric rays (line-of-sight, reflection, diffraction, scattering) and computes their EM characteristics.
- This allows it to derive highly accurate channel metrics like path loss, coverage maps, and channel impulse responses for complex environments.

#### 2.2 VIAVI's Ray Tracing Technology
- Challenge: Ray tracing is computationally demanding, especially with large maps and moving objects.
- VIAVI's Solution: A near-real-time, GPU-accelerated ray tracing technology. It uses patented algorithms to reduce computational overhead while maintaining accuracy and scalability.
- Key Enabler: This technology is a key enabler for building various types of network digital twins, from RF-level to full RAN-level.

#### 2.3. Applications and Types of Digital Twins
The paper outlines a progression of digital twin capabilities:
- RF Digital Twin (Standalone)
  - Function: Provides realistic network planning and enhances network perception. 
  - Key Capability: It enables proactive radio resource management, mobility prediction, and link adaptation without relying on extensive measurement feedback from UEs.
  - Use Cases: Ideal for scenarios requiring ultra-reliable, low-latency performance (xURLLC), such as industrial automation and private 5G/6G.
  - Closed-Loop Operation: The digital twin can be kept in sync with the real world.
  - It can be dynamically updated with real-time sensing data to account for moving objects or environmental changes.
  - Example: A digital twin of Qualcomm's San Diego campus was used for optimized beam selection. The ray tracing computation (106 ms) was significantly faster than the UE sampling time (490 ms), enabling real-time beam prediction and reducing energy consumption.
 
- UE digital Twin
  - Components: This is created by combining VIAVI's ray tracing technology with the TM500 UE emulator.
  - Function: Creates a comprehensive, site-specific digital twin of UEs for testing network equipment (gNB, DU, RU) in a controlled lab setting.
  - Benefits:
    - Provides an immersive virtual environment for performance and capacity testing, which helps minimize post-deployment issues in the field.
    - Allows AI models for the air interface to be trained and tested using realistic, site-specific data.
    - Creates a "Field to Lab" bridge by synchronizing real subscriber data (connectivity, mobility, traffic patterns) from the field with the digital twin.
    - Operators can replay site-specific network issues (like traffic surges or handover ping-pong) in the lab to develop more robust optimization strategies.
- RAN Digital Twin
  - Components: This is a complete RAN digital twin created by integrating ray tracing with the TeraVM AI RAN Scenario Generator (RSG).
  - Function: Serves as a platform for AI training, testing, and post-deployment life-cycle management.
  - Key Application (O-RAN):
    - It is a "perfect sandbox" for training and testing xApps and rApps.
    - In intent-based management scenarios, the RIC (RAN Intelligent Controller) can use the digital twin to evaluate different RAN policies before deployment.
    - Example: An energy-saving policy (like cell activation/shutdown) can be verified in the digital twin to see its impact on KPIs (e.g., energy efficiency, UE throughput, QoE) using closed-loop optimization.
   
  <img width="1304" height="536" alt="image" src="https://github.com/user-attachments/assets/b5e9958f-703f-4377-b112-4db88f8c3a45" />
