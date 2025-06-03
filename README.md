```markdown
# Gray-Scale-To-RGB

This repository contains the code, notebooks, and trained models for my Neural Networks final project, in which I explored methods to colorize grayscale images using deep learning. Two main approaches (Model 1 and Model 2) were implemented and compared on the STL-10 dataset, along with in-memory caching experiments. You will also find sample outputs, performance plots, and a final written report.

---

## 📁 Repository Contents

```

Gray-Scale-To-RGB/
├─ .gitignore
├─ Neural Network Final Report\_BekirBerkYILDIRIM.pdf
├─ best\_model.pt
├─ main.ipynb
├─ main\_inmemory\_final.ipynb
├─ model1.ipynb
├─ model2.ipynb
├─ model2result.png
├─ performance\_during\_training.png
├─ README.md

````

- **Neural Network Final Report_BekirBerkYILDIRIM.pdf**  
  A PDF summarizing both Model 1 and Model 2, their performance (MSE, PSNR, SSIM), failure analysis, and conclusions.

- **model1.ipynb**  
  Jupyter notebook for **Model 1 (LiteColorizer)**:  
  - Lightweight four-layer encoder–decoder  
  - Trained on 128×128 inputs using only MSE loss  
  - Covers data loading, model definition, training, validation metrics, and example visualizations

- **model2.ipynb**  
  Jupyter notebook for **Model 2 (ResNet + LSTM Refinement)**:  
  - Uses a frozen ResNet-18 encoder for semantic feature extraction  
  - Coarse upsampling + pixel-wise LSTM refinement  
  - Includes training loop, validation metrics, PSNR/SSIM calculations, and sample outputs

- **main.ipynb**  
  A consolidated notebook that runs both Model 1 and Model 2 sequentially, showing the end-to-end pipeline and final comparisons

- **main_inmemory_final.ipynb**  
  Similar to `main.ipynb`, but uses in-RAM caching of the entire STL-10 dataset for faster I/O

- **best_model.pt**  
  Trained weights for the best performing variant of Model 2. Load these weights into the corresponding architecture definition to reproduce the visual results in `model2.ipynb`

- **model2result.png**  
  A grid of 10 test examples (Input, Prediction, Ground Truth) produced by the final version of Model 2

- **performance_during_training.png**  
  A plot showing training/validation loss curves (MSE + perceptual) and PSNR/SSIM metrics over epochs for Model 2

---

## 🚀 How to Run

Below steps assume a Python 3.8+ environment with `pip` available. Adjust as needed for your environment.

### 1. Clone the repository

```bash
git clone https://github.com/username/Gray-Scale-To-RGB.git
cd Gray-Scale-To-RGB
````

### 2. Install dependencies

Create a `requirements.txt` with the following contents:

```
torch>=1.12.0
torchvision>=0.13.0
scikit-image>=0.19.0
tqdm>=4.60.0
matplotlib>=3.4.0
```

Then install:

```bash
pip install -r requirements.txt
```

If you do not have a `requirements.txt`, you can install dependencies directly:

```bash
pip install torch torchvision scikit-image tqdm matplotlib
```

### 3. Download STL-10 dataset

The notebooks use `torchvision.datasets.STL10(download=True)` to fetch STL-10 automatically. If you prefer manual download, place the unzipped `stl10_binary/` folder under a `./data/` directory (so that the path is `./data/stl10_binary/`).

### 4. Run Model 1 (LiteColorizer)

Open and execute `model1.ipynb`:

1. Launch JupyterLab or Jupyter Notebook:

   ```bash
   jupyter lab
   ```
2. Navigate to `model1.ipynb`.
3. Run each cell in order:

   * Cell 1: Imports and transforms
   * Cell 2: Data caching and DataLoader creation
   * Cell 3: LiteColorizer architecture
   * Cell 4: Training loop (MSE only)
   * Cell 5: Metrics and sample visualizations

Outputs:

* Saves `model1_best.pt` (weights)
* Plots of training/validation loss
* Ten “Input – Prediction – Ground Truth” images

### 5. Run Model 2 (ResNet + LSTM Refinement)

Open and run `model2.ipynb`:

1. Cell 1: In-RAM caching of all 96×96 STL-10 images (train/test)
2. Cell 2: Imports, VGG perceptual loss definition, optional GAN discriminator
3. Cell 3: FinalColorizer definition (multi-scale U-Net-style decoder, no last skip)
4. Cell 4: Dual-loss training loop (MSE + Perceptual)

   * Saves weights as `improved2_best.pt` or similar
   * Logs PSNR & SSIM per epoch
5. Cell 5: Visualization of 10 random test images (loads saved `.pt`)

### 6. Review Combined Comparison

* `main.ipynb` runs both Model 1 and Model 2 back-to-back. It contains the final comparison table (MSE, PSNR, SSIM) and side-by-side visualizations.
* `main_inmemory_final.ipynb` is identical to `main.ipynb`, but uses in-RAM caching for faster data loading.

---

## 📄 Detailed Report

Refer to **“Neural Network Final Report\_BekirBerkYILDIRIM.pdf”** for:

* Full descriptions of Model 1 and Model 2 architectures
* Hyperparameter choices and training details
* Quantitative results (tables and plots)
* Qualitative analysis (10 test examples with commentary)
* Failure analysis (explanations of underperformance)
* Comparative discussion and future work suggestions

---

## 🔬 Key Insights & Failure Points

1. **Model 1 (LiteColorizer)**

   * **Pros**:

     * Fast to train and small memory footprint
     * Learns coarse semantic color mapping quickly
   * **Cons**:

     * Could never use 512×512 grayscale inputs due to slow disk I/O and RAM limits
     * Downsampling to 128×128 lost high-frequency details and textures
     * Only four convolutional layers – insufficient capacity for complex color distributions
     * Interpolation artifacts introduced by scaling 96×96 STL-10 images to 128×128

2. **Model 2 (ResNet + LSTM Refinement)**

   * **Pros**:

     * Leverages frozen ResNet-18 for semantic feature extraction
     * Improved object-level color consistency
   * **Cons**:

     * Pixel-by-pixel LSTM refinement is extremely slow (\~15 min per epoch on 96×96 inputs)
     * Still limited by 96×96 resolution – fine details remain blurred
     * Dataset size (5 k train, 8 k test) is too small for a large model – overfitting risk

3. **Best Classroom Example**

   * A classmate’s “raccoon” notebook achieved a grade of 90/100—the highest in class—demonstrating strong performance on fur texture and natural colorization. Their code/notebooks are included in the repository’s “raccoon\_example” folder for reference.

---

## 🔗 External Links

* **STL-10 dataset**
  [https://cs.stanford.edu/\~acoates/stl10/](https://cs.stanford.edu/~acoates/stl10/)

* **PyTorch Documentation**
  [https://pytorch.org/docs/stable/](https://pytorch.org/docs/stable/)

* **Torchvision Models (ResNet, VGG)**
  [https://pytorch.org/vision/stable/models.html](https://pytorch.org/vision/stable/models.html)

---

Thank you for your time and feedback! If there are any questions or suggestions, please feel free to reach out.

```
```
