# Multi-Class Segmentation of Flood Imagery

## 🎯 Project Objective
The goal of this project is to build a semantic segmentation pipeline trained on the FloodNet dataset. The task involves classifying each pixel into one of 10 distinct classes, using aerial drone imagery captured in the aftermath of Hurricane Harvey, which caused extensive flooding.

### 📘 Workflow
The entire process, from data preprocessing to model training, is implemented in the notebook:
`FloodNet_segmentation_v2.ipynb`.

This notebook includes:

- Data loading and augmentation<br>
- Model architecture and training setup<br>
- Evaluation on the test set.
  
## Dataset Description

The FloodNet dataset consists of a total of __2,343__ images with corresponding segmentation masks.

- __1,843__ images are used for training. However, to train the model, this training set was split into a training and validation set with a 80-20 ratio<br>
- __500__ images are reserved for testing and evaluation.

🏷️ Classes
FloodNet features __10__ semantic classes:
```
1. Background  
2. Building Flooded  
3. Building Non-Flooded  
4. Road Flooded  
5. Road Non-Flooded  
6. Water  
7. Tree  
8. Vehicle  
9. Pool  
10. Grass
```
Examples:<br>

![image](https://github.com/user-attachments/assets/6461d545-a1a9-4191-ab2b-f2b9fd1c608f)



### Note :

- One can see that the last image sample shown above (bottom right) has its ground truth mask incorrectly labelled. This kind of mistake can be found several times in the training dataset.
The impact of such wrong data on the quality of the model will be studied in the future by training the model with cleansed data.  

## 🧠 Model Architecture

To tackle the semantic segmentation task, the SegFormer model was used, which has a transformer-based architecture for efficient and scalable segmentation.
The implementation is sourced from the official [keras-cv repository](https://github.com/keras-team/keras-cv).

The model is built using the MiT (Mix Transformer) backbone, specifically:

- Backbone: `MiT-B0`<br>
- Pretrained Weights: `mit_b0_imagenet` (pretrained on ImageNet).

## 📈 Data Augmentation

With only 1,800+ images available in the training set, the dataset size is insufficient for robust model training and can lead to overfitting. To address this, data augmentation techniques and resizing were applied during the preprocessing stage. These methods help to artificially expand the dataset, improving the model's generalization ability and reducing the risk of overfitting.

```
train_transforms = A.Compose ([
            # Horizontal/vertical flipping
            A.HorizontalFlip(p=0.5),
            A.VerticalFlip(p=0.5),  
            A.Rotate(limit=90, p=0.5),

            # Random scaling (zooming in/out)
            A.ShiftScaleRotate(
                shift_limit=0.1,   
                scale_limit=0.2,   
                rotate_limit=0,    
                p=0.5              
            ),

            # Random brightness and contrast adjustment
            A.RandomBrightnessContrast(
                brightness_limit=0.2,  
                contrast_limit=0.2,    
                p=0.5
            ),
        ])
 ```

## Training Hyperparameters

- Epochs: __50__<br>
- Optimizer: __Adam__<br>
- Initial Learning Rate: __0.0001__<br>
- LR scheduler: __ReduceLROnPlateau__<br>
- Batch size: __8__<br>

## 📊 Training Metrics

The following graphs show the evolution of accuracy, Dice coefficient, and Dice loss throughout the training process. As observed, all metrics and the loss function reach convergence after approximately __35 epochs__.

![image](https://github.com/user-attachments/assets/94cb5a09-64c0-4cc2-988b-6c259ec66823)
    
## ✅ Accuracy on the test set

The trained model, based on the configuration outlined above, achieved a Dice coefficient of 83.6% on the test set, as evaluated on Kaggle's leaderboard.

![Floodnet_segmentation_leaderbaord](https://github.com/user-attachments/assets/53f18eab-bf4e-4b94-9a1f-42afdf393374)

