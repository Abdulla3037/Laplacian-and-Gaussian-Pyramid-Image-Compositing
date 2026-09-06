# 🖼️ Image Pyramid Blending: Gaussian & Laplacian Compositing

Image blending and compositing using Gaussian and Laplacian pyramids decomposes each image into multiple frequency bands and blends them separately at each scale, instead of stitching two images together with a hard edge. This produces smooth, seamless transitions with no visible seam, even between images with very different colors and textures.

---

## 📑 Table of Contents

| # | Section |
|---|---------|
| 1 | [Project Overview](#-project-overview) |
| 2 | [Features](#-features) |
| 3 | [How the Algorithm Works](#-how-the-algorithm-works) |
| 4 | [Examples Included](#-examples-included) |
| 5 | [Project Structure](#-project-structure) |
| 6 | [How to Run](#-how-to-run) |
| 7 | [Key Functions](#-key-functions) |
| 8 | [Requirements](#-requirements) |
| 9 | [Key Insights](#-key-insights) |
| 10 | [Contributing](#-contributing) |
| 11 | [License](#-license) |

---

## 🎯 Features

- Custom Gaussian pyramid construction (progressive downsampling + blur)
- Custom Laplacian pyramid construction (high-frequency detail extraction)
- Binary and soft-alpha mask support, each with its own Gaussian pyramid
- Level-by-level pyramid blending using the mask pyramid
- Full image reconstruction from the blended pyramid
- Verified numerically near-zero reconstruction error on the original round trip
- Works on any image size, including non-power-of-two dimensions

---

## 🔍 How the Algorithm Works

Pyramid blending avoids the harsh seams of a naive alpha blend by blending at multiple frequency bands separately.

- **Step 1:** Build a Gaussian pyramid for each source image (repeated blur + downsample).
- **Step 2:** Derive each image's Laplacian pyramid the detail lost between consecutive Gaussian levels.
- **Step 3:** Build a Gaussian pyramid of the composite mask (binary or soft alpha).
- **Step 4:** Blend the two Laplacian pyramids at every level, weighted by the mask's Gaussian pyramid at that level: `blended = mask * L_a + (1 - mask) * L_b`.
- **Step 5:** Collapse the blended pyramid back into a single image by progressively upsampling and adding each Laplacian level.

---

## 🖼️ Examples Included

| # | Images | Blend Type |
|---|--------|-----------|
| 1 | Tiger & Bear | Vertical split, binary mask |
| 2 | Coffee & Tea | Vertical split, binary mask |
| 3 | Black Panther & Tiger | Vertical split, binary mask |
| 4 | Fox & Cat | Horizontal split, binary mask |
| 5 | Airplane & Ocean | Object compositing, soft alpha mask |

---

## ⚙️ How to Run

**Step 1: Install requirements**
```bash
pip install numpy pillow matplotlib jupyter
```

**Step 2: Open the notebook**
```bash
jupyter notebook Laplacian_and_Gaussian_Pyramid.ipynb
```
(Or open it directly in JupyterLab / VS Code / Google Colab.)

**Step 3: Run all cells**
Run the notebook top to bottom. It reads the source images from the `Image-Pyramid-Blending-Dip/` folder and saves its blended results back into that same folder.

---

## 🧩 Key Functions

| Function | Purpose |
|---|---|
| `gaussian_pyramid(img, levels)` | Builds the Gaussian pyramid |
| `laplacian_pyramid(gpyramid)` | Builds the Laplacian pyramid from a Gaussian pyramid |
| `reconstruct(lpyramid)` | Rebuilds the original image from a Laplacian pyramid |
| `vertical_split_mask` / `horizontal_split_mask` | Generate a binary composite mask |
| `blend_pyramids(la, lb, gmask)` | Blends two Laplacian pyramids using a mask's Gaussian pyramid |
| `pyramid_blend(img_a, img_b, mask, levels)` | End-to-end blend: builds all pyramids, blends, reconstructs |

---

## 🔧 Requirements

- Python 3
- NumPy
- Pillow (PIL)
- Matplotlib
- Jupyter Notebook / JupyterLab (or VS Code with the Jupyter extension)

---

## 💡 Key Insights

- **Reconstruction is lossless in practice** : near-zero mean absolute error between the original image and its Gaussian → Laplacian → reconstructed round trip, confirming the pyramid math (especially the ×4 energy compensation during upsampling) is implemented correctly.
- **Blending at multiple scales beats blending at one** : a naive pixel-wise blend produces a visible hard edge at the mask boundary, while pyramid blending hides that seam by blending low frequencies (color/lighting) more broadly and high frequencies (edges/texture) more locally.
- **The mask doesn't need to be binary** : the Airplane & Ocean example uses a soft alpha mask instead of a hard 0/1 split, showing the same pipeline generalizes to arbitrary object compositing, not just simple half-and-half blends.
- **Image size doesn't need to be a power of two** : the pyramid functions handle odd dimensions correctly by tracking exact shapes at each level during both downsampling and upsampling.

---

## 🤝 Contributing

Have ideas or improvements? Feel free to fork the repository, apply your changes, and submit a pull request.

---

## 🔐 License

This project is licensed under the [MIT License](./LICENSE).

---

## ✉️ Contact

For any questions or concerns, feel free to reach out by email at abdullahasan220618@gmail.com
