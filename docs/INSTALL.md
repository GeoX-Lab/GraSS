# Installation & Data Preparetion

## Requirements

* Linux
* Python>=3.6.2 and < 3.9
* PyTorch>=1.4
* torchvision (matching PyTorch install)
* CUDA (must be a version supported by the pytorch version)
* OpenCV
* scikit-image

## Installing GraSS with VISSL

1. Create Enviroment
```
conda create -n grass_env python=3.8
conda activate grass_env
```
2. Install PyTorch
```
conda install pytorch==2.0.0 torchvision==0.15.0 torchaudio==2.0.0 pytorch-cuda=11.7 -c pytorch -c nvidia
```
Or Visit [Pytorch](https://pytorch.org/) to install

3. Install Apex(optional)
```
git clone --recursive https://www.github.com/NVIDIA/apex
cd apex
python3 setup.py install
```

4. Install opencv and scikit-image
```
pip install opencv-python
pip install scikit-image
```

4. Install GraSS

Download GraSS source code and switch to the source path for installation:

```
git clone --recursive https://github.com/GeoX-Lab/GraSS.git
cd GraSS
pip install --progress-bar off -r requirements.txt
pip install classy-vision@https://github.com/facebookresearch/ClassyVision/tarball/master
pip install -e .[dev]
```

## Data Preparetion and Training

1. Prepare Data, e.g.,

(The data is organized with ImageNet style)
```
SegmentationImage
|_ <potsdam>
|   _ <ssl_train>
|  |  |_ <train>
|  |  |  |_ <img-t1-name>.tif
|  |  |  |_ ...
|  |  |  |_ <img-tN-name>.tif
|  |  |  |_ ...
|   _ <ssl_val>
|  |  |_ <val>
|  |  |  |_ <img-v1-name>.tif
|  |  |  |_ ...
|  |  |  |_ <img-vN-name>.tif
|  |  |  |_ ...
|_ <loveda_urban>
|  |_ <ssl_train>
|  |  |_ <train>
|  |  |  |_ <img-t1-name>.tif
|  |  |  |_ ...
|  |  |  |_ <img-tN-name>.tif
|  |  |  |_ ...
|_ <loveda_rural>
|  |_ <ssl_train>
|  |  |_ <train>
|  |  |  |_ <img-t1-name>.tif
|  |  |  |_ ...
|  |  |  |_ <img-tN-name>.tif
|  |  |  |_ ...
```
2. Set Data Path

move to [dataset_catalog.json](../configs/config/dataset_catalog.json) and add (e.g. LoveDA Urban):
```
{
    "loveda_urban":{
        "train":["SegmentationImage/loveda_urban/ssl_train"," "],
        "val":["SegmentationImage/loveda_urban/ssl_val"," "]
    }
}
```
3. Data Set

There are two ways to set data config:

a) Move to [grass_1gpu_resnet_b256.yaml](../configs/config/pretrain/GraSS/grass_1gpu_resnet_b256.yaml) and change DATASET_NAMES to (e.g. Potsdam):
```
DATASET_NAMES: ["loveda_urban"] # change dataset name here
```
and training with this config ([grass_1gpu_resnet_b256.yaml](../configs/config/pretrain/GraSS/grass_1gpu_resnet_b256.yaml))
```
python tools/run_distributed_engines.py config=pretrain/GraSS/grass_1gpu_resnet_b256.yaml
```
b) Execute directly without modifying the configuration file:
```
python tools/run_distributed_engines.py config=pretrain/GraSS/grass_1gpu_resnet_b256.yaml \
config.DATA.TRAIN.DATASET_NAMES=["loveda_urban"]
```

4. Other Config

All settings of GraSS can be set in [grass_1gpu_resnet_b256.yaml](../configs/config/pretrain/GraSS/grass_1gpu_resnet_b256.yaml), including data augumentation, training epoch, optimizer, adn hyperparameters, etc.
