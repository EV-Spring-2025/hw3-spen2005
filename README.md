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

# How to run the code
0. Follow the instruction in PhysGaussian to set up the enviroment.
1. Download wolf_whitebg-trained.zip and unzip it.
2. run
   ```bash=
   python gs_simulation.py --model_path ./model/wolf_whitebg-trained/ --output_path output --config ./config/wolf_config.json --render_img --compile_video --white_bg
   ```
3. Manually adjust the parameter in ./config/wolf_config.json.

# Simulation Results
https://www.youtube.com/watch?v=PG57BQSjBsw

# Physical Parameters Adjustments and Ablation study

## n_grid
baseline: n_grid = 200

|       |   100   |   300   |
| ----- | ------- | ------- |
| snow  | 28.03dB | 31.61dB |
| jelly | 28.18dB | 25.85dB |

### Adjustment
We tested different values of n_grid. The default was 200, and we also tried 100 and 300. 

### Takeaways

We found that increasing n_grid makes the object look more detailed. For example, when the material is jelly, we observed bending or deformation effects at n_grid=300 that were not as visible with lower resolutions.

This is likely because a higher n_grid means the simulation grid is more finely divided. With more grid cells, the simulation can capture smaller, more complex motions and shapes. 

## substep_dt
baseline: substep_dt = 2e-5

|       |   6e-6  |   9e-6  |
| ----- | ------- | ------- |
| snow  | 31.96dB | 33.49dB |
| jelly | 33.43dB | 27.88dB |

### Adjustment
We tested different values of substep_dt. The default was 2e-5, and we also tried 9e-6 and 6e-6. 

### Takeaways
Visually, it was hard to notice any clear difference in the simulation results. However, we found that the smaller the substep_dt, the slower the simulation runs.

This is likely because substep_dt controls the time interval of each internal simulation step. A smaller substep_dt means the simulator takes more steps to cover the same amount of real time, which increases the total number of calculations. 

## grid_v_damping_scale
baseline: grid_v_damping_scale = 1.10

|       |   1.00  |   1.05  |
| ----- | ------- | ------- |
| snow  | 19.18dB | 28.02dB |
| jelly | 30.37dB | 28.18dB |

### Adjustment
We tested different values of grid_v_damping_scale. The default was 1.1, and we also tried 1.0 and 1.05. 

### Takeaways
We noticed that when grid_v_damping_scale is higher, the object tends to shake or bounce back and forth more.

This likely happens because a higher grid_v_damping_scale means there's less damping, or in other words, the simulation slows down the grid's velocity less. So, when the object moves, the leftover motion in the grid isn't fully removed, and that leftover "energy" causes the object to keep moving slightly back and forth 

## softening
baseline: softening = 0.10

|       |   0.15  |   0.20  |
| ----- | ------- | ------- |
| snow  | 28.03dB | 28.02dB |
| jelly | 43.41dB | 41.95dB |

### Adjustment
We tested different values of softening. The default was 0.10, and we also tried 0.15 and 0.20. 

### Takeaways
We observed that softening didn’t seem to affect the jelly material much, but for snow, increasing softening made the object fall faster.

