# 🖼️ Content-Based Image Retrieval (CBIR) System

This project implements a **Content-Based Image Retrieval (CBIR)** system using various image descriptor extraction techniques, including **Color Histogram**, **Average Color**, **Texture Descriptors**, **Cumulative Histogram**, and descriptors based on the **Gray-Level Co-occurrence Matrix (GLCM)**.

The primary goal is to retrieve images from a database that are semantically similar to a given query image by measuring the distance between their descriptor vectors.

---

## 🛠️ Technologies Used

* **Language:** Python
* **Libraries:**
    * `numpy`: For numerical operations and manipulation of image matrices and descriptors.
    * `matplotlib.pyplot`: For reading images (`plt.imread`).
    * `math`: For mathematical functions, notably calculating the square root.

---

## 🚀 System Functionality

The system follows the standard steps of a CBIR pipeline:

1.  **Preprocessing:** Conversion of color (RGB) images to **grayscale**.
2.  **Descriptor Extraction:** Calculation of feature vectors (descriptors) for each image in the database (training set) and for the query image (test set).
3.  **Distance Calculation:** Measurement of similarity between the query image's descriptor and the descriptors of the database images. **Euclidean distance** is used.
4.  **Evaluation:** Calculation of performance metrics (**Precision** and **Recall**) to assess the effectiveness of each descriptor.
5.  **Performance Improvement:** Creation of a **new combined descriptor** to enhance overall system precision.

---

## ✨ Implemented Descriptors and Performance

The notebook explores the effectiveness of several descriptors for the image retrieval task. The performance is evaluated using the **Average Precision at $K=1$ ($P@1$)**.

| Descriptor | Description | Python Function(s) | Average Precision |
| :--- | :--- | :--- | :--- |
| **Average Color** | Mean of the grayscale levels in the image. | `avrage_color` | **45.83%** |
| **Histogram** | Distribution of grayscale levels (256 bins). | `histo` | **58.33%** |
| **Simple Texture** | Combination of Variance, Energy, Entropy, Contrast, and Homogeneity (applied directly to the image). | `texture` | **29.17%** |
| **Cumulative Histogram** | Cumulative histogram of grayscale levels. | `histo_cum` | **70.83%** |
| **GLCM Texture** | Texture descriptors applied to the Gray-Level Co-occurrence Matrix (GLCM). | `cooccurence`, `texture_cooccurence` | **54.17%** |

### 📈 Performance Improvement

A combination approach was tested by creating a **combined descriptor** (`nouveau_mat_per`). This approach considers an image as **relevant** if it is classified as such by **at least one** of the individual descriptors (based on their respective performance matrices).
This combination achieved a significant improvement in average precision:
* **Average Precision of the New Descriptor: 91.67%**

---

## 💻 Code Structure (Notebook `notebook.ipynb`)

The project is implemented in a single Jupyter notebook, structured with functions for each operation:

| Category | Function | Description |
| :--- | :--- | :--- |
| **Utilities** | `rgb2gray(I)` | Converts an RGB image to grayscale. |
| | `distance(x, y)` | Calculates the Euclidean distance between two vectors. |
| **Descriptors** | `histo(I)` | Calculates the grayscale histogram. |
| | `histo_cum(I)` | Calculates the cumulative histogram. |
| | `avrage_color(I1)` | Calculates the average color. |
| | `variance(I)`, `energy(I)`, `entropie(I)`, `contrast(I)`, `homogenite(I)` | Base functions for texture features. |
| | `texture(I)` | Combination of basic texture descriptors. |
| | `cooccurence(I)` | Calculates the co-occurrence matrix (for a horizontal shift of 1). |
| | `texture_cooccurence(I)` | Texture descriptors applied to the co-occurrence matrix. |
| **Data Management** | `img_train(...)`, `img_test(...)` | Separate the image database into training and test sets. |
| | `mat_desc_train(...)`, `mat_desc_test(...)` | Calculate the descriptor matrices for the training and test sets. |
| **Evaluation** | `dist_mat(...)` | Calculates the performance matrix (relevance) $M$, where $M[i, j]=1$ if the $j$-th training image is considered the same class as the $i$-th test image based on minimal distance. |
| | `evalu(m)` | Calculates the Precision and Recall matrices at various cutoffs. |
| | `moyen_prec(prec)` | Calculates the Average Precision ($P@1$). |
| | `nouveau\_mat\_per(...)` | Implements the new combined descriptor. |


