# Guide to install tiny-cuda-nn on euler cluster

1. Open the bashrc file (~/.bashrc) and type the following command:
```
module load eth_proxy cuda/11.3.1 gcc/8.2.0 ninja
export CUDA_HOME='/cluster/apps/gcc-8.2.0/cuda-11.3.1-o54iuxgz6jm4csvkstuj5hjg4tvd44h3' 
```
Note: 
* The path to cuda can be different or changed due to the system update, in this case please check it in the euler
* Remember detach and restart the euler every time after adding some change in .bashrc file
* You can check the version of those modules by typing
```
module list  # all loaded modules
nvcc --version # whether cuda is nvcc-compiled
```
#!/bin/sh

# Before running this script to install tiny-cudann
# 1. Open ~/.bashrc and add the following command

# 
