# Image PCA Visualization using Python

This project demonstrates how to apply **Principal Component Analysis (PCA)** to an RGB image using Python. The image is loaded, converted into a NumPy array, reshaped into pixel-level RGB data, transformed using PCA, and then visualized as a new RGB image using the first three principal components.

## Project Overview

The main goal of this notebook is to understand how PCA can be applied to image data. Each pixel in an RGB image contains three colour values: Red, Green, and Blue. PCA transforms these original colour channels into new components called principal components. These components show the main directions of variation in the image colour data.

The notebook compares:

* The original RGB image
* The PCA-transformed image where:

  * PC1 is displayed as the red channel
  * PC2 is displayed as the green channel
  * PC3 is displayed as the blue channel

## Features

* Loads an image using PIL
* Handles RGB image conversion
* Converts the image into a NumPy array
* Reshapes image data for PCA
* Applies PCA using scikit-learn
* Prints the explained variance ratio
* Scales PCA values for image display
* Visualizes the original and PCA-transformed images side by side
* Includes support for truncated or damaged image files using `ImageFile.LOAD_TRUNCATED_IMAGES`

## Technologies Used

* Python
* Google Colab
* NumPy
* Matplotlib
* Pillow
* scikit-learn

## Required Libraries

Install the required libraries using:

```bash
pip install numpy matplotlib pillow scikit-learn
```

If you are using Google Colab, most of these libraries are already installed.

## How to Use

1. Clone this repository:

```bash
git clone https://github.com/your-username/your-repository-name.git
```

2. Open the notebook in Google Colab or Jupyter Notebook.

3. Upload your image file to the working directory.

4. Make sure the image file name matches the code. For example:

```python
img = Image.open("00.jpg").convert("RGB")
```

If your image has a different name, update the file name in the code.

5. Run all cells in the notebook.

## Main Code Workflow

### 1. Import Libraries

```python
from PIL import Image, ImageFile
import numpy as np
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA
from sklearn.preprocessing import MinMaxScaler
```

These libraries are used for image loading, numerical processing, PCA transformation, scaling, and visualization.

### 2. Load the Image

```python
ImageFile.LOAD_TRUNCATED_IMAGES = True

img = Image.open("00.jpg").convert("RGB")
img = np.array(img)
```

The image is opened using PIL and converted into RGB format. It is then converted into a NumPy array so it can be processed mathematically.

The line below helps load truncated or slightly damaged image files:

```python
ImageFile.LOAD_TRUNCATED_IMAGES = True
```

However, the best solution is always to use a clean, undamaged image file.

### 3. Check Image Shape

```python
H, W, C = img.shape
assert C == 3, f"Expected RGB image with 3 channels, got {C}"
```

This checks that the image has three colour channels: Red, Green, and Blue.

### 4. Reshape the Image for PCA

```python
X = img.reshape(-1, 3).astype(np.float32)
```

The image is reshaped from:

```text
Height × Width × 3
```

into:

```text
Number of pixels × 3
```

Each row represents one pixel, and each column represents a colour channel.

### 5. Apply PCA

```python
pca = PCA(n_components=3)
X_pca = pca.fit_transform(X)
```

PCA transforms the original RGB values into three principal components.

### 6. Display Explained Variance Ratio

```python
print("Explained variance ratio:", pca.explained_variance_ratio_)
```

This shows how much colour variation is captured by each principal component.

Example:

```text
Explained variance ratio: [0.85, 0.10, 0.05]
```

This means:

* PC1 explains 85% of the variation
* PC2 explains 10% of the variation
* PC3 explains 5% of the variation

### 7. Scale PCA Values for Display

```python
scaler = MinMaxScaler(feature_range=(0, 1))
X_pca_scaled = scaler.fit_transform(X_pca)
```

PCA values may contain negative numbers, so they need to be scaled between 0 and 1 before displaying them as an image.

### 8. Reshape Back to Image Format

```python
pca_rgb = X_pca_scaled.reshape(H, W, 3)
```

The PCA-transformed pixel data is reshaped back into image format.

### 9. Visualize the Result

```python
plt.figure(figsize=(12, 5))

plt.subplot(1, 2, 1)
plt.imshow(img)
plt.title("Original RGB")
plt.axis("off")

plt.subplot(1, 2, 2)
plt.imshow(pca_rgb)
plt.title("PCA components as RGB (PC1, PC2, PC3)")
plt.axis("off")

plt.tight_layout()
plt.show()
```

This displays the original image and the PCA image side by side.

## Output

The notebook produces:

1. The explained variance ratio of the three PCA components
2. A side-by-side image comparison:

   * Original RGB image
   * PCA-transformed image

## Example Output

```text
Explained variance ratio: [0.91, 0.07, 0.02]
```

This means most of the image colour variation is captured by the first principal component.

## Common Error

You may get this error:

```text
OSError: image file is truncated
```

This means the image file is corrupted or incomplete.

To temporarily fix it, use:

```python
from PIL import ImageFile
ImageFile.LOAD_TRUNCATED_IMAGES = True
```

However, the recommended fix is to replace the image with a clean copy.

## Project Structure

```text
project-folder/
│
├── README.md
├── image_pca_visualization.ipynb
├── 00.jpg
└── requirements.txt
```

## Applications

This project can be useful for:

* Understanding PCA visually
* Learning image processing basics
* Exploring dimensionality reduction
* Analysing colour variation in images
* Practising NumPy, PIL, Matplotlib, and scikit-learn

## Limitations

This PCA transformation is mainly for visualization and analysis. It does not improve image quality. The PCA image may look different from the original image because the RGB channels are replaced by principal components.

Also, forcing Python to load a truncated image may produce inaccurate visual results.

## Author

Muhammad Abubakr

## License

This project is for educational purposes. You may modify and use it for learning, assignments, and portfolio work.

