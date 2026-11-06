# Pediatric Wrist Fracture Detection and Classification

## Project Overview

This project focuses on detecting and classifying wrist fractures in pediatric X-ray images using deep learning techniques.

The main objective was to develop a model capable of detecting visible fractures and identifying potential **hidden** fractures that are not directly visible in X-ray images.

The project explores different deep learning approaches, ultimately focusing on YOLOv8 Nano for fracture detection and image-level classification.

## Methodology

### 1. Initial Experiments

We initially experimented with several neural network architectures for image classification, including:
- Custom CNN architectures (trained from scratch).
- Pretrained models, particularly ResNet.

These experiments **are not** part of this repo. 

Despite experimenting with different approaches and training configurations, the classification models did not achieve satisfactory results. We therefore explored object detection as an alternative approach.

### 2. Dataset Preparation

Before training the YOLO model, we carefully prepared the dataset to distinguish between images with visible fractures and images without fractures.

An important challenge was the presence of **773 images containing hidden fractures**, which could not be reliably labeled as negative samples.

To address this issue, we excluded these images from the training and validation sets and assigned them to the test set. Furthermore, all images belonging to the same patients were kept in the same split to prevent data leakage.

The resulting dataset was divided as follows:

| Dataset Split | Number of Images | Percentage |
|---|---:|---:|
| Training | 14,182 | 69.8% |
| Validation | 2,735 | 13.5% |
| Test | 3,410 | 16.8% |
| **Total** | **20,327** | **100%** |

The test set contains all 773 images with hidden fractures, allowing us to evaluate the model's ability to identify fractures that were not represented in the training data.

#### Dataset Label Distribution

The YOLO dataset was prepared using a single detection class (class 0 – visible fracture).

| Split | Visible Fractures | No Visible Fracture | Total |
|---|---:|---:|---:|
| Training | 9,426 | 4,756 | 14,182 |
| Validation | 1,936 | 799 | 2,735 |
| Test | 2,188 | 1,222 | 3,410 |

Images without visible fractures were represented using empty YOLO annotation files.

### 3. Fracture Detection – YOLOv8 Nano

We used the YOLOv8 Nano architecture for fracture detection.

The model was trained for 30 epochs and achieved the following approximate performance:

- **mAP@50:** 95%
- **mAP@50-95:** 57%

These results indicated that the model could successfully detect visible fractures in X-ray images.

### 4. Image-Level Classification and Hidden Fracture Detection

The next step was to use the trained YOLO model for image-level classification, with particular emphasis on identifying hidden fractures.

We applied a simple confidence-based decision rule:

- If YOLO detects a bounding box with a confidence score of at least 5%, the image is classified as containing a fracture.
- Otherwise, the image is classified as having no fracture.

We experimented with different confidence thresholds and selected 0.05 as a value that provided a reasonable balance between the two classes.

Using this approach, we achieved approximately **84% classification accuracy on the test set**.

An important aspect of this experiment is that the test set includes images with hidden fractures that were excluded from training and validation.

## Project Structure

The repository contains three Jupyter notebooks covering the main stages of the project, along with a folder containing test images for inference.

```text
pediatric-wrist-fracture-classification/
├── 01-data-preparation-for-yolo.ipynb
├── 02-fracture-detection-and-classification.ipynb
├── 03_yolo_inference.ipynb
├── test_images/
├── README.md
└── .gitignore
```

01_data_preparation_for_yolo.ipynb: Dataset exploration, preprocessing and preparation of YOLO annotations.

02_fracture_detection_and_classification.ipynb: YOLO model training, evaluation and image-level classification experiments.

03_yolo_inference.ipynb: Running inference using the trained YOLO model and examining predictions on test images.

test_images/: A collection of test X-ray images used for inference and qualitative evaluation.

## Technologies Used

- Python
- PyTorch
- Ultralytics YOLOv8
- OpenCV
- NumPy
- Pandas
- Matplotlib
- Scikit-learn
- Jupyter Notebook

## Limitations

This project is an experimental deep learning approach to pediatric wrist fracture detection. The results should not be interpreted as clinical validation or used as a substitute for professional medical diagnosis.

Contributors

- Nikolina Ostojic – Master's student at the Faculty of Sciences, University of Novi Sad.
- Isidora Kisic – Master's student at the Faculty of Sciences, University of Novi Sad.