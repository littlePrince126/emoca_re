
<!-- # EMOCA -->
<h1 align="center">EMOCA: Emotion Driven Monocular Face Capture and Animation</h1>
<p align="center">

  <p align="center">
    <a href="https://ps.is.tuebingen.mpg.de/person/rdanecek"><strong>Radek Daněček</strong></a>    
    ·
    <a href="https://ps.is.tuebingen.mpg.de/person/black"><strong>Michael J. Black</strong></a>
    ·
    <a href="https://sites.google.com/site/bolkartt"><strong>Timo Bolkart</strong></a>

  </p>
  <h2 align="center">CVPR 2022</h2>
  <div align="center">
  </div>

  <!-- <a href="">
    <img src="./assets/teaser.jpeg" alt="Logo" width="100%">
  </a> -->

This repository is the official implementation of the [CVPR 2022](https://cvpr2022.thecvf.com/) paper [EMOCA: Emotion-Driven Monocular Face Capture and Animation](https://ps.is.mpg.de/uploads_file/attachment/attachment/686/EMOCA__CVPR22.pdf). 



  <!-- 
<p align="center"> 
<img src="gdl_apps/EMOCA/EMOCA_gif_sparse_det.gif">
<img src="gdl_apps/EMOCA/EMOCA_gif_sparse_rec.gif">
</p>
-->
<p align="center"> 
<img src="gdl_apps/EMOCA/EMOCA_gif_sparse_det_rec.gif">
</p>

<p align="center"> 
<img src="gdl_apps/EMOCA/emoca.png">
</p>
<p align="center">Top row: input images. Middle row: coarse shape reconstruction. Bottom row: reconstruction with detailed displacements.<p align="center">


<p align="center">
  <br>
    <a href="https://pytorch.org/get-started/locally/"><img alt="PyTorch" src="https://img.shields.io/badge/PyTorch-ee4c2c?logo=pytorch&logoColor=white"></a>
    <a href="https://pytorchlightning.ai/"><img alt="Lightning" src="https://img.shields.io/badge/-Lightning-792ee5?logo=pytorchlightning&logoColor=white"></a>
    <a href='https://emoca.is.tue.mpg.de/' style='padding-left: 0.5rem;'>
      <img src='https://img.shields.io/badge/Project-Page-blue?style=flat&logo=Google%20chrome&logoColor=blue' alt='Project Page'></a>
    <a href='https://youtu.be/zjMLB2-dVGw' style='padding-left: 0.5rem;'>
      <img src='https://img.shields.io/badge/Youtube-Video-red?style=flat&logo=youtube&logoColor=red' alt='Youtube Video'>
    </a>
    <a href='https://ps.is.mpg.de/uploads_file/attachment/attachment/686/EMOCA__CVPR22.pdf'>
      <img src='https://img.shields.io/badge/Paper-PDF-green?style=flat&logo=arXiv&logoColor=green' alt='Paper PDF'>
    </a>
</p>

EMOCA takes a single in-the-wild image as input and reconstructs a 3D face with sufficient facial expression detail to convey the emotional state of the input image. EMOCA advances the state-of-the-art monocular face reconstruction in-the-wild, putting emphasis on accurate capture of emotional content. The official project page is [here](https://emoca.is.tue.mpg.de/index.html).
 

## EMOCA project 
The training and testing script for EMOCA can be found in this subfolder: 

[EMOCA](gdl_apps/EMOCA) 

## Installation 

### Dependencies

1) Install [conda](https://docs.conda.io/en/latest/miniconda.html)

2) Install [mamba](https://github.com/mamba-org/mamba)

<!-- 0) Clone the repo with submodules:  -->
<!-- ``` -->
<!-- git clone --recurse-submodules ... -->
<!-- ``` -->
3) Clone this repo

### Short version 

1) Run the installation script: 

```bash
bash install.sh
```
If this ran without any errors, you now have a functioning conda environment with all the necessary packages to [run the demos](#usage). If you had issues with the installation script, go through the [long version](#long-version) of the installation and see what went wrong. Certain packages (especially for CUDA, PyTorch and PyTorch3D) may cause issues for some users.

### Long version

1) Pull the relevant submodules using: 
```bash
bash pull_submodules.sh
```


2) Set up a conda environment with one of the provided conda files. I recommend using `conda-environment_py36_cu11_ubuntu.yml`.  
<!-- This is the one I use for the cluster `conda-environment_py36_cu11_cluster.yml`. The differences between tehse two are probably not important but I include both for completeness.  -->

You can use [mamba](https://github.com/mamba-org/mamba) to create a conda environment (strongly recommended):

```bash
mamba env create python=3.6 --file conda-environment_py36_cu11_ubuntu.yml
```

but you can also use plain conda if you want (but it will be slower): 
```bash
conda env create python=3.6 --file conda-environment_py36_cu11_ubuntu.yml
```


Note: the environment might contain some packages. If you find an environment is missing then just `conda/mamba`- or  `pip`- install it and please notify me.

2) Activate the environment: 
```bash 
conda activate work36_cu11
```

3) For some reason cython is glitching in the requirements file so install it separately: 
```bash 
pip install Cython==0.29.14
```

4) Install `gdl` using pip install. I recommend using the `-e` option and I have not tested otherwise. 

```bash
pip install -e .
```

5) Verify that previous step correctly installed Pytorch3D

For some people the compilation fails during requirements install and works after. Try running the following separately: 

```bash
pip install git+https://github.com/facebookresearch/pytorch3d.git@v0.6.0
```

Pytorch3D installation (which is part of the requirements file) can unfortunately be tricky and machine specific. EMOCA was developed with is Pytorch3D 0.6.0 and the previous command includes its installation from source (to ensure its compatibility with pytorch and CUDA). If it fails to compile, you can try to find another way to install Pytorch3D.

Note: EMOCA was developed with Pytorch 1.9.1 and Pytorch3d 0.6.0 running on CUDA toolkit 11.1.1 with cuDNN 8.0.5. If for some reason installation of these failed on your machine (which can happen), feel free to install these dependencies another way. The most important thing is that version of Pytorch and Pytorch3D match. The version of CUDA is probably less important.

## Usage 

0) Activate the environment: 
```bash
conda activate work36_cu11
```

1) For running EMOCA examples, go to [EMOCA](gdl_apps/EMOCA) 

2) For running examples of Emotion Recognition, go to [EmotionRecognition](gdl_apps/EmotionRecognition)


## Structure 
This repo has two subpackages. `gdl` and `gdl_apps` 

### GDL
`gdl` is a library full of research code. Some things are OK organized, some things are badly organized. It includes but is not limited to the following: 

- `models` is a module with (larger) deep learning modules (pytorch based) 
- `layers` contains individual deep learning layers 
- `datasets` contains base classes and their implementations for various datasets I had to use at some points. It's mostly image-based datasets with various forms of GT if any
- `utils` - various tools

The repo is heavily based on PyTorch and Pytorch Lightning. 

### GDL_APPS 
`gdl_apps` contains prototypes that use the GDL library. These can include scripts on how to train, evaluate, test and analyze models from `gdl` and/or data for various tasks. 

Look for individual READMEs in each sub-projects. 

Current projects: 
- [EMOCA](gdl_apps/EMOCA) 
- [EmotionRecognition](gdl_apps/EmotionRecognition)



## Citation 

If you use this work in your publication, please cite the following publications:
```
@inproceedings{EMOCA:CVPR:2022,
  title = {{EMOCA}: {E}motion Driven Monocular Face Capture and Animation},
  author = {Danecek, Radek and Black, Michael J. and Bolkart, Timo},
  booktitle = {Conference on Computer Vision and Pattern Recognition (CVPR)},
  pages = {},
  year = {2022}
}
```
As EMOCA builds on top of [DECA](https://github.com/YadiraF/DECA) and uses parts of DECA as fixed part of the model, please further cite:
```
@article{DECA:Siggraph2021,
  title={Learning an Animatable Detailed {3D} Face Model from In-The-Wild Images},
  author={Feng, Yao and Feng, Haiwen and Black, Michael J. and Bolkart, Timo},
  journal = {ACM Transactions on Graphics (ToG), Proc. SIGGRAPH},
  volume = {40}, 
  number = {8}, 
  year = {2021}, 
  url = {https://doi.org/10.1145/3450626.3459936} 
}
```

## License
This code and model are **available for non-commercial scientific research purposes** as defined in the [LICENSE](https://emoca.is.tue.mpg.de/license.html) file. By downloading and using the code and model you agree to the terms of this license. 

## Acknowledgements 
There are many people who deserve to get credited. These include but are not limited to: 
Yao Feng and Haiwen Feng and their original implementation of [DECA](https://github.com/YadiraF/DECA).
Antoine Toisoul and colleagues for [EmoNet](https://github.com/face-analysis/emonet).
