# 01 Tracking

## Preparation

Download the AlphaTracker repository. Once downloaded, change the name of the main folder from `AlphaTracker-master` to `AlphaTracker`. 

### Install conda

This project is tested in conda env and thus conda is recommended. To install conda, please follow the instructions from the [conda website](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html) With conda installed, please set up the environment with the following steps.

### Install conda environment

In your command window, locate the terminal prompt. Open this application. Then, find the folder that contains the `AlphaTracker` repository that you just downloaded. Then inside the terminal window, change the directory as follows: `cd /path/to/AlphaTracker`. 

1. Create conda environment with dependencies by typing in the following:
```bash
conda env create -f environment.yml
```

If the above command line failed, please manually install the packages in the environment.yml that failed to install and then run the following command line:
```bash
conda activate alphatracker
conda env update --file environment.yml
```


### Install YOLO

Install [YOLO](https://pjreddie.com/darknet/yolo/) for training by copy-pasting the following into the terminal window.
```bash
cd ./Tracking/AlphaTracker/train_yolo/darknet/
make
cd ../../../../
```
To test that your installation was successful:
```bash
cd ./Tracking/AlphaTracker/train_yolo/darknet/
./darknet
```
If successful, you should see the following:
```bash
usage: ./darknet <function>
```
If it did not install but instead creates an error about shared libraries make sure your CUDA libraries are in your library path:
```bash
export LD_LIBRARY_PATH=/usr/local/lib:/usr/local/cuda-10.0
./darknet
```
***Make sure the path in the above example points to the correct path on YOUR system.  ***

You may choose to add this line to your ~/.bashrc file.

### Install PyTorch
Install pytorch following the guide from [Pytorch Website](https://pytorch.org/get-started/previous-versions/).
(The code is tested with pytorch 0.4.0, pytorch 0.4.1)

For example, to install with CUDA-9.0, 
```bash
conda install pytorch=0.4.1 cuda90 -c pytorch
```


### Download weights 

Install google drive on your system, if not already installed.
```bash
pip install googledrivedownloader
```

Download files from google drive and place them in specific locations by copy-pasting the following into the terminal window:
```bash
conda activate alphatracker
cd ./Tracking/AlphaTracker/
python3 download.py
```

You can check that your sample data has downloaded correctly to the data directory
```bash
ls ./data/sample_annotated_data/demo
```

<br>

## Training

### Step 1. Data Preparation

Labeled data is required to train the model. The code would read RGB images and json files of
annotations to train the model. Our code is compatible with data annotated by the open source tool Sloth.
Figure 1 shows an example of annotation json file. In this example, there only two images. Each image
has two mice and each mouse has two keypoint annotated.
<div align="center">
    <img src="media/jsonFormatForTraining.png", width="500" alt><br>
    Figure 1. Example of Annotation Json File
</div>

**Note** that point order matters. You must annotate all body parts in the same order for all frames. For
example, all the first points represent the nose, all the second points represent the tail and etc.
If the keypoint is not visible in one frame, then make the x,y of the keypoint to be -1.

### Step 2. Configuration

Before training, you need to charge the parameters in ./setting.py (red block in Figure 2). The meaning of the parameters can be found in the ./setting.py.
<div align="center">
    <img src="media/parameterForTracking.png", width="500" alt><br>
    Figure 2. Parameters
</div>

### Step 3. Running the code

Change directory to the alphatracker folder (where this README is in) and use the following command line to train the model:
```bash
conda activate alphatracker
python train.py
```

<br>



## Tracking

### Step 1. Configuration

Before tracking, you need to change the parameters in ./Tracking/AlphaTracker/setting.py (blue block in Figure 2). The meaning of
the parameters can be found in the ./Tracking/AlphaTracker/setting.py.

The default ./Tracking/AlphaTracker/setting.py will use a trained weight to track a demo video

### Step 2. Running the code

Use the following command line to train the model...copy paste the following into the terminal window:
```bash
conda activate alphatracker
cd ./Tracking/AlphaTracker/
python track.py
```



<br>

### General Notes about the Parameters:
1. Remember not to include any spaces or parentheses in your file names. Also, file names are case-sensitive. 
2. For training the parameter num_mouse must include the same number of items as the number of json files
that have annotated data. For example if you have one json file with annotated data for 3 animals then
```num_mouse=[3]``` if you have two json files with annoted data for 3 animals then ```num_mouse=[3,3]```.
3. ```sppe_lr``` is the learning rate for the SAPE network. If your network is not performing well you can lower this
number and try retraining
4. ```sppe_epoch``` is the number of training epochs that the SAPE network does. More epochs will take longer but
can potentially lead to better performance.

<br>


