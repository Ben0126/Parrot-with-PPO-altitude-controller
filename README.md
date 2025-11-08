# Reinforcement Learning Controller for Parrot Quadcopter Altitude Control

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![MATLAB](https://img.shields.io/badge/MATLAB-R2021a+-orange.svg)](https://www.mathworks.com/products/matlab.html)
[![Platform](https://img.shields.io/badge/Platform-Parrot%20Minidrone-blue.svg)](https://www.parrot.com/)

A research project implementing **Proximal Policy Optimization (PPO)** reinforcement learning algorithm for altitude control of Parrot minidromes (Mambo and Rolling Spider). This project demonstrates superior control performance compared to traditional PID controllers, especially in environments with random disturbances.

## 🎓 Research Institution

**Intelligent Control Lab.**
National Taitung University, Taitung, Taiwan

## 📝 Overview

Most quadcopters adopt Proportional-Integral-Derivative (PID) controllers that require time-consuming empirical tuning of parameters. Moreover, these parameters need to be redesigned and readjusted for various flight environments to achieve optimal control performance. This research demonstrates that reinforcement learning can address these limitations by learning optimal control policies through continuous feedback.

### Key Features

- 🤖 **PPO-based RL Controller**: GPU-accelerated training with Proximal Policy Optimization
- 🎯 **Superior Performance**: Outperforms well-tuned PID controllers in both transient and steady-state responses
- 🌪️ **Robust to Disturbances**: Excellent environmental adaptability with random external disturbances
- 🚁 **Dual Drone Support**: Compatible with Parrot Mambo and Rolling Spider minidromes
- 📊 **Complete Simulation Environment**: Built on MATLAB/Simulink with Aerospace Blockset
- 🔧 **Hardware Deployment Ready**: Code generation for actual drone deployment

## 🏆 Research Highlights

This project achieved:
- ✅ Better control performance than PID in various target altitudes
- ✅ Enhanced robustness in flight environments with random disturbances
- ✅ Demonstrated environmental adaptability without parameter retuning
- ✅ Published in multiple international conferences (see [Publications](#publications))

## 📊 Publications

This research has been published in the following conferences:

1. **Chun-Hung Liu**, Shun-Pin Yeh, Yu-Chien Wang, Wei-Lin Lai, Guan-Yu Luo, Shang-Chi Shen, Ze-An Ding, Li-Ming Chu, "Performance Evaluation of Proximal Policy Optimization Algorithm in Controlling Quadcopters," *IEEE International Symposium on Computer, Consumer and Control (IEEE-IS3C 2023)*, 1311, Taichung, Taiwan, Apr. 2023.

2. **Chun-Hung Liu**, Shun-Pin Yeh, Yu-Chien Wang, Wei-Lin Lai, Guan-Yu Luo, Li-Ming Chu, "Analysis of Various Control Strategies for Indoor Quadcopter Applications," *2023 Conference of Research and Development in Technology Education (2023 CRDTE)*, N059, Oral presentation, Kaohsiung, Taiwan, Apr. 29, 2023.

3. **Chun-Hung Liu**, Wei-Lin Lai, Shang-Chi Shen, Shun-Pin Yeh, Po-Cheng Wu, Yu-Chien Wang, and Guan-Yu Luo, "Impact of control strategies on altitude control in indoor quadcopter," *2022 IET International Conference on Engineering Technologies and Applications (IET ICETA 2022)*, 5416, Poster presentation, Changhua, Taiwan, Oct. 2022.

**See [PUBLICATIONS.md](PUBLICATIONS.md) for detailed publication information.**

## 🚀 Quick Start

### Prerequisites

- MATLAB R2021a or later
- Required Toolboxes:
  - Aerospace Blockset
  - Reinforcement Learning Toolbox
  - Simulink Control Design
  - Embedded Coder (for hardware deployment)
  - Fuzzy Logic Toolbox
  - Computer Vision Toolbox
- (Optional) PARROT Support Package for hardware deployment
- (Recommended) CUDA-capable GPU for accelerated training

### Installation

1. Clone the repository:
```bash
git clone https://github.com/Ben0126/Parrot-with-PPO-altitude-controller.git
cd Parrot-with-PPO-altitude-controller
```

2. Open MATLAB and navigate to the project directory

3. Open the project:
```matlab
open parrotMinidroneHover.prj
```

4. The project will automatically initialize all required variables and paths

### Running Simulations

#### Basic Simulation

1. Open the main model:
```matlab
open_system('mainModels/parrotMinidroneHover.slx')
```

2. Run the simulation:
```matlab
sim('parrotMinidroneHover')
```

#### Training PPO Controller

1. Open the PPO environment script:
```matlab
open('parrotPPOenv.mlx')
```

2. Execute the live script to train the RL agent (requires GPU for optimal performance)

#### Selecting Drone Model

For Parrot Mambo:
```matlab
setMamboModel
```

For Rolling Spider:
```matlab
setRollingSpiderModel
```

### Configuration

The project uses variant subsystems controlled by these flags in `utilities/startVars.m`:

- `VSS_COMMAND`: Command input source (0: Signal builder, 1: Joystick, 2: Pre-saved data, 3: Spreadsheet)
- `VSS_SENSORS`: Sensor dynamics (0: Feedthrough, 1: Dynamics)
- `VSS_ENVIRONMENT`: Environment model (0: Constant, 1: Variable)
- `VSS_VISUALIZATION`: Visualization mode (0: Scopes, 1: Workspace, 2: FlightGear, 3: Simulink 3D)
- `VSS_VEHICLE`: Airframe model (0: Linear, 1: Nonlinear)

## 📁 Project Structure

```
├── controller/              # Flight control system models
│   └── flightControlSystem.slx
├── libraries/               # Reusable dynamics and environment libraries
│   ├── dynamicsLibrary.slx
│   └── environmentLibrary.slx
├── linearAirframe/          # Linear airframe models
│   ├── linearAirframe.slx
│   └── trimNonlinearAirframe.slx
├── nonlinearAirframe/       # Nonlinear airframe dynamics
│   └── nonlinearAirframe.slx
├── mainModels/              # Main simulation models
│   └── parrotMinidroneHover.slx
├── tasks/                   # Configuration scripts
│   ├── vehicleVars.m        # Physical properties
│   ├── controllerVars.m     # Controller parameters
│   ├── sensorsVars.m        # Sensor configurations
│   ├── estimatorVars.m      # State estimator settings
│   └── commandVars.m        # Command inputs
├── utilities/               # Helper functions
│   ├── startVars.m          # Main initialization
│   ├── setMamboModel.m      # Mambo configuration
│   └── setRollingSpiderModel.m  # Rolling Spider configuration
├── rundata/                 # Experimental data and trained models
├── support/                 # 3D models and visualization assets
├── parrotPPOenv.mlx        # PPO training environment
├── *.fis                   # Fuzzy logic controllers
└── README.md               # This file
```

## 🎮 Control Architecture

The project includes multiple control strategies:

1. **PID Controller**: Traditional proportional-integral-derivative control
2. **Fuzzy Logic Controller**: Rule-based fuzzy inference system
3. **PPO-RL Controller**: Reinforcement learning with Proximal Policy Optimization

### Sensor Suite

- **IMU**: 3-axis accelerometer and gyroscope
- **Sonar**: Altitude measurement
- **Optical Flow**: Velocity estimation
- **Barometer**: Pressure-based altitude (optional)

### State Estimation

- Complementary filter for attitude estimation
- Kalman filter for altitude and position estimation
- Velocity estimation from optical flow

## 🔬 Technical Details

### PPO Algorithm

The Proximal Policy Optimization algorithm is used for training the RL controller with the following characteristics:

- **Observation Space**: Altitude error, vertical velocity, and drone state
- **Action Space**: Continuous thrust command
- **Reward Function**: Exponential function based on altitude error and velocity
- **Training**: GPU-accelerated using MATLAB's Reinforcement Learning Toolbox

### Drone Specifications

**Parrot Mambo:**
- Mass: 0.063 kg
- Inertia: diag([0.0000582857, 0.0000716914, 0.0001]) kg·m²
- Rotor radius: 0.033 m

**Rolling Spider:**
- Mass: 0.068 kg
- Inertia: diag([0.0686e-3, 0.092e-3, 0.1366e-3]) kg·m²
- Rotor radius: 0.033 m

### Simulation Parameters

- **Sampling Rate**: 200 Hz (Ts = 0.005s)
- **Simulation Time**: 100s
- **Initial Position**: [42.299886°N, -71.350447°W, 71.3232m]
- **Air Density**: 1.184 kg/m³
- **Gravity**: 9.81 m/s²

## 📈 Results

The RL controller demonstrates superior performance compared to PID:

- **Better transient response**: Faster settling time and less overshoot
- **Better steady-state response**: Smaller steady-state error
- **Enhanced robustness**: Superior performance under random disturbances
- **No parameter retuning needed**: Adapts to different altitudes without adjustment

**See [RESULTS.md](RESULTS.md) for detailed experimental results and performance comparisons.**

## 🛠️ Hardware Deployment

To generate code for actual Parrot drone:

1. Set up code generation:
```matlab
setPARROTCodeGen
```

2. Generate flight code:
```matlab
generateFlightCode
```

3. Deploy to drone using PARROT Support Package

## 🧪 Testing

Run all project tests:
```matlab
runProjectTests
```

## 📚 Documentation

- [TECHNICAL_DOCUMENTATION.md](TECHNICAL_DOCUMENTATION.md) - Detailed technical architecture
- [PUBLICATIONS.md](PUBLICATIONS.md) - Complete list of publications and abstracts
- [RESULTS.md](RESULTS.md) - Experimental results and analysis

## 🤝 Contributing

This is a research project. If you use this code for your research, please cite our publications:

```bibtex
@inproceedings{liu2023performance,
  title={Performance Evaluation of Proximal Policy Optimization Algorithm in Controlling Quadcopters},
  author={Liu, Chun-Hung and Yeh, Shun-Pin and Wang, Yu-Chien and Lai, Wei-Lin and Luo, Guan-Yu and Shen, Shang-Chi and Ding, Ze-An and Chu, Li-Ming},
  booktitle={IEEE International Symposium on Computer, Consumer and Control (IEEE-IS3C 2023)},
  pages={1311},
  year={2023},
  address={Taichung, Taiwan}
}
```

## 👥 Authors

- **Chun-Hung Liu** - Principal Investigator
- **Shun-Pin Yeh** (Ben Yeh) - Lead Developer
- **Yu-Chien Wang**
- **Wei-Lin Lai**
- **Guan-Yu Luo**
- **Shang-Chi Shen**
- **Ze-An Ding**
- **Li-Ming Chu**

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

Copyright (c) 2023 Ben Yeh

## 🙏 Acknowledgments

- This project builds upon MathWorks' Parrot Minidrone example
- Original dynamics model derived from work by Fabian Riether and Sertac Karaman
- Supported by the Intelligent Control Lab at National Taitung University

## 📧 Contact

For questions or collaborations, please contact:
- Intelligent Control Lab, National Taitung University, Taiwan

## 🔗 Related Links

- [MathWorks Aerospace Blockset](https://www.mathworks.com/products/aerospace-blockset.html)
- [MATLAB Reinforcement Learning Toolbox](https://www.mathworks.com/products/reinforcement-learning.html)
- [Parrot Developer](https://developer.parrot.com/)

---

**⭐ If you find this project useful for your research, please consider starring the repository and citing our publications!**
