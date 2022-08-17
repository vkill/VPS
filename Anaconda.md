## Anaconda

### Install

Ref https://docs.anaconda.com/anaconda/install/linux/

```
cd ~
wget https://repo.anaconda.com/archive/Anaconda3-2022.05-Linux-x86_64.sh
bash Anaconda3-2022.05-Linux-x86_64.sh

# accept the license terms
# accept the default install location
# Don't run conda init

source ~/anaconda3/bin/activate
conda init zsh

rm -rf ~/Anaconda3-2022.05-Linux-x86_64.sh
```

```
conda info -e

conda create -n xx python
conda activate xx

conda create --name yy --copy --clone xx
conda activate yy

conda activate base
conda remove -n yy --all
conda remove -n xx --all

conda deactivate
```

```
sudo apt -y install nvidia-cuda-toolkit

conda install pytorch torchvision torchaudio cudatoolkit -c pytorch
```
