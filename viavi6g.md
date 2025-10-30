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

### 3. ISAC Channel Modeling
#### 3.1 sensing channel model
- Channel Model H(t): The channel is modeled as a sum of discrete components, or "taps"5.
- Mathematical Expression: 
- Channel Taps: These include reflections from both the target and background clutter7.
- parameters:
  - Path-gain (cp):The amplitude of the path-gain provides information about the Radar Cross Section (RCS) of an object.
  - Delay (tau_p):Delay provides information on the total path length of the signal11.It is used for estimating the position of targets.
     - Mono-static ISAC: Total path length is twice the distance to the target.
     - Bi-static ISAC: Total path length is the sum of the transmitter-to-target distance and the target-to-receiver distance.
   - Doppler shift (f(d,p)):This parameter provides information about the speed of the targets.
   - Angle-of-Arrival (AoA) & Angle-of-Departure (AoD):These angles also provide useful information about the target's position and speed.ISAC systems can localize targets by using a combination of delay and AoA/AoD information.

#### 3.2 ray tracing for sensing channel modelling
- VIAVI uses its ray tracing technology to create a digital replica of the test environment for accurate ISAC testing.
- Function: It emulates the time-varying channel in real-time by modeling all significant reflections from targets and background objects.
- Fidelity: The technology supports all reflected rays, including Line-of-Sight (LoS) and Non-Line-of-Sight (NLoS) paths for both targets and clutter.
- Use Case: A primary use case is investigating target identification performance by analyzing the reflected rays.
**Target Identification Methods (Section 4.2.2)**
- RCS Estimation:
  - The ISAC system measures the total power drop of the signal.
  - This drop has two components: path-loss from propagation and power drop from target reflection.
  - By estimating the path-loss (using the distance derived from delay), the system can isolate the RCS.
  - Different targets (e.g., drones, birds) have different RCS values, allowing the system to distinguish them.
- Speed Estimation:
  - The system estimates the Doppler shift to determine the target's speed.
  - This helps classify targets, as different objects have different typical speed ranges.
- Micro-Doppler Estimation:
  - This is used when a target has rotating parts, such as the blades of a drone or the moving arms of a pedestrian.
  - These rotating parts create a fluctuation in the Doppler shift over time.
  - An AI-based approach can be trained on a library of these micro-Doppler fluctuation signatures to achieve high performance in target classification (e.g., distinguishing a drone from a bird).
