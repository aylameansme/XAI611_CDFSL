# XAI611_CDFSL
Reduced version of CDFSL for XAI611 project II ( most python files and description are originated from https://github.com/IBM/cdfsl-benchmark )

---

## Introduction 
The Cross-Domain Few-Shot Learning (CD-FSL) challenge benchmark includes data from the CropDiseases [1], EuroSAT [2], ISIC2018 [3-4], and ChestX [5] datasets, which covers plant disease images, satellite images, dermoscopic images of skin lesions, and X-ray images, respectively. The selected datasets reflect real-world use cases for few-shot learning since collecting enough examples from above domains is often difficult, expensive, or in some cases not possible. In addition, they demonstrate the following spectrum of readily quantifiable domain shifts from ImageNet: 1) CropDiseases images are most similar as they include perspective color images of natural elements, but are more specialized than anything available in ImageNet, 2) EuroSAT images are less similar as they have lost perspective distortion, but are still color images of natural scenes, 3) ISIC2018 images are even less similar as they have lost perspective distortion and no longer represent natural scenes, and 4) ChestX images are the most dissimilar as they have lost perspective distortion, all color, and do not represent natural scenes. *However, in this project, you can only evaluate on ChestX images. *

## Datasets 
The following datasets are used for evaluation in this challenge:

### Source domain:
#### - miniImageNet
```
gdown https://drive.google.com/uc?id=1uxpnJ3Pmmwl-6779qiVJ5JpWwOGl48xt
```

### Target domain:
#### - ChestX-Ray8
- Home: https://www.kaggle.com/nih-chest-xrays/data
- Direct: command line  ```kaggle datasets download -d nih-chest-xrays/data```

## General information
- No meta-learning in-domain

- Only ImageNet based models or meta-learning allowed.

- 5-way classification

- n-shot, for varying n per dataset

- 600 randomly selected few-shot 5-way trials up to 50-shot (scripts provided to generate the trials)

- Average accuracy across all trials reported for evaluation.

- For generating the trials for evaluation, please refer to finetune.py and the examples below

---
## Specific Task
- Shots: n = {5, 20, 50}
---
## Enviroment
- Python 3.5.5

- Pytorch 0.4.1

- h5py 2.9.0

---
## Step
1. Download the dataset for evaluation (ChestX-Ray8) using the above link.
2. Download miniImageNet using the above link.
3. Change configuration file ```./configs.py``` to reflect the correct paths to each dataset. Please see the existing example paths for information on which subfolders these paths should point to.
4. Train base models on miniImageNet
- Standard supervised learning on miniImageNet. 
  
```python ./train.py --dataset miniImageNet --model ResNet10  --method baseline --train_aug``` 
  
5. Test   
  
```python finetune.py --model ResNet10 --method baseline  --train_aug --n_shot 5 ```

6. For testing your own methods, simply replace the function finetune() in finetune.py with your own method. Your method should at least have the following arguments,

- novel_loader: data loader for the corresponding dataset (EuroSAT, ISIC2018, Plant Disease, ChestX-Ray8)

- n_query: number of query images per class

- n_way: number of shots

- n_support: number of support images per class

