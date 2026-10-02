# Parrot Minidrone PPO Altitude Controller (MATLAB/Simulink, simulation only)

Undergraduate research code from the Intelligent Control Lab, National Taitung University (2022–2023).
A PPO reinforcement-learning altitude controller for the Parrot minidrone, trained and evaluated
**in simulation only**. The results are reported in the papers listed below; this repository does not
contain evaluation logs.

## What this repository is

- **Base model.** MathWorks' *Parrot Minidrone Hover* example (Aerospace Blockset): airframe, sensor and
  environment models, the flight-control project structure, and the files under `tasks/`,
  `utilities/`, `support/`, `resources/`, `libraries/`, `linearAirframe/` and `nonlinearAirframe/`.
  Those files keep their original MathWorks copyright.
- **What was added for the research** (in `controller/flightControlSystem.slx`, subsystem
  *Altitude Controller*):
  - an **RL Agent** block (MATLAB Reinforcement Learning Toolbox, PPO) that adds a thrust correction
    on top of the model's gravity feed-forward term;
  - switchable baseline controllers: PID, GA-tuned PID and Mamdani fuzzy logic (`test*.fis`);
  - IAE / ISE / ITAE logging of the altitude response.

## My part

I designed the reinforcement-learning formulation of the altitude controller:

- **Observation (input):** 2-D — altitude error and vertical velocity.
- **Action (output):** 1-D thrust correction, bounded to ±10, added to the gravity feed-forward.
- **Reward function:** exponential shaping of the altitude error and the vertical velocity, with early
  termination when the error grows too large. `Record.txt` is my log of the reward variants I tried.
- **Networks** (training script in the companion repo
  [`Parrot-with-PPO-Controller`](https://github.com/Ben0126/Parrot-with-PPO-Controller),
  `CreateParrotEnvironmantAndTrainAgent.mlx`): actor 2-32-64-64-16-2, critic 2-32-64-64-8-1, ReLU.

The papers are co-authored (author lists below).

## Publications

- C.H. Liu, **S.P. Yeh**, Y.C. Wang, W.L. Lai, S.C. Shen, Z.A. Ding, L.M. Chu*, "Design of Reinforcement
  Learning Controller for Quadcopter in Flight Environment with Random Disturbance," *Green Science &
  Technology Journal*, Vol. 13, No. 1, pp. 45–54, May 2023.
- C.H. Liu*, **S.P. Yeh**, Y.C. Wang, W.L. Lai, G.Y. Luo, S.C. Shen, Z.A. Ding, L.M. Chu, "Performance
  Evaluation of Proximal Policy Optimization Algorithm in Controlling Quadcopters," *IEEE International
  Symposium on Computer, Consumer and Control (IS3C)*, Taichung, Taiwan, Apr. 2023.

Related lab work on classical control strategies is listed in [PUBLICATIONS.md](PUBLICATIONS.md).

## Running it

Requirements: MATLAB R2021a or later with Simulink, Aerospace Blockset, Reinforcement Learning Toolbox,
Fuzzy Logic Toolbox and Simulink Control Design (plus whatever the MathWorks Parrot hover example needs on
your release).

```matlab
open parrotMinidroneHover.prj                       % sets up paths and variables (utilities/startVars.m)
open_system('mainModels/parrotMinidroneHover.slx')
sim('parrotMinidroneHover')
```

- Choose the airframe with `setMamboModel` or `setRollingSpiderModel` (in `tasks/`).
- Variant flags (`VSS_COMMAND`, `VSS_SENSORS`, `VSS_ENVIRONMENT`) are set in `utilities/startVars.m`.
- The controller used in the *Altitude Controller* subsystem is selected inside
  `controller/flightControlSystem.slx`.
- The `.mat` files at the top level and in `rundata/` are saved agents and run data from the
  experiments; which file produced which figure in the papers is not recorded here.

## License

`LICENSE` (MIT) applies only to the files I added or changed. The MathWorks example files keep their
original copyright and license terms.
