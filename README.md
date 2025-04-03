# Multi-Class Segmentation of Flood Imagery

## Project Summary

This project is the Segmentation Project of the OpenCV University course [_Deep Learning with TensorFlow and Keras_](https://opencv.org/university/deep-learning-with-tensorflow-keras/?srsltid=AfmBOor6TFbg3fgao3lfzY5EIBo3GTuRqpwvra6nrZBQpmDMZ8B7ZcGY).<br>
The objective is to develop a training pipeline and train a semantic segmentation model on the FloodNet dataset to classify 10 distinct classes. The dataset comprises drone-captured images depicting the aftermath of Hurricane Harvey, which led to severe flooding. 

The notebook `FloodNet_segmentation.ipynb` was used to process the images and implement the training pipeline for the semantic segmentation task.

## Dataset Description

The FloodNet dataset contains a total of __2343__ images and corresponding masks.__1843__ images are dedicated for training and __500__ images are left for the test set for evaluation.
The dataset consists of 10 classes : <br>
`Background, Building Flooded, Building Non-Flooded, Road flooded, Road Non-flooded, Water, Tree, Vehicle, Pool, Grass ` <br>

Examples:<br>

![image](https://github.com/user-attachments/assets/6461d545-a1a9-4191-ab2b-f2b9fd1c608f)

To train the model, the training dataset was split into a training and validation set with a __80-20__ ratio.

### Note :

- One can see that the last image sample shown above (bottom right) has its ground truth mask incorrectly labelled. This kind of mistake can be found several times in the training dataset.

## Model Architecture

The model architecture chosen to tacke this segmentation task is the __SegFormer__ model, which implementation can be found in the [keras-cv](https://github.com/keras-team/keras-cv) repository.
The backbone for this model is the __MiTBackbone__ with weights `mit_b0_imagenet` pretrained on imagenet. <br>

## Data Augmentation

The training data consists of just over 1800 images, which is not enough data to train a model on and it can also cause overfitting. Therefore, the data was pre-processed using data augmentation and resizing.

![image](https://github.com/user-attachments/assets/a24eae59-5fe0-429b-860c-20d12d99ac74)

## Training Hyperparameters

- Epochs: __50__<br>
- Optimizer: __Adam__<br>
- Initial Learning Rate: __0.0001__<br>
- LR scheduler: __ReduceLROnPlateau__<br>
- Batch size: __8__<br>

## Accuracy on the test set

The trained model based on the configuration mentioned above lead to a dice score of __83.1%__ on the test set on the Kaggle's Leaderboard
![image](https://github.com/user-attachments/assets/81cce902-d561-43ca-b517-d939cb291211)
