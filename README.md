# TAL in Long_jump with ActionFormer
## Introduction
This is a short introduction to this repository. We use the ActionFormer. It is a new way to localize moments of actions with Transformers. You can find some details in [project original link](https://github.com/happyharrycn/actionformer_release). In this project, I will use it to localize the long jump action. 

## ActionFormer Introduction
This code repo implements ActionFormer, one of the first Transformer-based model for temporal action localization --- detecting the onsets and offsets of action instances and recognizing their action categories. Without bells and whistles, ActionFormer achieves 71.0% mAP at tIoU=0.5 on THUMOS14, outperforming the best prior model by 14.1 absolute percentage points and crossing the 60% mAP for the first time. Further, ActionFormer demonstrates strong results on ActivityNet 1.3 (36.56% average mAP) and the more challenging EPIC-Kitchens 100 (+13.5% average mAP over prior works).

# Reproduce the results on Dataset

**Details**: The features are extracted from two-stream I3D models pretrained on Kinetics using clips of `16 frames` at the video frame rate (`~30 fps`) and a stride of `4 frames`. This gives one feature vector per `4/30 ~= 0.1333` seconds.

**Unpack Features and Annotations**
* Unpack the file under *./data* (or elsewhere and link to *./data*).
* The folder structure should look like
```
This folder
│   README.md
│   ...  
│
└───data/
│    └───jump/
│    │	 └───annotations
│    │	 └───i3d_features   
│    └───...
|
└───librs
│
│   ...
```

**Training and Evaluation**
* Train our ActionFormer with I3D features. This will create an experiment folder under *./ckpt* that stores training config, logs, and checkpoints.
```shell
python ./train.py ./configs/jump_i3d.yaml --output reproduce
```
* [Optional] Monitor the training using TensorBoard
```shell
tensorboard --logdir=./ckpt/jump_i3d_reproduce/logs
```
* Evaluate the trained model. The expected average mAP should be around 62.6(%) as in Table 1 of our main paper. **With recent commits, the expected average mAP should be higher than 66.0(%)**.

仅查看结果指标运行：
```shell
python ./eval.py ./configs/jump_i3d.yaml ./ckpt/jump_i3d_reproduce 
```
查看输出的结果（开始结束时间与标签和置信度）：
```shell
python ./eval.py ./configs/jump_i3d.yaml ./ckpt/jump_i3d_reproduce --saveonly True
```
