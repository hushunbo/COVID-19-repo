## Usage


### Description

This 3D V-Net is pretty similar to 3D U-Net. Therefore, the codes of 3D V-Net are revised from the 3D U-Net codes.
The main difference between these two ararchitectures contains: 1. Loss function (3D V-Net uses dice loss). 2. 3D V-Net uses the residual block.
These difference are shown in the configuration file (.yaml file).

### Install Dependencies

```
conda create --name 3d_vnet --file environment.txt
source activate 3d_vnet
pip install nibabel
pip install opencv-python
pip install pydicom
pip install SimpleITK
```

### Data and Checkpoint

Checkpoint on Google Drive: `03.baselines.demo/3D_Vnet/pytorch3dunet/3dunet/` please respect the folder structure.
Data on Google Drive: `CT_scan_spatial_signal_normalized/` paste these normalized `.npy` arrays into `03.baselines.demo/3D_Vnet/arrays_raw/`

### Transfer .npy to .h5 files
The normalized arrays are in `.npy` format, while 3D V-net takes `.h5` as input. 
see `./03.baselines.demo/3D V-net/pre_process.py` this file convert normalized arrays into `.h5` format
In `pre_process.py` you need to change the `"main_data_path"` to the directory of `.npy` and the `"destin_path"` to the directory of `.h5`.
```
cd ./03.baselines.demo/3D_Vnet
python pre_process.py
```
Now the testing `.npy` files have been changed into the file format for 3D V-Net, which is in `.h5`

### Test 3D V-Net

please open `./03.baselines.demo/3D_Vnet/resources/test_config_dice.yaml` 

line 2 is the model_path, please change it to your own directory, e.g.`/home/zhoul0a/Desktop/COVID-19/reports/COVID-19-repo-master/03.baselines.demo/3D V-net/pytorch3dunet/3dunet/best_checkpoint_vnet.pytorch`

line 40 is the directory saving proprocessed `.h5` files ready for prediction, please change it to your own directory, e.g. `/home/zhoul0a/Desktop/COVID-19/reports/COVID-19-repo-master/03.baselines.demo/3D V-net/h5files/`

```
python pytorch3dunet/predict.py --config resources/test_config_dice.yaml
```

The predictions will be saved to the folder containing the `.h5` test sets, which is the `"destin_path"` you have specified in `Transfer .npy to .h5 files`.


### Post-processing (transfer h5 to npz for further evaluation)

```
python post_process.py
```



The codes are modified based on [/wolny/pytorch-3dunet](https://github.com/wolny/pytorch-3dunet).
