# DEHRL

This repository provides implementation of [**domains**](#domains), [**DEHRL**](#setup-an-environment-to-run-our-code) and [**results visualization**](#results-visualization) for reproducing results in the paper:

* [**Diversity−Driven Extensible Hierarchical Reinforcement Learning.**](xxx)
[*Yuhang Song* &#8224;](http://www.cs.ox.ac.uk/people/yuhang.song/),
[*Jianyi Wang* &#8224;](http://45.77.201.133/html/Members/jianyiwang.html),
[*Thomas Lukasiewicz* &#8727;](http://www.cs.ox.ac.uk/people/thomas.lukasiewicz/),
[*Zhenghua Xu* &#8727;](https://www.cs.ox.ac.uk/people/zhenghua.xu/),
[*Mai Xu*](http://45.77.201.133/html/Members/maixu.html).
Equal contribution&#8224;.
Corresponding author&#8727;.

Published on [**Proceedings of the 33rd National Conference on Artificial Intelligence (AAAI 2019)**](https://www.computer.org/web/tpami).
By [Intelligent Systems Lab](http://www.cs.ox.ac.uk/people/thomas.lukasiewicz/isg-index.html) @ [Computer Science](http://www.cs.ox.ac.uk/) in [University of Oxford](http://www.ox.ac.uk/).

<p align="center"><img src="https://github.com/YuhangSong/DEHRL/blob/code_release/imgs/overcooked.png"/></p>

Specifically, this repository includes simple guidelines to:
* [Use the domains](#domains).
* [Setup a environment and run DEHRL.](#setup-an-environment-and-run-dehrl)
* [Reproduce visualization from the paper.](#results-visualization)

If you find our code or paper useful for your research, please cite:
```
@inproceedings{SWLXX-AAAI-2019,
  title = "Diversity-Driven Extensible Hierarchical Reinforcement Learning",
  author = "Yuhang Song and Jianyi Wang and Thomas Lukasiewicz and Zhenghua Xu and Mai Xu",
  year = "2019",
  booktitle = "Proceedings of the 33rd National Conference on Artificial Intelligence‚ AAAI 2019‚ Honolulu, Hawaii, USA‚ January 27 - February 1‚ 2019",
  month = "January",
  publisher = "AAAI Press",
}
```

Note that the project is based on an [RL implemention by Ilya Kostrikov](https://github.com/ikostrikov/pytorch-a2c-ppo-acktr), we thank a lot for his contribution to the community.

## Domains

All domains share the same interface as [OpenAI Gym](https://github.com/openai/gym).
See ```overcooked.py```, ```minecraft.py```, ```gridworld.py``` for more details.
Note that MineCraft is based on [the implemention by Michael Fogleman](https://github.com/fogleman/Minecraft), we thank a lot for his contribution to the community.
Besides, be aware that MineCraft only supports single thread training and it requires the xserver.

## Setup an environment and run DEHRL

In order to install requirements, follow:

```bash
# create env
conda create -n dehrl

# source in env
source ~/.bashrc
source activate dehrl

conda install pytorch torchvision -c soumith
pip install opencv-contrib-python
conda install scikit-image
pip install --upgrade imutils

mkdir dehrl
cd dehrl
git clone https://github.com/YuhangSong/DEHRL.git
mkdir results
```

Run commands here to enter the virtual environment before proceeding to following commands:
```bash
source ~/.bashrc
source activate dehrl
```

### Run OverCooked

Level 1; Any | Level 1; Fix | Level 1; Random
:-------------------------:|:-------------------------:|:-------------------------:
<img src="imgs/1-any.png">  |  <img src="imgs/1-fix.png">  |  <img src="imgs/1-random.png">
Level 2; Any | Level 2; Fix | Level 2; Random
<img src="imgs/2-any.png">  |  <img src="imgs/2-fix.png">  |  <img src="imgs/2-random.png">

#### Level: 1

Goal-type: any.
```bash
CUDA_VISIBLE_DEVICES=0 python main.py --algo ppo --use-gae --lr 2.5e-4 --clip-param 0.1 --value-loss-coef 1 --num-processes 8 --actor-critic-mini-batch-size 256 --actor-critic-epoch 4 --exp code_release --obs-type 'image' --env-name "OverCooked" --reward-level 1 --setup-goal any --new-overcooked --num-hierarchy 2 --num-subpolicy 5 --hierarchy-interval 4 --num-steps 128 128 --reward-bounty 0.1875 --distance mass_center --transition-model-mini-batch-size 64 --train-mode together --clip-reward-bounty --clip-reward-bounty-active-function linear --log-behavior-interval 5 --aux r_0
```

Goal-type: fix
```bash
CUDA_VISIBLE_DEVICES=0 python main.py --algo ppo --use-gae --lr 2.5e-4 --clip-param 0.1 --value-loss-coef 1 --num-processes 8 --actor-critic-mini-batch-size 256 --actor-critic-epoch 4 --exp code_release --obs-type 'image' --env-name "OverCooked" --reward-level 1 --setup-goal fix --new-overcooked --num-hierarchy 2 --num-subpolicy 5 --hierarchy-interval 4 --num-steps 128 128 --reward-bounty 0.1875 --distance mass_center --transition-model-mini-batch-size 64 --train-mode together --clip-reward-bounty --clip-reward-bounty-active-function linear --log-behavior-interval 5 --aux r_0
```

Goal-type: random
```bash
CUDA_VISIBLE_DEVICES=0 python main.py --algo ppo --use-gae --lr 2.5e-4 --clip-param 0.1 --value-loss-coef 1 --num-processes 8 --actor-critic-mini-batch-size 256 --actor-critic-epoch 4 --exp code_release --obs-type 'image' --env-name "OverCooked" --reward-level 1 --setup-goal random --new-overcooked --num-hierarchy 2 --num-subpolicy 5 --hierarchy-interval 4 --num-steps 128 128 --reward-bounty 0.1875 --distance mass_center --transition-model-mini-batch-size 64 --train-mode together --clip-reward-bounty --clip-reward-bounty-active-function linear --log-behavior-interval 5 --aux r_0
```

#### Level 2

Goal-type: any
```bash
CUDA_VISIBLE_DEVICES=0 python main.py --algo ppo --use-gae --lr 2.5e-4 --clip-param 0.1 --value-loss-coef 1 --num-processes 8 --actor-critic-mini-batch-size 256 --actor-critic-epoch 4 --exp code_release --obs-type 'image' --env-name "OverCooked" --reward-level 2 --setup-goal any --new-overcooked --num-hierarchy 3 --num-subpolicy 5 5 --hierarchy-interval 4 12 --num-steps 128 128 128 --reward-bounty 1 --distance mass_center --transition-model-mini-batch-size 64 64 --train-mode together --clip-reward-bounty --clip-reward-bounty-active-function linear --log-behavior-interval 5 --aux r_0
```

Goal-type: fix
```bash
CUDA_VISIBLE_DEVICES=0 python main.py --algo ppo --use-gae --lr 2.5e-4 --clip-param 0.1 --value-loss-coef 1 --num-processes 8 --actor-critic-mini-batch-size 256 --actor-critic-epoch 4 --exp code_release --obs-type 'image' --env-name "OverCooked" --reward-level 2 --setup-goal fix --new-overcooked --num-hierarchy 3 --num-subpolicy 5 5 --hierarchy-interval 4 12 --num-steps 128 128 128 --reward-bounty 1 --distance mass_center --transition-model-mini-batch-size 64 64 --train-mode together --clip-reward-bounty --clip-reward-bounty-active-function linear --log-behavior-interval 5 --aux r_0
```

Goal-type: random
```bash
CUDA_VISIBLE_DEVICES=0 python main.py --algo ppo --use-gae --lr 2.5e-4 --clip-param 0.1 --value-loss-coef 1 --num-processes 8 --actor-critic-mini-batch-size 256 --actor-critic-epoch 4 --exp code_release --obs-type 'image' --env-name "OverCooked" --reward-level 2 --setup-goal random --new-overcooked --num-hierarchy 3 --num-subpolicy 5 5 --hierarchy-interval 4 12 --num-steps 128 128 128 --reward-bounty 1 --distance mass_center --transition-model-mini-batch-size 64 64 --train-mode together --clip-reward-bounty --clip-reward-bounty-active-function linear --log-behavior-interval 5 --aux r_0
```

#### Load pre-trained model and data

The code antomatically check and reload everything from the log dir you set, and everything (model/curves/videos, etc.) is saved every 10 minutes.
Thus, please feel free to continue your experiment from where you stopped by simply typing in the same command.

If you want to load our pre-trained model, download the zipped log dir from [here]() and unzip it to the ```../results/``` dir.
Then run the same commands to load our model as well as other training data.

### Run MineCraft

```bash
CUDA_VISIBLE_DEVICES=0 python main.py --algo ppo --use-gae --lr 2.5e-4 --clip-param 0.1 --value-loss-coef 1 --num-processes 1 --actor-critic-mini-batch-size 256 --actor-critic-epoch 4 --exp code_release --obs-type 'image' --env-name "MineCraft" --num-hierarchy 4 --num-subpolicy 8 8 8 --hierarchy-interval 4 4 4 --num-steps 128 128 128 128 --reward-bounty 1 --distance l1 --transition-model-mini-batch-size 64 64 64 --train-mode together --clip-reward-bounty --clip-reward-bounty-active-function linear --log-behavior-interval 10 --aux r_0
```

<p align="center"><img src="https://github.com/YuhangSong/DEHRL/blob/code_release/imgs/minecraft_0.png" width="600"/></p>

### Run Atari

#### Game: MontezumaRevengeNoFrameskip-v4

The agent can reach the key when the skull is not presented, video is [here](https://www.dropbox.com/s/abcjmzxu9eubcbd/H-0_F-67778560_observation.avi?dl=0).
```bash
CUDA_VISIBLE_DEVICES=0 python main.py --algo ppo --use-gae --lr 2.5e-4 --clip-param 0.1 --value-loss-coef 1 --num-processes 8 --actor-critic-mini-batch-size 256 --actor-critic-epoch 4 --exp code_release --obs-type 'image' --env-name "MontezumaRevengeNoFrameskip-v4" --num-hierarchy 3 --num-subpolicy 5 5 --hierarchy-interval 8 4 --num-steps 128 128 128 --reward-bounty 1 --distance mass_center --transition-model-mini-batch-size 64 64 --train-mode together --clip-reward-bounty --clip-reward-bounty-active-function linear --log-behavior-interval 5 --aux r_0
```
But due to the skull is affecting the mass centre of the state significantly, our method fails when the skull is moving in the state.
Necessary solution is to generate a mask that can locate the part of the state that is controlled by the agent.
Luckily, [a recent paper under review by ICLR 2019](https://www.dropbox.com/s/mftt1ll0q39dalz/File%202018-10-7%2C%209%2036%2022%20AM.pdf?dl=0) solves this problem.
But their code is not released yet, so we implement their idea to generate the mask.

However, their idea works ideally on simple domain like GridWorld.
Run following command to see the learnt masks on GridWorld,
```bash
CUDA_VISIBLE_DEVICES=0 python main.py --algo ppo --use-gae --lr 2.5e-4 --clip-param 0.1 --value-loss-coef 1 --num-processes 8 --actor-critic-mini-batch-size 256 --actor-critic-epoch 4 --exp code_release --obs-type 'image' --env-name "GridWorld" --num-hierarchy 2 --num-subpolicy 5 --hierarchy-interval 4 --num-steps 128 128 --reward-bounty 1 --distance mass_center --transition-model-mini-batch-size 64 --inverse-mask --num-grid 7 --train-mode together --clip-reward-bounty --clip-reward-bounty-active-function linear --log-behavior-interval 5 --aux r_0
```
<p align="center"><img src="https://github.com/YuhangSong/DEHRL/blob/code_release/imgs/adm_on_grid.gif" width="900"/></p>
But it performs poorly on MontezumaRevenge.
We are waiting for their official release to further advance our DEHRL on domains with uncontrollable objects.

## Run MuJoCo

```bash
CUDA_VISIBLE_DEVICES=0 python main.py --algo ppo --use-gae --lr 2.5e-4 --clip-param 0.1 --value-loss-coef 1 --num-processes 1 --actor-critic-mini-batch-size 256 --actor-critic-epoch 4 --exp code_release --obs-type 'image' --env-name "Reacher-v2" --num-hierarchy 3 --num-subpolicy 5 5 --hierarchy-interval 8 4 --num-steps 128 128 128 --reward-bounty 1 --distance mass_center --transition-model-mini-batch-size 64 64 --train-mode together --clip-reward-bounty --clip-reward-bounty-active-function linear --log-behavior-interval 5 --aux r_0
```

## Run Explore2D

Random policy baseline
```bash
CUDA_VISIBLE_DEVICES=0 python main.py --algo ppo --use-gae --lr 2.5e-4 --clip-param 0.1 --value-loss-coef 1 --num-processes 8 --actor-critic-mini-batch-size 256 --actor-critic-epoch 4 --exp 2d_explore --obs-type 'image' --env-name "Explore2D" --episode-length-limit 128 --num-hierarchy 2 --num-subpolicy 5 --hierarchy-interval 4 --num-steps 128 128 --reward-bounty 0 --distance l2 --transition-model-mini-batch-size 64 --train-mode together --clip-reward-bounty --clip-reward-bounty-active-function linear --log-behavior-interval 5 --aux r_0
```

Test basic, works perfectly [here](https://www.dropbox.com/s/qqsw2a28n1e5fvi/H-1_F-2843648_state_prediction.avi?dl=0)
```bash
CUDA_VISIBLE_DEVICES=0 python main.py --algo ppo --use-gae --lr 2.5e-4 --clip-param 0.1 --value-loss-coef 1 --num-processes 8 --actor-critic-mini-batch-size 256 --actor-critic-epoch 4 --exp 2d_explore --obs-type 'image' --env-name "Explore2D" --episode-length-limit 12 --num-hierarchy 2 --num-subpolicy 5 --hierarchy-interval 4 --num-steps 128 128 --reward-bounty 1 --distance l2 --transition-model-mini-batch-size 64 --train-mode together --clip-reward-bounty --clip-reward-bounty-active-function linear --log-behavior-interval 5 --aux r_0
```

Go for longer episode-length-limit,
```bash
CUDA_VISIBLE_DEVICES=0 python main.py --algo ppo --use-gae --lr 2.5e-4 --clip-param 0.1 --value-loss-coef 1 --num-processes 8 --actor-critic-mini-batch-size 256 --actor-critic-epoch 4 --exp 2d_explore --obs-type 'image' --env-name "Explore2D" --episode-length-limit 128 --num-hierarchy 2 --num-subpolicy 5 --hierarchy-interval 4 --num-steps 128 128 --reward-bounty 1 --distance l2 --transition-model-mini-batch-size 64 --train-mode together --clip-reward-bounty --clip-reward-bounty-active-function linear --log-behavior-interval 5 --aux r_0
```

Visualize Explore2D: replace ```main.py``` to ```vis_explore2d.py``` in corresponding command.

## Results Visualization

The code log multiple curves to help analysis the training process, run:
```
tensorboard --logdir ../results/code_release/
```
for visualization with tensorboard.
