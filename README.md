<div align="center">

# <b>COMP 7160 Project</b>

[SHUWEN NIU](https://github.com/sw1014)
24484059

</div>

## Introduction
Recent advancements in 3D Gaussian Splatting (3DGS) have demonstrated superior performance, surpassing NeRF in both novel view synthesis quality and rendering speed. Building upon this progress, this project introduces 3D Gaussians to represent head avatars, facilitating efficient rendering through a standard differentiable rasterizer, in contrast to implicit representations. However, applying 3DGS for monocular head avatar reconstruction poses challenges due to its discretized and stochastic nature, which heavily relies on a plausible initialization for both geometry and the deformation field, complicating training convergence. To address this, this project explores an efficient initialization strategy for 3D Gaussian head avatars. Specifically, inspired by [MonoGaussianAvatar](https://github.com/yufan1012/MonoGaussianAvatar),
initializing the mean positions of Gaussian points using a limited number of points randomly sampled on a sphere, complemented by a carefully designed point insertion and deletion strategy. This approach ensures rapid convergence to coarse geometry during the initial stage while allowing for subsequent refinement of the geometry.

This project is a reproduction of [MonoGaussianAvatar](https://github.com/yufan1012/MonoGaussianAvatar), the main contributions of this project can be summarized as follows:
* This project employs dynamic 3D Gaussians as representation to reconstruct high-fidelity head avatars from monocular inputs, enabling the synthesis of novel poses and expressions.
* This project explores an initialization strategy for 3D Gaussian-based head avatars, leading to robust and efficient convergence during the training process. Moreover, experimental results indicate that initializing with a small number of points randomly sampled on a sphere enhances the capture of finer details compared to a large number of well-initialized points sampled on the FLAME template.


## Getting Started
* 环境配置过程中主要需要注意满足PyTorch3d, functorch, [gaussian-splatting](https://github.com/graphdeco-inria/gaussian-splatting),建议与本项目采用相同配置：python 3.9,pytorch 1.11.0, cuda 11.3

* Create a conda or python environment and activate: `conda create -n monogshead python=3.9; conda activate monogshead`.
* Install PyTorch 1.11.0:
 `conda install pytorch==1.11.0 torchvision==0.12.0 torchaudio==0.11.0 cudatoolkit=11.3 -c pytorch`.
* Install PyTorch3d:
 `conda install -c fvcore -c iopath -c conda-forge fvcore iopath; conda install pytorch3d -c pytorch3d`
* Install other requirements: `pip install -r requirement.txt; pip install -U face-detection-tflite`
* Install gaussian-splatting: 
`cd submodules/; git clone https://github.com/graphdeco-inria/gaussian-splatting --recursive; cd gaussian-splatting/; pip install -e submodules/diff-gaussian-rasterization`

##FLAME模型下载
* Download [FLAME model](https://flame.is.tue.mpg.de/download.php), choose **FLAME 2020** and unzip it, copy 'generic_model.pkl' into `./code/flame/FLAME2020`

## Preparing dataset
This project use a preprocessed dataset [subject1](https://dataset.ait.ethz.ch/downloads/IMavatar_data/data/subject1.zip) from [IMavatar](https://github.com/zhengyuf/IMavatar/).

If you'd like to generate your own dataset, please follow intructions in the [IMavatar repo](https://github.com/zhengyuf/IMavatar/tree/main/preprocess).

dataset的文件目录结构为：




## Training
```
python scripts/exp_runner.py ---conf ./confs/subject1.conf [--is_continue]
```
## Evaluation
Set the *is_eval* flag for evaluation, optionally set *checkpoint* (if not, the latest checkpoint will be used) and *load_path* 
```
python scripts/exp_runner.py --conf ./confs/subject1.conf --is_eval [--checkpoint 60] [--load_path ...]
```