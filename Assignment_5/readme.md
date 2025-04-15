# A5: Image Compression via Block-wise SVD

**Student:** Khushi Choudhary
**Course:** MATH/CSCI 485 - Spring 2025
**Assignment:** Assignment 5

## Objective

This project explores how we can compress grayscale images using a mathematical technique called Singular Value Decomposition (SVD). Instead of applying SVD to the whole image at once, we break the image into small 8x8 blocks and apply SVD to each block individually. We then rebuild the image using only the most important information (the top 'k' singular values) from each block's SVD. The goal is to see how changing 'k' (from 1 to 8) affects the compression amount and the visual quality of the resulting image.

## Implementation Summary

The process was carried out in the `Assignment5.ipynb` Jupyter Notebook using Python and common libraries like `numpy` (for math), `opencv-python` (for images), and `matplotlib` (for plots).

1.  **Preprocessing:**
    *   We started with the `cameraman.png` image (a 256x256 grayscale picture).
    *   We checked that its size is divisible by 8, which is necessary for breaking it into 8x8 blocks without leftovers.
    *   We converted the image's pixel data to a format suitable for precise math calculations (float64).
    *   We created a folder (`reconstructed_cameraman/`) to save the compressed images we generate.

2.  **`compress_block` Function:**
    *   We created a function called `compress_block` that takes one 8x8 image block and the number `k` as input.
    *   Inside the function, it performs SVD on the block. SVD breaks the block down into three matrices (U, Sigma, V-transpose) that represent different aspects of the block's data. Sigma contains the 'singular values' which tell us the importance of different features.
    *   The function then keeps only the top `k` most important singular values and the corresponding parts of the U and V-transpose matrices.
    *   It uses these reduced parts to reconstruct an approximation of the original 8x8 block.
    *   Finally, it returns this reconstructed (compressed) block.

3.  **Analysis Loop:**
    *   The main part of the code loops through `k` from 1 to 8.
    *   For each value of `k`:
        *   It goes through the original image, taking one 8x8 block at a time.
        *   It uses the `compress_block` function (described above) to compress and reconstruct each block using the current value of `k`.
        *   It puts these reconstructed blocks together to form a complete, new image for that specific `k`.
        *   It calculates the **Compression Ratio**: This tells us how much smaller the data *could* be. It's calculated as (data in original 8x8 block, which is 64 values) divided by (data needed for the k-SVD components, which turns out to be 17 * k values). A higher ratio means more compression.
        *   It calculates the **Reconstruction Error**: This measures how different the new compressed image is from the original. We used the Frobenius norm, which gives a single number representing the total pixel-wise difference. A lower error means the compressed image is closer to the original.
        *   It saves the reconstructed image for the current `k` as a standard PNG file.

## Results

Running the code produced the following numbers:

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

*(Note: The time taken for each k was also measured, typically very fast, around 0.02-0.05 seconds per k).*

**Plots and Visualizations:**

The Jupyter Notebook (`Assignment5.ipynb`) shows:
1.  A plot of **Compression Ratio vs. k**.
2.  A plot of **Reconstruction Error vs. k**.
3.  A side-by-side comparison of the **original image and the reconstructed images** for k=1, 2, 3, 4, 5, 6, and 8.

## Analysis of Results

**Compression Ratio:**
*   As we increase `k` (keep more information), the compression ratio goes down. This makes sense – keeping more data means less compression.
*   The best compression happens at `k=1`.
*   Interestingly, for `k=8` (keeping all information), the "compression ratio" is less than 1. This means storing the SVD parts actually takes *more* space than the original pixels. SVD compression is only useful when we discard some information (use `k < 8`).

**Reconstruction Error:**
*   As we increase `k`, the error (difference from the original) drops quickly, especially between `k=1` and `k=4`. This tells us the first few singular values capture the most important parts of the image blocks.
*   The error decreases more slowly for higher `k` values.
*   At `k=8`, the error is essentially zero, meaning we get back the original block perfectly (or almost perfectly).

**Visual Quality:**
*   Looking at the actual images confirms the error numbers.
*   With `k=1`, the image looks very blocky and lacks detail.
*   Quality gets much better for `k=2` and `k=3`, but you can still see the block boundaries.
*   Around `k=4` or `k=5`, the image looks pretty good, capturing most details well.
*   For `k=6` and `k=8`, the images look almost identical to the original.

**Trade-off Summary:**
This project clearly shows the trade-off:
*   Using a **small `k`** gives high compression (potentially smaller files) but results in lower image quality (more errors, blocky look).
*   Using a **large `k`** gives low compression (potentially larger files) but results in high image quality (low error, looks like the original).
Choosing the best `k` depends on what you need: maximum compression or maximum quality. For this specific image, a `k` value around 4 or 5 seems like a reasonable compromise between saving space and keeping the image looking good.

## How to Run

1.  **Dependencies:** Make sure you have Python and these libraries:
    *   `numpy`
    *   `matplotlib`
    *   `opencv-python`
    *   `jupyter` (to run the notebook)
    ```bash
    pip install numpy matplotlib opencv-python jupyterlab
    ```
2.  **Get Files:** Download or clone the project files.
3.  **Run Notebook:** Open `Assignment5.ipynb` in Jupyter Lab or Jupyter Notebook and run the cells one by one.
    *   *Note:* Check the `image_path` variable in the second code cell to make sure it points correctly to where you saved `cameraman.png`.
4.  **Output:** The compressed images will appear in the `reconstructed_cameraman/` folder. Plots and results are shown inside the notebook.

## Files in Repository

*   `Assignment5.ipynb`: The main Jupyter Notebook with code, results, and analysis.
*   `reconstructed_cameraman/`: Folder containing output images (created when you run the notebook).
*   `README.md`: This explanation file.
*   (Optional) `cameraman.png`: The input image, if you include it.

