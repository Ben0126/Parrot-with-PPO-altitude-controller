# Publications

This page lists all publications related to this research project on reinforcement learning control for quadcopters.

## 📚 Conference Papers

### 2023

#### 1. Performance Evaluation of Proximal Policy Optimization Algorithm in Controlling Quadcopters

**Authors:** Chun-Hung Liu*, Shun-Pin Yeh, Yu-Chien Wang, Wei-Lin Lai, Guan-Yu Luo, Shang-Chi Shen, Ze-An Ding, Li-Ming Chu

**Conference:** IEEE International Symposium on Computer, Consumer and Control (IEEE-IS3C 2023)

**Paper ID:** 1311

**Location:** Taichung, Taiwan

**Date:** April 2023

**Presentation Type:** Accepted

**Abstract:**
This paper presents a comprehensive performance evaluation of the Proximal Policy Optimization (PPO) algorithm applied to quadcopter altitude control. The study compares the PPO-based reinforcement learning controller against traditional PID controllers across various flight scenarios and environmental conditions. Results demonstrate that the RL controller achieves superior performance in both transient and steady-state responses, particularly when subjected to random disturbances.

---

#### 2. Analysis of Various Control Strategies for Indoor Quadcopter Applications

**Authors:** Chun-Hung Liu*, Shun-Pin Yeh, Yu-Chien Wang, Wei-Lin Lai, Guan-Yu Luo, Li-Ming Chu*

**Conference:** 2023 Conference of Research and Development in Technology Education (2023 CRDTE)

**Paper ID:** N059

**Location:** Kaohsiung, Taiwan

**Date:** April 29, 2023

**Presentation Type:** Oral presentation

**Abstract:**
This paper analyzes multiple control strategies specifically designed for indoor quadcopter applications. The study investigates PID, fuzzy logic, and reinforcement learning controllers, evaluating their performance in constrained indoor environments where GPS is unavailable. The analysis considers factors such as computational complexity, real-time performance, robustness to sensor noise, and adaptability to changing conditions.

---

### 2022

#### 3. Impact of Control Strategies on Altitude Control in Indoor Quadcopter

**Authors:** Chun-Hung Liu*, Wei-Lin Lai, Shang-Chi Shen, Shun-Pin Yeh, Po-Cheng Wu, Yu-Chien Wang, and Guan-Yu Luo

**Conference:** 2022 IET International Conference on Engineering Technologies and Applications (IET ICETA 2022)

**Paper ID:** 5416

**Location:** Changhua, Taiwan

**Date:** October 2022

**Presentation Type:** Poster presentation

**Abstract:**
This study investigates the impact of different control strategies on altitude control performance for indoor quadcopters. The research focuses on comparing conventional PID control with emerging reinforcement learning approaches, examining their effectiveness in maintaining stable altitude under various disturbance conditions. The findings provide insights into the advantages and limitations of each control strategy for practical indoor applications.

---

## 📖 Main Research Paper

### Design of Reinforcement Learning Controller for Quadcopter in Flight Environment with Random Disturbance

**Authors:** Chun-Hung Liu², Shun-Pin Yeh¹, Yu-Chien Wang¹, Wei-Lin Lai¹, Shang-Chi Shen¹, Ze-An Ding¹, Li-Ming Chu¹*

**Affiliations:**
- ¹ Intelligent Control Lab., National Taitung University, Taitung, Taiwan
- ² (If different affiliation, please specify)

**Abstract:**

Most quadrotors adopt Proportional-Integral-Derivative (PID) controllers, that requires time-consuming empirical tuning of PID parameters. Moreover, the parameters need to be redesigned and readjusted for various flight environments to achieve optimal control performance, especially when desired altitude is changed. Recently, some studies have demonstrated that reinforcement learning (RL) in the field of machine learning can address highly complex problems for the system. RL adopts continuous learning through feedback of different errors under the same flight environment to obtain the optimal decision for the system.

Therefore, this study focuses on developing RL technology and applying it to an attitude control of quadcopters. In addition, a Proximal Policy Optimization (PPO) algorithm is used to design a RL controller for the quadcopter to enhance control performance for various flight environments. The performance of the RL controller is compared to that of the PID controller in various target altitudes without and with adding external random disturbances.

The simulation results indicate that the RL controller performs better than the PID controller in terms of control performance, including transient and steady-state responses. This study preliminarily concluded that as compared with the well-tuned PID controller, this well-trained RL controller is able to have better environmental adaptability and control capability in various flight environments.

**Keywords:** Quadcopter, PID controller, Reinforcement Learning Controller, Proximal Policy Optimization Algorithm, Flight Environment with Random Disturbance

---

## 📊 Research Impact

### Key Contributions

1. **Novel Application of PPO**: First comprehensive application of Proximal Policy Optimization to Parrot minidrone altitude control

2. **Comparative Analysis**: Rigorous comparison between RL and traditional PID controllers under realistic conditions

3. **Robustness Evaluation**: Systematic evaluation of controller performance under random disturbances

4. **Practical Implementation**: Complete implementation ready for hardware deployment on actual Parrot minidromes

### Research Significance

- Demonstrates the practical applicability of deep reinforcement learning to real-world drone control
- Provides evidence that RL controllers can outperform well-tuned classical controllers
- Addresses the parameter tuning challenge inherent in PID controllers
- Contributes to the growing field of learning-based control for autonomous systems

---

## 🎯 Citation

If you use this work in your research, please cite:

### BibTeX Format

```bibtex
@inproceedings{liu2023performance,
  title={Performance Evaluation of Proximal Policy Optimization Algorithm in Controlling Quadcopters},
  author={Liu, Chun-Hung and Yeh, Shun-Pin and Wang, Yu-Chien and Lai, Wei-Lin and Luo, Guan-Yu and Shen, Shang-Chi and Ding, Ze-An and Chu, Li-Ming},
  booktitle={IEEE International Symposium on Computer, Consumer and Control (IEEE-IS3C 2023)},
  pages={1311},
  year={2023},
  address={Taichung, Taiwan}
}

@inproceedings{liu2023analysis,
  title={Analysis of Various Control Strategies for Indoor Quadcopter Applications},
  author={Liu, Chun-Hung and Yeh, Shun-Pin and Wang, Yu-Chien and Lai, Wei-Lin and Luo, Guan-Yu and Chu, Li-Ming},
  booktitle={2023 Conference of Research and Development in Technology Education (2023 CRDTE)},
  pages={N059},
  year={2023},
  address={Kaohsiung, Taiwan}
}

@inproceedings{liu2022impact,
  title={Impact of control strategies on altitude control in indoor quadcopter},
  author={Liu, Chun-Hung and Lai, Wei-Lin and Shen, Shang-Chi and Yeh, Shun-Pin and Wu, Po-Cheng and Wang, Yu-Chien and Luo, Guan-Yu},
  booktitle={2022 IET International Conference on Engineering Technologies and Applications (IET ICETA 2022)},
  pages={5416},
  year={2022},
  address={Changhua, Taiwan}
}
```

### IEEE Format

[1] C.-H. Liu, S.-P. Yeh, Y.-C. Wang, W.-L. Lai, G.-Y. Luo, S.-C. Shen, Z.-A. Ding, and L.-M. Chu, "Performance Evaluation of Proximal Policy Optimization Algorithm in Controlling Quadcopters," in *IEEE International Symposium on Computer, Consumer and Control (IEEE-IS3C 2023)*, Taichung, Taiwan, Apr. 2023, p. 1311.

[2] C.-H. Liu, S.-P. Yeh, Y.-C. Wang, W.-L. Lai, G.-Y. Luo, and L.-M. Chu, "Analysis of Various Control Strategies for Indoor Quadcopter Applications," in *2023 Conference of Research and Development in Technology Education (2023 CRDTE)*, Kaohsiung, Taiwan, Apr. 29, 2023, p. N059.

[3] C.-H. Liu, W.-L. Lai, S.-C. Shen, S.-P. Yeh, P.-C. Wu, Y.-C. Wang, and G.-Y. Luo, "Impact of control strategies on altitude control in indoor quadcopter," in *2022 IET International Conference on Engineering Technologies and Applications (IET ICETA 2022)*, Changhua, Taiwan, Oct. 2022, p. 5416.

---

## 🔬 Research Lab

**Intelligent Control Lab**
Department of Electrical Engineering
National Taitung University
Taitung, Taiwan

### Research Focus Areas

- Reinforcement Learning for Control Systems
- Autonomous Aerial Vehicles
- Intelligent Control Theory
- Machine Learning Applications in Robotics
- Adaptive and Robust Control

---

## 📧 Contact for Academic Inquiries

For questions about this research or potential collaborations:

- **Lab:** Intelligent Control Lab, National Taitung University
- **Location:** Taitung, Taiwan

---

*Last Updated: 2023*
