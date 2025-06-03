```markdown
# Gray-Scale-To-RGB

A Neural Networks project that implements and compares two deep‐learning approaches for colorizing STL-10 grayscale images.

## Repository Structure

```

Gray-Scale-To-RGB/
├─ model1.ipynb                       # LiteColorizer (4-layer CNN, 128×128 inputs, MSE loss)
├─ model2.ipynb                       # FinalColorizer (ResNet-18 encoder + U-Net-style decoder + LSTM, 96×96 inputs, MSE+perceptual loss)
├─ main.ipynb                         # Combined comparison of Model 1 vs. Model 2
├─ best\_model.pt                      # Trained weights for Model 2
├─ model2result.png                   # Grid of 10 “Input / Prediction / Ground Truth” examples
├─ performance\_during\_training.png    # PSNR & SSIM curves for Model 2
└─ Neural Network Final Report\_BekirBerkYILDIRIM.pdf  # Detailed analysis and conclusions

````

## Requirements

- Python 3.8+  
- PyTorch 1.12+, torchvision 0.13+  
- scikit-image, tqdm, matplotlib  

Install dependencies:
```bash
pip install torch torchvision scikit-image tqdm matplotlib
````

## Quick Start

1. **Clone Repository**

   ```bash
   git clone https://github.com/username/Gray-Scale-To-RGB.git
   cd Gray-Scale-To-RGB
   ```

2. **Run Model 1 (LiteColorizer)**

   * Open `model1.ipynb` in Jupyter.
   * Execute all cells:

     * Loads STL-10 and downsamples images to 128×128.
     * Defines a 4-layer encoder–decoder.
     * Trains using MSE loss.
     * Displays sample “Input / Prediction / Ground Truth” outputs.

3. **Run Model 2 (FinalColorizer)**

   * Open `model2.ipynb`.
   * Execute each cell:

     * Caches all 96×96 STL-10 images in RAM for faster I/O.
     * Defines a frozen ResNet-18 encoder + U-Net-style decoder + LSTM refinement.
     * Trains with combined MSE + perceptual loss, printing PSNR & SSIM.
     * Visualizes 10 random test examples.

4. **Compare Both Models**

   * Open `main.ipynb`:

     * Runs Model 1 and Model 2 back-to-back.
     * Summarizes MSE, PSNR & SSIM for each.
     * Shows side-by-side visual comparisons.

## Key Findings

* **Model 1 (LiteColorizer)**

  * Pros: Fast to train; small memory footprint.
  * Cons: Downsampling to 128×128 lost fine details; shallow depth limited color accuracy; mismatch with STL-10’s native 96×96 resolution introduced interpolation artifacts.

* **Model 2 (FinalColorizer)**

  * Pros: Leverages ResNet-18 features for better semantic color choices; U-Net decoder + LSTM refinement reduces boundary artifacts.
  * Cons: Still limited to 96×96 inputs (512×512 was infeasible due to slow disk I/O and RAM limits); LSTM pixel-by-pixel is very slow (\~15 min/epoch); dataset size (5 k train, 8 k test) remained a bottleneck.

* **Classroom Benchmark**

  * A peer’s “raccoon” example achieved a 90/100 grade—the best in class—demonstrating high-quality fur texture colorization under similar constraints.

## Additional Resources

* **Detailed Report:** `Neural Network Final Report_BekirBerkYILDIRIM.pdf` (contains architecture diagrams, hyperparameters, failure analysis, and future work suggestions).
* **Sample Outputs:**

  * `model2result.png` (10-case comparison)
  * `performance_during_training.png` (metrics over epochs)

Feel free to explore or modify any notebook to reproduce and extend these experiments.

```
```
