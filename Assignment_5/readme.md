# A5: Image Compression via Block-wise SVD

**Student:** Khushi Choudhary
**Course:** MATH/CSCI 485 - Spring 2025
**Assignment:** Assignment 5

## Objective

This project explores the use of Singular Value Decomposition (SVD) for lossy image compression. The goal is to apply SVD block-wise (to 8x8 non-overlapping blocks) to a grayscale image, retaining only the top 'k' singular values for reconstruction. We analyze how the compression ratio and image quality (measured by reconstruction error and visual inspection) evolve as 'k' varies from 1 to 8.

## Implementation Summary

The process was implemented in the `Assignment5.ipynb` Jupyter Notebook using Python with `numpy`, `opencv-python`, and `matplotlib`.

1.  **Preprocessing:**
    *   The `cameraman.png` (256x256) grayscale image was loaded using OpenCV.
    *   Its dimensions were verified to be divisible by 8, ensuring compatibility with 8x8 block processing.
    *   The image data was converted from `uint8` to `float64` format, suitable for numerical SVD calculations.
    *   An output directory (`reconstructed_cameraman/`) was created to store results.

2.  **`compress_block` Function:**
    *   A function `compress_block(block, k)` was defined to handle the SVD compression for a single 8x8 block.
    *   It computes the SVD of the input block: \( \text{block} = U \Sigma V^T \) using `np.linalg.svd`.
    *   It truncates the SVD components by keeping only the first `k` columns of U (\( U_k \)), the first `k` singular values (\( s_k \)), and the first `k` rows of Vh (\( V_k^T \)).
    *   It reconstructs the block using the truncated components: \( \text{reconstructed\_block} = U_k \text{diag}(s_k) V_k^T \).
    *   The reconstructed 8x8 block is returned.

3.  **Analysis Loop:**
    *   The code iterates through \( k = 1, 2, ..., 8 \).
    *   For each `k`, the original image is processed block by block (8x8).
    *   The `compress_block` function is applied to each block.
    *   The reconstructed blocks are reassembled into a full reconstructed image for that `k`.
    *   The **Compression Ratio** is calculated as \( \frac{\text{Original Data per Block}}{\text{Retained Data per Block}} = \frac{64}{k \times (8 + 8 + 1)} = \frac{64}{17k} \).
    *   The **Reconstruction Error** is calculated using the Frobenius norm of the difference between the original float image and the reconstructed float image: \( ||\text{Original} - \text{Reconstructed}_k||_F \).
    *   Metrics (ratio, error) are stored, and the reconstructed image for each `k` is saved as a PNG file.

## Results

The analysis loop produced the following quantitative results:

**Results Table:**

| k | Compression Ratio | Reconstruction Error (Frobenius) |
|---|-------------------|----------------------------------|
| 1 | 3.7647            | 3660.8765                        |
| 2 | 1.8824            | 2067.9262                        |
| 3 | 1.2549            | 1225.9714                        |
| 4 | 0.9412            | 724.6354                         |
| 5 | 0.7529            | 397.3199                         |
| 6 | 0.6275            | 192.2857                         |
| 7 | 0.5378            | 51.4319                          |
| 8 | 0.4706            | 0.0000                           |

*(Note: Processing time per k was also measured and is available in the notebook output, typically around 0.02-0.05s per k on the test machine).*

**Plots and Visualizations:**

The Jupyter Notebook (`Assignment5.ipynb`) contains the following generated plots:
1.  **Compression Ratio vs. k:** Shows a decreasing trend as k increases.
2.  **Reconstruction Error vs. k:** Shows a sharply decreasing trend, especially for low k.
3.  **Image Comparison Grid:** Displays the original image alongside reconstructed images for k=1, 2, 3, 4, 5, 6, and 8, allowing for visual quality assessment.

## Analysis of Results

**Compression Ratio:**
*   The plot and table confirm that the compression ratio **decreases** hyperbolically as `k` increases (\( \propto 1/k \)). This is logical, as retaining more singular values requires storing more data (\(17k\) values per block).
*   The highest compression (\( \approx 3.76 \)) occurs at \( k=1 \).
*   For \( k=4 \), the ratio is slightly less than 1, meaning the compressed representation is slightly larger than the original pixel data if stored naively. For \( k=8 \), the ratio is \( \approx 0.47 \), meaning the SVD components (\( U_8, s_8, V_8^T \)) require significantly more storage than the original 64 pixel values per block. This emphasizes that practical SVD compression relies on \( k < 8 \).

**Reconstruction Error:**
*   The Frobenius norm error **decreases significantly** as `k` increases, particularly between \( k=1 \) and \( k=4 \). This indicates that the first few singular values capture the most dominant structural information within the 8x8 blocks.
*   The error reduction slows down for higher values of `k`.
*   At \( k=8 \), the error becomes zero (within floating-point precision), as all original information is used in the reconstruction (\( U \Sigma V^T \)).

**Visual Quality:**
*   Visual inspection of the reconstructed images (see notebook) aligns with the error metrics.
*   \( k=1 \) results in a very blocky image with severe detail loss.
*   Quality improves dramatically for \( k=2 \) and \( k=3 \), though artifacts remain.
*   \( k=4 \) and \( k=5 \) offer a good balance, retaining most important features with less obvious blockiness.
*   \( k=6 \) and \( k=8 \) produce images visually very close to the original.

**Trade-off Summary:**
The results demonstrate the fundamental trade-off in lossy compression:
*   **Lower `k`:** Higher compression ratio (smaller file size potential) but lower image quality (higher error, more artifacts).
*   **Higher `k`:** Lower compression ratio (larger file size potential) but higher image quality (lower error, fewer artifacts).
The optimal `k` depends on the desired balance between file size and fidelity for a given application. For the cameraman image, \( k \approx 4 \) or \( k \approx 5 \) seems to provide a reasonable compromise.

## How to Run

1.  **Dependencies:** Ensure you have Python installed with the following libraries:
    *   `numpy`
    *   `matplotlib`
    *   `opencv-python`
    *   `jupyter` (to run the notebook)
    ```bash
    pip install numpy matplotlib opencv-python jupyterlab
    ```
2.  **Clone/Download:** Get the project files.
3.  **Run Notebook:** Open and run the cells sequentially in `Assignment5.ipynb` using Jupyter Lab or Jupyter Notebook.
    *   *Note:* You may need to adjust the `image_path` variable in the second code cell if `cameraman.png` is not in the expected location relative to the notebook.
4.  **Output:** Reconstructed images will be saved in the `reconstructed_cameraman/` directory. Plots and analysis are displayed directly in the notebook.

## Files in Repository

*   `Assignment5.ipynb`: Jupyter Notebook containing the Python code, analysis, and generated outputs.
*   `reconstructed_cameraman/`: Directory containing the reconstructed images for k=1 through k=8 (generated upon running the notebook).
*   `README.md`: This report file.
*   (Potentially) `cameraman.png`: The input image used (if included in the repo).

