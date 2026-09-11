# Pneumonia Detection Using ResNet50 and Flask

## 📌 Project Overview

This project is a **Pneumonia Detection Web Application** that uses a
deep learning model based on **ResNet50** to classify a chest X-ray
image as either:

-   **Pneumonia**
-   **Normal**

The trained model is integrated into a **Flask web application**. A user
uploads a chest X-ray image, the application preprocesses the image,
sends it to the ResNet50 model, and returns the predicted class along
with the model confidence score.

> **Medical disclaimer:** This project is intended for educational and
> demonstration purposes only. It is not a medical diagnostic tool and
> should not be used to make healthcare decisions.

------------------------------------------------------------------------

## 🧠 How the Project Works

The application follows this workflow:

``` text
Chest X-ray Image
       ↓
Image Upload
       ↓
Flask API
       ↓
Resize Image to 224 × 224
       ↓
Normalize Pixel Values
       ↓
ResNet50 Model
       ↓
Sigmoid Prediction
       ↓
Classification
       ↓
Normal / Pneumonia + Confidence
```

------------------------------------------------------------------------

## 🛠️ Technologies Used

-   Python
-   TensorFlow
-   Keras
-   ResNet50
-   NumPy
-   Flask
-   h5py
-   HTML/CSS/JavaScript (for the optional frontend)

------------------------------------------------------------------------

## 📁 Project Structure

``` text
pneumonia-detection/
│
├── app.py
├── best_resnet1.weights.h5
│
├── templates/
│   └── index.html
│
├── static/
│   ├── css/
│   ├── js/
│   └── images/
│
└── README.md
```

### Important files

  File                        Purpose
  --------------------------- ----------------------------------------
  `app.py`                    Flask application and prediction logic
  `best_resnet1.weights.h5`   Trained ResNet50 weights
  `templates/index.html`      Web interface
  `README.md`                 Project documentation

------------------------------------------------------------------------

## 🧩 Model Architecture

The application creates a ResNet50-based binary classification model.

``` text
Input Image
224 × 224 × 3
       ↓
ResNet50
       ↓
Global Average Pooling 2D
       ↓
Dense Layer
256 neurons
ReLU activation
       ↓
Dropout
0.5
       ↓
Dense Layer
1 neuron
Sigmoid activation
       ↓
Prediction
```

The ResNet50 base is created without the original ImageNet
classification head:

``` python
ResNet50(
    weights=None,
    include_top=False,
    input_shape=(224, 224, 3)
)
```

A `GlobalAveragePooling2D` layer is then used, followed by a 256-unit
dense layer, dropout, and a single sigmoid output for binary
classification.

------------------------------------------------------------------------

## 🔍 Image Preprocessing

Before prediction, every uploaded image is:

1.  Loaded from the uploaded file.
2.  Resized to **224 × 224 pixels**.
3.  Converted into a NumPy array.
4.  Divided by `255.0` to normalize pixel values.
5.  Expanded with a batch dimension.

Conceptually:

``` text
Original X-ray
      ↓
224 × 224
      ↓
Pixel normalization
      ↓
Model input
```

------------------------------------------------------------------------

## 🤖 Prediction Logic

The model produces a sigmoid value between `0` and `1`.

The application uses a threshold of **0.5**:

``` text
prediction >= 0.5
        ↓
    Pneumonia

prediction < 0.5
        ↓
      Normal
```

The API returns:

``` json
{
    "confidence": 0.87,
    "class": "Pneumonia"
}
```

The exact confidence value depends on the uploaded image and trained
model weights.

------------------------------------------------------------------------

## 🌐 Flask API

### Home Route

``` text
GET /
```

This route loads the web interface from:

``` text
templates/index.html
```

### Prediction Route

``` text
POST /predict
```

The image should be uploaded using the form field:

``` text
file
```

Example response:

``` json
{
    "confidence": 0.23,
    "class": "Normal"
}
```

------------------------------------------------------------------------

## ⚙️ Installation

### 1. Clone or download the project

Open a terminal in the project folder.

### 2. Create a virtual environment

``` bash
python -m venv venv
```

### 3. Activate the environment

#### Windows

``` bash
venv\Scripts\activate
```

#### Linux / macOS

``` bash
source venv/bin/activate
```

### 4. Install dependencies

``` bash
pip install flask tensorflow numpy h5py
```

------------------------------------------------------------------------

## 📦 Requirements

A `requirements.txt` file can contain:

``` text
Flask
numpy
tensorflow
h5py
```

Install them with:

``` bash
pip install -r requirements.txt
```

------------------------------------------------------------------------

## 🧠 Model Weights

The application expects the trained weights file:

``` text
best_resnet1.weights.h5
```

The code checks whether this file exists before loading it.

If the file is missing, the application prints:

``` text
❌ Weights file not found: best_resnet1.weights.h5
```

Make sure the weights file is located in the same directory as `app.py`,
unless the path is changed in the code.

------------------------------------------------------------------------

## ▶️ Run the Application

Start the Flask server:

``` bash
python app.py
```

The application runs using:

``` python
app.run(debug=True)
```

Then open the local Flask address shown in the terminal, typically:

``` text
http://127.0.0.1:5000/
```

------------------------------------------------------------------------

## 🖼️ Using the Application

1.  Open the web application.
2.  Select a chest X-ray image.
3.  Upload the image.
4.  The Flask server receives the image.
5.  The image is resized and normalized.
6.  ResNet50 performs the prediction.
7.  The application displays:
    -   Predicted class
    -   Confidence score

------------------------------------------------------------------------

## 🔐 Error Handling

The prediction API checks whether a file was uploaded.

If no file is provided:

``` json
{
    "error": "No file uploaded"
}
```

If the user submits an empty file selection:

``` json
{
    "error": "No selected file"
}
```

------------------------------------------------------------------------

## 📊 Example Results

### Normal X-ray

``` text
Prediction: Normal
Confidence: model-dependent
```

### Pneumonia X-ray

``` text
Prediction: Pneumonia
Confidence: model-dependent
```

The confidence score should be interpreted as the model's output, not as
a clinical probability or diagnosis.

------------------------------------------------------------------------

## 🚀 Future Improvements

Possible improvements include:

-   Add a better frontend UI.
-   Display the uploaded X-ray preview.
-   Add Grad-CAM heatmaps to visualize areas influencing the prediction.
-   Add model performance metrics such as accuracy, precision, recall,
    F1-score, and ROC-AUC.
-   Add a proper validation/test pipeline.
-   Add secure file-name handling.
-   Restrict uploaded file types and file sizes.
-   Deploy the application using a production WSGI server.
-   Add logging and better exception handling.
-   Add model versioning.
-   Add a clear medical disclaimer in the user interface.

------------------------------------------------------------------------

## ⚠️ Important Medical Disclaimer

This application is an **AI/deep-learning demonstration project**.
Predictions from the model may be incorrect.

It should **not** be used as a substitute for a radiologist, physician,
or other qualified healthcare professional.

Never use the prediction from this application alone to diagnose or rule
out pneumonia.

------------------------------------------------------------------------

## 👨‍💻 Project Summary

**Project:** Pneumonia Detection Using Deep Learning\
**Model:** ResNet50\
**Framework:** TensorFlow/Keras\
**Backend:** Flask\
**Input:** Chest X-ray image\
**Output:** Normal or Pneumonia + confidence score\
**Classification Type:** Binary Classification

------------------------------------------------------------------------

## 📄 License

This project is intended for educational and research purposes. Add an
appropriate open-source license if you plan to publish the project
publicly.
