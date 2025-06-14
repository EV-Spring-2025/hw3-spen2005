[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/SdXSjEmH)
# EV-HW3: PhysGaussian

This homework is based on the recent CVPR 2024 paper [PhysGaussian](https://github.com/XPandora/PhysGaussian/tree/main), which introduces a novel framework that integrates physical constraints into 3D Gaussian representations for modeling generative dynamics.

You are **not required** to implement training from scratch. Instead, your task is to set up the environment as specified in the official repository and run the simulation scripts to observe and analyze the results.


## Getting the Code from the Official PhysGaussian GitHub Repository
Download the official codebase using the following command:
```
git clone https://github.com/XPandora/PhysGaussian.git
```


## Environment Setup
Navigate to the "PhysGaussian" directory and follow the instructions under the "Python Environment" section in the official README to set up the environment.


## Running the Simulation
Follow the "Quick Start" section and execute the simulation scripts as instructed. Make sure to verify your outputs and understand the role of physics constraints in the generated dynamics.


## Homework Instructions
Please complete Part 1–2 as described in the [Google Slides](https://docs.google.com/presentation/d/13JcQC12pI8Wb9ZuaVV400HVZr9eUeZvf7gB7Le8FRV4/edit?usp=sharing).


# Reference
```bibtex
@inproceedings{xie2024physgaussian,
    title     = {Physgaussian: Physics-integrated 3d gaussians for generative dynamics},
    author    = {Xie, Tianyi and Zong, Zeshun and Qiu, Yuxing and Li, Xuan and Feng, Yutao and Yang, Yin and Jiang, Chenfanfu},
    booktitle = {Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition},
    year      = {2024}
}
```


## n_grid
baseline: n_grid = 200
### snow
100: 28.03dB
300: 31.61dB

### jelly
100: 28.18dB
300: 25.85dB

## substep_dt
baseline: substep_dt = 2e-5
### snow
6e-6: 31.96dB
9e-6: 33.49dB

### jelly
6e-6: 33.43dB
9e-6: 27.88dB

## grid_v_damping_scale
baseline: grid_v_damping_scale = 1.10
### snow
1.00: 19.18dB
1.05: 28.02dB

### jelly
1.00: 30.37dB
1.05: 28.18dB

### Insights
We tested different values of grid_v_damping_scale. The default was 1.1, and we also tried 1.0 and 1.05. We noticed that when grid_v_damping_scale is higher, the object tends to shake or bounce back and forth more.

This likely happens because a higher grid_v_damping_scale means there's less damping, or in other words, the simulation slows down the grid's velocity less. So, when the object moves, the leftover motion in the grid isn't fully removed, and that leftover "energy" causes the object to keep moving slightly back and forth 

## softening
baseline: softening = 0.10
### snow
0.15: 28.03dB
0.20: 28.02dB

### jelly
0.15: 43.41B
0.20: 28.02dB
