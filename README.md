# RheoAgent: A Cross-Rheological Material Handling Robotic Manipulation System Based on Hierarchical Decision-Making Framework

[[Project page]](https://aqua12138.github.io/RheoAgentWebsite/)


Haixu Zhang<sup>1,2,3</sup>,
Bo Zhang<sup>3</sup>,
Danyang Zhang<sup>1,2</sup>,
Xi Chen<sup>4</sup>,
Wenqiang Lai<sup>1,2</sup>,
Hongxue Huang<sup>1,2</sup>,
Chi Zhang<sup>5</sup>,
Haixin Liu<sup>4</sup>,,
Tin Lun Lam<sup>1,2</sup>,,
Hu Huang<sup>6</sup>,,
Yuan Gao<sup>1,2</sup>,

<sup>1</sup>The Chinese University of Hong Kong, Shenzhen,
<sup>2</sup>Shenzhen Institute of Artificial Intelligence and Robotics for Society,
<sup>3</sup>Shenzhen University,
<sup>4</sup>Beijing Institute for General Artificial Intelligence,
<sup>5</sup>Sun Yat-Sen University,
<sup>6</sup>University of Science and Technology of China


<div align="center">
    <img src="assets/method_nmi.png" width="80%">
</div>



## Introduction
RheoAgent is a hierarchical decision-making robotic manipulation system for cross-rheological material handling to tackle the robotics manipulation challenges
posed by diverse rheological material scenarios. It establishes a new framework for intelligent rheological material handling, with applications that extend to industrial automation, laboratory robotics, and assistive technologies.

## Installation
### Ml-agents
Please follow the official tutorial to install mlagent==0.28.0:

[ML-Agents Official Installation Guide](https://github.com/Unity-Technologies/ml-agents/blob/release_19_docs/docs/Installation.md)

### Fluidlab
Please follow the official tutorial to install fluidlab:
[Fluidlab](https://github.com/zhouxian/FluidLab)


## Real Robot
### Hardware :
Robotic Manipulators:
* 1x UR5 or UR7e (MoveIt 2 Servo interface is required)
* 1x RealSense D405
* 1x RealSense D435i
* 1x RealSense L515
* 1x Elephant Mercury B1 (for heterogeneous multi-agent coordination)
* 1x 3D printed plate-type end-effector
* 1x 3D printed cup-type end-effector
* USB-C cables and screws for RealSense
* ArUco marker (for calibration)
* 1x NVIDIA RTX 3090 Ti GPU

Software:
* Ubuntu 22.04
* Unity
* RealSense SDK

## Benchmark Quick Reproduction Guide
- Pre-training
  - Prepare the Unity ML-Agents initial environment by following the [official tutorial](https://github.com/Unity-Technologies/ml-agents), with additional perception dependencies from [mbaske/grid-sensor](https://github.com/mbaske/grid-sensor) for specific tasks (e.g., pour, gather).
  - Get pre-trained model

    ```bash
    #demo
    conda activate mlagents
    cd ./ml-agents/ml-agents/mlagents/trainers
    python learn.py ~/RheoAgent/external/ml-agents/config/poca/Fluidlab.yaml --run-id=pour --num--envs=10 --base-port=5004 --force --env=/your/env/name --no-graphics
    ```
- Fine-tuning
  - Get fine-tuned model
    ```bash
    #demo
    conda activate fluidlab
    cd ./RheoMARS
    python run.py --cfg_file configs/shac/pour.yaml --rl shac --exp_name=water --perc_type sensor --pre_train_model pour_policy.pt --horizon 500 --material WATER
    ```

    - **pre_train_model**: Use the initial model obtained in [Pre-training](#Pre-training) as the pre-trained model
    - **rl**: Use SHAC reinforcement learning algorithm for fine-tuning

- Evaluation in the real-world
  - [External Hand-Eye Calibration](https://github.com/pal-robotics/aruco_ros)
  - [Controller Setup](https://moveit.picknik.ai/main/doc/examples/realtime_servo/realtime_servo_tutorial.html)
  - SAM2 Visual Perception
  - End-to-End Control Publishing

## Demonstration
### Simulaiton

<table>
<tr>
  <!-- Block 1 -->
  <td align="center" width="50%">
    <table>
      <tr>
        <td><img src="assets/GIF-simulation/pour_inviscid_PT.gif" width="100%"></td>
        <td><img src="assets/GIF-simulation/pour_viscous_PT.gif" width="100%"></td>
        <td><img src="assets/GIF-simulation/pour_elasto_PT.gif" width="100%"></td>
      </tr>
      <tr>
        <td align="center">↓<br><span style="font-size:12px;">Fine-tuned</span></td>
        <td align="center">↓<br><span style="font-size:12px;">Fine-tuned</span></td>
        <td align="center">↓<br><span style="font-size:12px;">Fine-tuned</span></td>
      </tr>
      <tr>
        <td><img src="assets/GIF-simulation/pour_inviscid_FT.gif" width="100%"></td>
        <td><img src="assets/GIF-simulation/pour_viscous_FT.gif" width="100%"></td>
        <td><img src="assets/GIF-simulation/pour_elasto_FT.gif" width="100%"></td>
      </tr>
    </table>
  </td>

  <!-- Block 2 -->
  <td align="center" width="50%">
    <table>
      <tr>
        <td><img src="assets/GIF-simulation/gather_inviscid_PT.gif" width="100%"></td>
        <td><img src="assets/GIF-simulation/gather_viscous_PT.gif" width="100%"></td>
        <td><img src="assets/GIF-simulation/gather_elasto_PT.gif" width="100%"></td>
      </tr>
      <tr>
        <td align="center">↓<br><span style="font-size:12px;">Fine-tuned</span></td>
        <td align="center">↓<br><span style="font-size:12px;">Fine-tuned</span></td>
        <td align="center">↓<br><span style="font-size:12px;">Fine-tuned</span></td>
      </tr>
      <tr>
        <td><img src="assets/GIF-simulation/gather_inviscid_FT.gif" width="100%"></td>
        <td><img src="assets/GIF-simulation/gather_viscous_FT.gif" width="100%"></td>
        <td><img src="assets/GIF-simulation/gather_elasto_FT.gif" width="100%"></td>
      </tr>
    </table>
  </td>
</tr>
</table>


### Real-World Deployment
<div align="center">

| | | | |
|---|---|---|---|
| <img src="assets/GIF-pour-granular/6159-1.gif" width="200"> | <img src="assets/GIF-pour-inviscid/6297-1.gif" width="200"> | <img src="assets/GIF-pour-viscous/6556-1.gif" width="200"> | <img src="assets/GIF-pour-elasto/6613-1.gif" width="200"> |
| <img src="assets/GIF-gather-granular/6720-1.gif" width="200"> | <img src="assets/GIF-gather-inviscid/7067-1.gif" width="200"> | <img src="assets/GIF-gather-viscous/7236-1.gif" width="200"> | <img src="assets/GIF-gather-elasto/6865-1.gif" width="200"> |

</div>



## Citation
If you find this work useful, please cite our paper:

```bibtex
@article{haixu2025nmi,
    author    = {Haixu Zhang, Bo Zhang, Danyang Zhang, Xi Chen, Wenqiang Lai, Hongxue Huang, Chi Zhang, Tin Lun Lam, Hu Huang, Yuan Gao},
    title     = {RheoAgent: A Cross-Rheological Material Handling Robotic Manipulation System via Hierarchical Decision-Making Framework},
    journal   = {xxx},
    year      = {2025},
}
```


## License


## Acknowledgement

