# Publications

All are co-authored work from the Intelligent Control Lab, National Taitung University. Simulation only.

## Reinforcement learning (this repository)

1. C.H. Liu, **S.P. Yeh**, Y.C. Wang, W.L. Lai, S.C. Shen, Z.A. Ding, L.M. Chu*, "Design of Reinforcement
   Learning Controller for Quadcopter in Flight Environment with Random Disturbance," *Green Science &
   Technology Journal* (ISSN 2223-6961), Vol. 13, No. 1, pp. 45–54, May 2023.

   **Abstract.** Most quadrotors adopt Proportional-Integral-Derivative (PID) controllers, that requires
   time-consuming empirical tuning of PID parameters. Moreover, the parameters need to be redesigned and
   readjusted for various flight environments to achieve optimal control performance, especially when
   desired altitude is changed. Recently, some studies have demonstrated that reinforcement learning (RL)
   in the field of machine learning can address highly complex problems for the system. RL adopts
   continuous learning through feedback of different errors under the same flight environment to obtain
   the optimal decision for the system. Therefore, this study focuses on developing RL technology and
   applying it to an attitude control of quadcopters. In addition, a Proximal Policy Optimization (PPO)
   algorithm is used to design a RL controller for the quadcopter to enhance control performance for
   various flight environments. The performance of the RL controller is compared to that of the PID
   controller in various target altitudes without and with adding external random disturbances. The
   simulation results indicate that the RL controller performs better than the PID controller in terms of
   control performance, including transient and steady-state responses. This study preliminarily concluded
   that as compared with the well-tuned PID controller, this well-trained RL controller is able to have
   better environmental adaptability and control capability in various flight environments.

2. C.H. Liu*, **S.P. Yeh**, Y.C. Wang, W.L. Lai, G.Y. Luo, S.C. Shen, Z.A. Ding, L.M. Chu, "Performance
   Evaluation of Proximal Policy Optimization Algorithm in Controlling Quadcopters," *IEEE International
   Symposium on Computer, Consumer and Control (IEEE-IS3C)*, Taichung, Taiwan, Apr. 2023 (paper ID 1311).

## Related: classical control strategies

3. C.H. Liu*, **S.P. Yeh**, Y.C. Wang, W.L. Lai, G.Y. Luo, L.M. Chu*, "Analysis of Various Control
   Strategies for Indoor Quadcopter Applications," *Conference of Research and Development in Technology
   Education (CRDTE)*, Kaohsiung, Taiwan, Apr. 2023. Oral presentation.
4. C.H. Liu*, W.L. Lai, S.C. Shen, **S.P. Yeh**, P.C. Wu, Y.C. Wang, G.Y. Luo, "Impact of control
   strategies on altitude control in indoor quadcopter," *IET International Conference on Engineering
   Technologies and Applications (IET ICETA)*, Changhua, Taiwan, Oct. 2022. Poster presentation.

## BibTeX (RL papers)

```bibtex
@article{liu2023design,
  title   = {Design of Reinforcement Learning Controller for Quadcopter in Flight Environment with Random Disturbance},
  author  = {Liu, C.H. and Yeh, S.P. and Wang, Y.C. and Lai, W.L. and Shen, S.C. and Ding, Z.A. and Chu, L.M.},
  journal = {Green Science \& Technology Journal},
  volume  = {13}, number = {1}, pages = {45--54}, year = {2023}
}
@inproceedings{liu2023performance,
  title     = {Performance Evaluation of Proximal Policy Optimization Algorithm in Controlling Quadcopters},
  author    = {Liu, C.H. and Yeh, S.P. and Wang, Y.C. and Lai, W.L. and Luo, G.Y. and Shen, S.C. and Ding, Z.A. and Chu, L.M.},
  booktitle = {IEEE International Symposium on Computer, Consumer and Control (IS3C)},
  address   = {Taichung, Taiwan}, year = {2023}, note = {Paper ID 1311}
}
```
