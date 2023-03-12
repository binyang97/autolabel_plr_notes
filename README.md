# Guide to setup conda environment for ```autolabel``` on euler cluster
Please create a conda environment with Python 3.8 or 3.9 before starting the installation

## 1. Basic environmental setups
Open the bashrc file (~/.bashrc) and type the following command
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

## 2. Install Pytorch and cudatoolkit
Run the following commands to install the packages in your environment
```
conda install pytorch==1.12.0 torchvision==0.13.0 torchaudio==0.12.0 cudatoolkit=11.3 -c pytorch
conda install ffmpeg
```
Run ```conda list ``` to check whether pytorch of cuda-version is correctly installed

## 3. Install tiny-cuda-nn
Run ```srun --gpus=1 --ntasks=2 --mem-per-cpu=4G -ply bash``` to start an interactive job for installation.
Then, run ```pip install git+https://github.com/NVlabs/tiny-cuda-nn/#subdirectory=bindings/torch``` to install the package
Note:
* If you get error like ```slurmstepd: error: poll(): Bad address ```, that means the installation got killed due to lack of memory. In this case, please increase the memory and cores in the command for starting an interactive job
* If you get some HTTP error, that is mostly because the euler is not connected to the Internet. Please check the eth_proxy in euler (or in .bashrc file) is correctly loaded
* If you get some GCC or G++ error, that is mostly due to the missmatched versions of gcc and cuda. Please check the all loaded modules. You can also use 
```module avail gcc/cuda``` to check the available versions in euler


## 4. Run the rest of commands in autolabel
```
git clone --recursive git@github.com:cvg/Hierarchical-Localization.git
pushd Hierarchical-Localization/
python -m pip install -e .
popd

git submodule update --init --recursive
pushd torch_ngp
git submodule update --init --recursive
pip install -e .

###This command again requires GPU access, please start an interactive job as installation for tiny-cuda-nn
bash scripts/install_ext.sh 

popd

pip install -e .
```

## Some other notes
You can also use ```sbatch``` to submit the jobs to remote cluster instead of starting a interactive job.
If you want to submit a job with bash script, you have to structure MyJob.sh as following.
```
#!/bin/sh
---- here is your work for submission----
```
and run ```dos2unix MyJob.sh``` in terminal to convert linebreaks in your bash file from dos format to unix format. (Otherwise ```sbatch``` will report an error)
