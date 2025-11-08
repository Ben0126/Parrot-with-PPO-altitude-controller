# Technical Documentation

This document provides detailed technical information about the Parrot Quadcopter PPO Altitude Controller system architecture, implementation details, and algorithms.

## Table of Contents

1. [System Architecture](#system-architecture)
2. [Control System Design](#control-system-design)
3. [Reinforcement Learning Implementation](#reinforcement-learning-implementation)
4. [Sensor Suite and State Estimation](#sensor-suite-and-state-estimation)
5. [Vehicle Dynamics Model](#vehicle-dynamics-model)
6. [Simulation Environment](#simulation-environment)
7. [Code Generation and Deployment](#code-generation-and-deployment)
8. [Configuration and Customization](#configuration-and-customization)

---

## System Architecture

### Overview

The system is built on MATLAB/Simulink and implements a complete flight control stack for Parrot minidromes. The architecture follows a modular design with clear separation between:

- **Vehicle Dynamics**: Physics-based quadcopter model
- **Sensor Simulation**: Realistic sensor models with noise and dynamics
- **State Estimation**: Kalman filters for robust state estimation
- **Control**: Multiple control strategies (PID, Fuzzy, RL)
- **Visualization**: Real-time 3D visualization and data logging

### System Block Diagram

```
┌─────────────────────────────────────────────────────────────────┐
│                        Command Input                              │
│         (Altitude, Pitch, Roll, Yaw Setpoints)                   │
└────────────────────┬──────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                   Flight Controller                              │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐         │
│  │ PID Control  │  │ Fuzzy Logic  │  │ PPO-RL Agent │         │
│  └──────────────┘  └──────────────┘  └──────────────┘         │
│           │                │                   │                 │
│           └────────────────┴───────────────────┘                 │
│                            │                                      │
│                    Motor Commands                                │
└────────────────────┬──────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                   Control Mixer                                  │
│           (Thrust, Yaw, Pitch, Roll → Motor 1-4)                │
└────────────────────┬──────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                  Vehicle Dynamics                                │
│  ┌──────────────────┐        ┌──────────────────┐              │
│  │ Linear Airframe  │   OR   │Nonlinear Airframe│              │
│  └──────────────────┘        └──────────────────┘              │
│                     6-DOF Rigid Body                             │
└────────────────────┬──────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Sensor Suite                                  │
│   IMU (Accel+Gyro) │ Sonar │ Optical Flow │ Barometer           │
└────────────────────┬──────────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────────┐
│                  State Estimator                                 │
│  Complementary Filter (Attitude) + Kalman Filters (Alt, Pos)    │
└────────────────────┬──────────────────────────────────────────────┘
                     │
                     └──────────────────────┐
                                            │ Feedback
                                            ▼
```

### File Organization

The project uses a hierarchical structure:

```
Root
├── Simulink Models (.slx)
│   ├── Main Models (parrotMinidroneHover.slx)
│   ├── Controller Subsystems
│   ├── Dynamics Libraries
│   └── Sensor Libraries
├── MATLAB Scripts (.m)
│   ├── Initialization (startVars.m)
│   ├── Configuration (tasks/*.m)
│   └── Utilities
├── Data Files (.mat, .fis)
│   ├── Trained Models
│   ├── Experimental Data
│   └── Fuzzy Controllers
└── Support Files
    ├── 3D Models (VRML)
    └── Project Resources
```

---

## Control System Design

### 1. PID Controller

Traditional Proportional-Integral-Derivative controller for baseline comparison.

#### Altitude Control Loop

```
e(t) = z_desired - z_actual
de/dt = -v_z

u(t) = Kp*e(t) + Ki*∫e(τ)dτ + Kd*de/dt
```

**Characteristics:**
- Requires manual tuning for each flight scenario
- Parameters must be retuned when target altitude changes
- Simple and computationally efficient
- Baseline for performance comparison

### 2. Fuzzy Logic Controller

Mamdani-type fuzzy inference system with rule-based control.

#### Fuzzy Sets

**Inputs:**
1. **eZ** (Altitude Error): [-∞, +∞] meters
2. **dZ** (Vertical Velocity): [-∞, +∞] m/s

**Output:**
1. **F** (Thrust Force): [0, max_thrust] N

**Membership Functions:**
- 7 fuzzy sets per input/output: {NB, NM, NS, ZE, PS, PM, PB}
  - NB: Negative Big
  - NM: Negative Medium
  - NS: Negative Small
  - ZE: Zero
  - PS: Positive Small
  - PM: Positive Medium
  - PB: Positive Big

**Rule Base:**
- Total of 49 rules (7×7 matrix)
- Example rule: "IF eZ is PS AND dZ is NS THEN F is PM"

**Implementation Files:**
- `test3.fis`, `test31.fis`, `test3plus.fis`, `test4.fis`

### 3. PPO Reinforcement Learning Controller

Proximal Policy Optimization with GPU acceleration.

#### RL Framework

**Agent Type:** PPO (Proximal Policy Optimization)

**Observation Space:**
- Altitude error: e_z = z_desired - z_current
- Vertical velocity: v_z
- Additional state variables (orientation, angular rates)

**Action Space:**
- Continuous thrust command: u ∈ [u_min, u_max]

**Reward Function:**

From experimental notes (Record.txt):

```matlab
% Altitude error component
r_altitude = exp(-(1/0.5 * e)^2)

% Velocity component
r_velocity = 0.1 * exp(-(1/1.0 * dz)^2)

% Total reward
r_total = 1.0 * r_altitude + 0.1 * r_velocity
```

**Penalty:**
- Episode termination if error exceeds threshold: e > 50 meters

**Training Configuration:**
- GPU acceleration enabled
- Experience replay buffer
- Policy and value function networks
- Clipped surrogate objective

---

## Reinforcement Learning Implementation

### PPO Algorithm Details

#### Policy Network Architecture

**Neural Network Structure:**
```
Input Layer (Observation) → Hidden Layers → Output Layer (Action)
```

**Key Hyperparameters:**
- Learning rate: Adaptive (not specified in files)
- Discount factor (γ): Typically 0.99
- GAE parameter (λ): Typically 0.95
- Clip ratio (ε): Typically 0.2
- Epochs per update: Multiple passes through experience buffer

#### Training Process

1. **Environment Setup** (`parrotPPOenv.mlx`)
   - Initialize Simulink model as RL environment
   - Define observation and action spaces
   - Configure reward function

2. **Agent Creation**
   - Instantiate PPO agent with neural network
   - Configure actor and critic networks
   - Set training options

3. **Training Loop**
   - Collect experiences through environment interaction
   - Compute advantages using GAE
   - Update policy using clipped objective
   - Update value function
   - Log training metrics

4. **Evaluation**
   - Test trained policy on various scenarios
   - Compare with PID baseline
   - Evaluate robustness to disturbances

### Advantages Over PID

1. **Adaptive Learning**: Learns optimal policy from experience
2. **No Manual Tuning**: Parameters learned automatically
3. **Generalization**: Single policy works across different altitudes
4. **Robustness**: Better handling of disturbances
5. **Optimal Control**: Approaches optimal control solution

---

## Sensor Suite and State Estimation

### Sensor Models

#### 1. IMU (Inertial Measurement Unit)

**Accelerometer:**
- Measurement range: ±50 m/s² (all axes)
- Natural frequency: 190 Hz
- Damping ratio: 0.707
- Scale factors: [1.00596, 1.00383, 0.99454]
- Bias: Calibrated from sensorCalibration.mat
- Noise power: ~0.015-0.022 (m/s²)²

**Gyroscope:**
- Measurement range: ±10 rad/s (all axes)
- Natural frequency: 190 Hz
- Damping ratio: 0.707
- Scale factors: [0.99861, 1.00644, 0.99997]
- Bias: Calibrated from sensorCalibration.mat
- Noise power: ~0.0006-0.0007 (rad/s)²

#### 2. Sonar Altimeter

- Minimum range: 0.44 m
- Noise power: 1.0
- Filtered using 3rd-order Butterworth filter (cutoff: 0.01 normalized)

#### 3. Barometer

- Air density: 1.225 kg/m³
- Conversion: Pressure = g × ρ × altitude + bias
- Bias: 101270.95 Pa
- Filtered using 3rd-order Butterworth filter

#### 4. Optical Flow Sensor

- Estimates horizontal velocity
- Gain (Mambo): 1.0
- Gain (Rolling Spider): 20.0
- Maximum operating altitude: -0.4 m (below ground)

### State Estimation

#### Attitude Estimation (Complementary Filter)

No longer actively used; replaced by Kalman filter.

**Legacy parameters:**
- Gyro update threshold: 0.002
- Accelerometer weight: 0.001
- Vision weight: 0.2

#### Attitude Estimation (Kalman Filter)

**IMU Kalman Filter:**

State vector: [θ, θ_bias]
- θ: Attitude angles (roll, pitch)
- θ_bias: Gyro bias

**Process noise:**
- Variance (accelerometer angle): 9×10⁻⁴ × 100 (normalized)
- Variance (gyro): 9.7344×10⁻⁸ (normalized)
- Variance (bias): var_gyro/1000 (normalized)

**Measurement:**
- Accelerometer-derived angles
- Threshold for innovation: 0.3

#### Altitude Estimation (Kalman Filter)

**State space model:**
```
A = [1  Ts]    G = [0]
    [0   1]        [1]

H = [1  0]

Q = 0.0005      (Process noise)
R = 0.1         (Measurement noise)
```

**Sensors fused:**
- Sonar (primary when in range)
- Barometer (backup)

**Outlier rejection:**
- Maximum sonar delta: 0.1 m
- Maximum pressure delta: 0.8 m
- Maximum filtered delta: 0.4 m

#### Position and Velocity Estimation

**Velocity Kalman Filter:**

State vector: [x, y, vx, vy]

```matlab
A = [1 0 -Ts  0 ]
    [0 1  0  -Ts]
    [0 0  1   0 ]
    [0 0  0   1 ]

B = [Ts  0]
    [0  Ts]
    [0   0]
    [0   0]

C = [1 0 0 0]
    [0 1 0 0]

Q = diag([0.09, 0.09, 0.01, 0.01])
R = 20 × I₂
```

**Inputs:**
- Optical flow measurements
- Accelerometer (with gain 0.2)

**Position Kalman Filter:**

Simple integrator with correction:

```matlab
Q = 0.001 × I₂
R = 0.3 × I₂
G = 0.1 × I₂
```

---

## Vehicle Dynamics Model

### Physical Parameters

#### Parrot Mambo

**Mass Properties:**
- Mass: 0.063 kg
- Inertia matrix: diag([5.829×10⁻⁵, 7.169×10⁻⁵, 1.0×10⁻⁴]) kg·m²

**Geometry:**
- Arm length (diagonal): 0.0624 m
- Arm length (x/y): 0.0624/√2 = 0.044 m
- Hub height: -0.015876 m

**Motors:**
- Command range: 10-500
- Command to ω² gain: 13840.8 (rad/s)²/command

**Takeoff:**
- Takeoff gain: 0.45 (45% above hover thrust)

#### Rolling Spider

**Mass Properties:**
- Mass: 0.068 kg
- Inertia matrix: diag([6.86×10⁻⁵, 9.2×10⁻⁵, 1.366×10⁻⁴]) kg·m²

**Other parameters:** Same as Mambo except:
- Takeoff gain: 0.2 (20% above hover thrust)
- Optical flow gain: 20.0

### Rotor Model

**Blade Properties:**
- Number of blades: 2
- Radius: 0.033 m
- Chord: 0.008 m
- Blade mass: 3.75×10⁻⁴ kg
- Blade inertia: m_blade × r²/4

**Aerodynamics:**
- Thrust coefficient (Ct): 0.0107
- Torque coefficient (Cq): Ct × √(Ct/2)
- Lift slope: 5.5
- Collective pitch (θ₀): 14.6°
- Tip pitch (θ_tip): 6.8°

**Thrust Model:**
```
T = Ct × ρ × A × r² × ω²
Q = Cq × ρ × A × r³ × ω²

where:
  ρ = air density (1.184 kg/m³)
  A = rotor disk area (π × r²)
  r = rotor radius
  ω = angular velocity (rad/s)
```

### Control Mixer

Transforms motor thrust commands to body forces and moments:

**Forward Mixing (Motors → Forces/Moments):**

```matlab
[F_total]   [1    1    1    1  ] [T₁]
[M_yaw  ] = [r_q -r_q  r_q -r_q] [T₂]
[M_pitch]   [-d  -d    d    d  ] [T₃]
[M_roll ]   [-d   d    d   -d  ] [T₄]

where:
  r_q = Cq/Ct × r (yaw moment arm)
  d = arm_length × √2/2
```

**Inverse Mixing (Forces/Moments → Motors):**

```matlab
Q2Ts = inv(Ts2Q)
```

**Saturation:**
- Maximum relative thrust: 92% (8% reserve for attitude control)
- Per-motor limits: 10-500 command units

### 6-DOF Dynamics

**Equations of Motion:**

Translational:
```
m × dV/dt = F_thrust + F_gravity + F_drag - m × Ω × V
```

Rotational:
```
I × dΩ/dt = M_control - Ω × (I × Ω)
```

**Variants:**
- **Linear Airframe**: Linearized about hover trim point
- **Nonlinear Airframe**: Full nonlinear dynamics with gyroscopic effects

### Environment Model

**Gravity:**
- g = 9.81 m/s²

**Air Properties:**
- Density: 1.184 kg/m³ (sea level, standard conditions)

**Initial Conditions:**
- Date: January 1, 2017, 00:00:00
- LLA: [42.299886°N, 71.350447°W, 71.3232 m]
- NED: [57, 95, -0.046] m
- Velocity: [0, 0, 0] m/s
- Euler angles: [0, 0, 0] rad
- Angular rates: [0, 0, 0] rad/s

---

## Simulation Environment

### Simulink Configuration

**Fixed-Step Solver:**
- Sampling time (Ts): 0.005 s (200 Hz)
- Simulation time: 100 s
- Solver: ode4 (Runge-Kutta 4th order)

**Vision Sampling:**
- Vision update rate: 40 × Ts = 0.2 s (5 Hz)

### Variant Subsystems

The model uses variant subsystems for flexible configuration:

#### VSS_COMMAND (Command Source)
- 0: Signal Builder (predefined trajectories)
- 1: Joystick (manual control)
- 2: Pre-saved MAT data
- 3: Spreadsheet (Excel) data

#### VSS_SENSORS (Sensor Dynamics)
- 0: Feedthrough (ideal sensors)
- 1: Dynamics (realistic sensor models)

#### VSS_ENVIRONMENT (Environment Model)
- 0: Constant (no wind/disturbances)
- 1: Variable (with disturbances)

#### VSS_VISUALIZATION (Display Mode)
- 0: Scopes (real-time plots)
- 1: Workspace (save to MATLAB workspace)
- 2: FlightGear (external 3D visualization)
- 3: Simulink 3D (built-in 3D animation)

#### VSS_VEHICLE (Dynamics Model)
- 0: Linear Airframe
- 1: Nonlinear Airframe

### Data Logging

**Logged Signals:**
- States: position, velocity, attitude, angular rates
- Commands: desired altitude, pitch, roll, yaw
- Sensor outputs: IMU, sonar, optical flow
- Control signals: motor commands, thrust, moments
- Estimator outputs: filtered states, innovations

**Storage:**
- MAT files in `rundata/` directory
- Typical file size: 17 MB (100s at 200 Hz with multiple signals)

---

## Code Generation and Deployment

### Target Hardware

**Supported Drones:**
- Parrot Mambo
- Parrot Rolling Spider

**Requirements:**
- MATLAB Coder
- Embedded Coder
- Simulink Coder
- PARROT Support Package (from Hardware Support Package Manager)

### Code Generation Scripts

#### 1. setPARROTCodeGen.m

Configures Simulink model for PARROT hardware:
- Sets target hardware
- Configures I/O interfaces
- Sets optimization options
- Prepares for embedded deployment

#### 2. setGRTCodeGen.m

Configures for Generic Real-Time (GRT) target:
- Desktop simulation
- Rapid prototyping
- HIL (Hardware-in-the-Loop) testing

#### 3. generateFlightCode.m

Generates deployable C code:
1. Validates model
2. Builds code
3. Creates executable
4. Packages for deployment

### Deployment Process

1. **Model Preparation:**
   ```matlab
   setMamboModel           % Select drone model
   setPARROTCodeGen        % Configure code generation
   ```

2. **Code Generation:**
   ```matlab
   generateFlightCode      % Build deployable code
   ```

3. **Deploy to Drone:**
   - Use PARROT Support Package
   - Connect via WiFi/Bluetooth
   - Upload generated code
   - Test flight

---

## Configuration and Customization

### Selecting Drone Model

**For Mambo:**
```matlab
setMamboModel
```
Updates `model` variable and reloads all parameters.

**For Rolling Spider:**
```matlab
setRollingSpiderModel
```

### Tuning Control Parameters

**PID Gains:**
Edit `tasks/controllerVars.m`:
```matlab
Controller.altKp = ...;  % Proportional gain
Controller.altKi = ...;  % Integral gain
Controller.altKd = ...;  % Derivative gain
```

**Fuzzy Logic:**
Edit .fis files using Fuzzy Logic Designer:
```matlab
fuzzyLogicDesigner('test3.fis')
```

**RL Controller:**
Retrain using `parrotPPOenv.mlx` with modified reward function.

### Adding Custom Disturbances

Modify `VSS_ENVIRONMENT = 1` variant to include:
- Wind gusts
- Sensor noise
- Actuator faults
- External forces

### Sensor Calibration

Update `mainModels/sensorCalibration.mat`:
```matlab
% Measure biases
accelBias = [...];
gyroBias = [...];

% Save
sensorCalibrationData = [accelBias; gyroBias];
save('sensorCalibration.mat', 'sensorCalibrationData');
```

### Custom Trajectories

**Using Signal Builder:**
1. Open `parrotMinidroneHover.slx`
2. Locate Signal Builder block
3. Edit trajectory points
4. Save model

**Using MAT file:**
1. Create data structure:
   ```matlab
   cmdData.time = [...];
   cmdData.altitude = [...];
   cmdData.yaw = [...];
   ```
2. Save: `save('cmdData.mat', 'cmdData')`
3. Set `VSS_COMMAND = 2`

**Using Excel:**
1. Create spreadsheet with columns: time, altitude, yaw, pitch, roll
2. Save as `cmdData.xlsx`
3. Set `VSS_COMMAND = 3`

---

## Performance Optimization

### GPU Acceleration

For RL training, enable GPU:
```matlab
% In parrotPPOenv.mlx
trainOpts.UseParallel = true;
trainOpts.UseDevice = 'gpu';
```

### Simulation Speed

**Faster simulation:**
- Use linear airframe (VSS_VEHICLE = 0)
- Disable 3D visualization
- Reduce logging signals
- Use fixed-step solver

**More accurate simulation:**
- Use nonlinear airframe (VSS_VEHICLE = 1)
- Enable sensor dynamics (VSS_SENSORS = 1)
- Smaller time step (reduce Ts)

---

## Troubleshooting

### Common Issues

1. **Model won't simulate:**
   - Ensure all toolboxes are installed
   - Run `startVars` to initialize
   - Check variant settings

2. **Poor control performance:**
   - Verify drone model selection
   - Check sensor calibration
   - Review control gains

3. **Code generation fails:**
   - Install required support packages
   - Check model for unsupported blocks
   - Review code generation settings

4. **RL training not converging:**
   - Adjust reward function
   - Increase training episodes
   - Modify network architecture
   - Check observation scaling

---

## References

### Original Work

This implementation builds upon:

1. **MathWorks Aerospace Blockset Examples**
   - Parrot Minidrone Competition

2. **Fabian Riether and Sertac Karaman**
   - Original dynamics implementation
   - See `support/LICENSE.md`

3. **Peter Corke**
   - Robotics Toolbox contributions
   - 6-DOF dynamics formulation

### Algorithms

1. **PPO:** Schulman et al., "Proximal Policy Optimization Algorithms," 2017
2. **Kalman Filtering:** R.E. Kalman, "A New Approach to Linear Filtering and Prediction Problems," 1960
3. **Fuzzy Logic:** Mamdani, "Application of fuzzy algorithms for control of simple dynamic plant," 1974

---

*For additional technical support, refer to the main README.md or contact the Intelligent Control Lab at National Taitung University.*
