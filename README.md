# Marvellous CNN - Surface Crack Detection

A Convolutional Neural Network (CNN) based deep learning project for detecting cracks on industrial surfaces from images.

The system performs binary image classification:

- **Crack** - Image contains a surface crack
- **No Crack** - Image does not contain a surface crack

---

## Project Overview

Surface cracks in industrial structures can affect the safety and durability of materials. Manual inspection of large numbers of surface images can be time-consuming.

This project uses a **Convolutional Neural Network (CNN)** to automatically classify surface images into two categories.

### Workflow

```text
Input Images
     |
     v
Dataset Validation
     |
     v
Train / Validation / Test Split
     |
     v
Image Preprocessing
     |
     v
Data Augmentation
     |
     v
CNN Model
     |
     v
Model Training
     |
     v
Model Evaluation
     |
     +-----------------------------+
     |                             |
     v                             v
Accuracy / Loss              Confusion Matrix
Graphs                       Classification Report
     |
     v
Single Image Prediction
     |
     +-------------------+
     |                   |
     v                   v
Crack Detected        No Crack
```

---

## Dataset

The original dataset contains two folders:

```text
CrackDataset/
│
├── Positive/
│   ├── image1.jpg
│   ├── image2.jpg
│   └── ...
│
└── Negative/
    ├── image1.jpg
    ├── image2.jpg
    └── ...
```

### Classes

| Folder | Class | Description |
|---|---|---|
| `Positive` | Crack | Images containing surface cracks |
| `Negative` | No Crack | Images without surface cracks |

The Python program maps these classes to:

```text
Crack
NoCrack
```

---

## Dataset Splitting

The program automatically divides the images into:

- **70%** Training
- **15%** Validation
- **15%** Testing

The generated dataset structure is:

```text
Processed_CrackDataset/
│
├── train/
│   ├── Crack/
│   └── NoCrack/
│
├── validation/
│   ├── Crack/
│   └── NoCrack/
│
└── test/
    ├── Crack/
    └── NoCrack/
```

A random seed of `42` is used to make the random operations reproducible.

> **Note:** The Python script creates a folder named `validation`, not `validate`.

---

## Image Preprocessing

All input images are resized to:

```text
128 × 128 pixels
```

Pixel values are normalized from approximately:

```text
0 - 255
```

to:

```text
0 - 1
```

### Training Data Augmentation

Training images use the following augmentation techniques:

- Rotation
- Zoom
- Width shifting
- Height shifting
- Horizontal flipping

Validation and test images are only normalized and are not augmented.

---

## CNN Architecture

The project uses a custom Convolutional Neural Network.

```text
Input Image
128 × 128 × 3
      |
      v
Conv2D - 32 Filters
      |
Batch Normalization
      |
Max Pooling
      |
      v
Conv2D - 64 Filters
      |
Batch Normalization
      |
Max Pooling
      |
      v
Conv2D - 128 Filters
      |
Batch Normalization
      |
Max Pooling
      |
      v
Conv2D - 256 Filters
      |
Batch Normalization
      |
Max Pooling
      |
      v
Flatten
      |
      v
Dense - 256
      |
Dropout - 0.5
      |
      v
Dense - 128
      |
Dropout - 0.3
      |
      v
Dense - 1
Sigmoid
      |
      v
Crack / No Crack
```

---

## Model Configuration

| Parameter | Value |
|---|---|
| Image Size | 128 × 128 |
| Batch Size | 32 |
| Maximum Epochs | 15 |
| Optimizer | Adam |
| Loss Function | Binary Crossentropy |
| Output Activation | Sigmoid |
| Random Seed | 42 |

---

## Training Callbacks

Three callbacks are used during training.

### 1. Early Stopping

Training monitors validation loss and can stop when the validation loss stops improving.

The best weights are restored after stopping.

### 2. Model Checkpoint

The best model based on validation accuracy is saved as:

```text
Best_Crack_Detection_Model.keras
```

### 3. Reduce Learning Rate

The learning rate is reduced when the validation loss reaches a plateau.

---

## Model Evaluation

After training, the model is evaluated on unseen test data.

The project calculates:

- Test Loss
- Test Accuracy
- Confusion Matrix
- Precision
- Recall
- F1-Score

A classification report is also generated.

---

## Visualizations

The project generates the following visualizations:

### Training vs Validation Accuracy

Displays training accuracy and validation accuracy across epochs.

### Training vs Validation Loss

Displays training loss and validation loss across epochs.

### Sample Training Images

A sample of training images is displayed before training.

---

## Single Image Prediction

The project also supports prediction on an individual image.

The image goes through:

1. Image loading
2. Resizing to `128 × 128`
3. Pixel normalization
4. CNN prediction
5. Binary classification

Possible outputs are:

```text
Crack Detected
```

or:

```text
No Crack
```

The prediction value is also displayed.

---

## Project Structure

The recommended GitHub repository structure is:

```text
Marvellous_CNN_Surface_Crack_Detection/
│
├── CNN_Surface_Crack_Detection.py
├── README.md
├── requirements.txt
├── .gitignore
│
├── CrackDataset/                 # Local dataset - not uploaded
│   ├── Positive/
│   └── Negative/
│
├── Processed_CrackDataset/       # Generated automatically - not uploaded
│   ├── train/
│   ├── validation/
│   └── test/
│
└── MarvellousCNN/                # Virtual environment - not uploaded
```

The dataset, processed dataset, virtual environment, and generated model files should not be committed to GitHub.

---

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/YOUR-USERNAME/Marvellous_CNN_Surface_Crack_Detection.git
```

Navigate into the project:

```bash
cd Marvellous_CNN_Surface_Crack_Detection
```

---

### 2. Create a Virtual Environment

For Windows:

```powershell
python -m venv MarvellousCNN
```

Activate the virtual environment:

```powershell
.\MarvellousCNN\Scripts\Activate.ps1
```

---

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

---

## Requirements

The project uses:

- Python
- TensorFlow
- Keras
- NumPy
- Matplotlib
- Scikit-learn

The required packages are listed in:

```text
requirements.txt
```

---

## Dataset Setup

Place the original dataset in the project root:

```text
Marvellous_CNN_Surface_Crack_Detection/
│
├── CrackDataset/
│   ├── Positive/
│   └── Negative/
│
├── CNN_Surface_Crack_Detection.py
├── README.md
├── requirements.txt
└── .gitignore
```

The program automatically creates the processed train, validation, and test folders.

---

## Running the Project

Activate the virtual environment and run:

```bash
python CNN_Surface_Crack_Detection.py
```

The program will:

1. Check whether the dataset folders exist.
2. Count positive and negative images.
3. Create the processed dataset.
4. Split the images into training, validation, and test sets.
5. Preprocess and augment the training images.
6. Build the CNN model.
7. Train the model.
8. Display training and validation results.
9. Evaluate the model on test data.
10. Generate the confusion matrix and classification report.
11. Save the trained models.
12. Perform a sample single-image prediction.

---

## Training Time

Training can take a significant amount of time depending on:

- Number of images
- Image size
- CPU/GPU availability
- System specifications
- Number of epochs

The current configuration uses:

```python
EPOCHS = 15
```

For a quick test run, temporarily change it to:

```python
EPOCHS = 3
```

Run the project to verify that everything works correctly.

After testing, change it back to:

```python
EPOCHS = 15
```

The project uses Early Stopping, so training may finish before all 15 epochs.

---

## Generated Model Files

The program generates:

```text
Best_Crack_Detection_Model.keras
Final_Marvellous_Crack_Detection_Model.keras
```

These trained model files are excluded from GitHub using `.gitignore` because they can be large.

---

## Results

The actual results should be recorded after completing a training run.

Example format:

```text
Test Accuracy : XX.XX%

Precision     : XX.XX%
Recall        : XX.XX%
F1-Score      : XX.XX%
```

The project does not claim a fixed accuracy because the actual metrics should come from the completed training run on the dataset.

---

## Technologies Used

- **Python**
- **TensorFlow**
- **Keras**
- **NumPy**
- **Matplotlib**
- **Scikit-learn**
- **Convolutional Neural Networks (CNN)**
- **Deep Learning**
- **Image Classification**
- **Image Augmentation**

---

## Future Improvements

Possible improvements include:

- Transfer learning using ResNet
- Transfer learning using MobileNet
- Transfer learning using EfficientNet
- Hyperparameter tuning
- Grad-CAM based visualization
- Streamlit web interface
- Real-time crack detection
- Model deployment
- Comparison of multiple CNN architectures
- Improved dataset balancing and preprocessing

---

## Author

**Shreya Borate**

Data Science | Machine Learning | Deep Learning | GenAI

---

## License

This project is intended for educational and learning purposes.
