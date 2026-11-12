# Lab 1 - Sampling, Color and Information in Images

[Open the notebook](DT096A_Lab1.ipynb)

This notebook follows the progression from how an image is sampled to how redundancy appears in image data. The main comparisons use `skimage.data.camera()` and a separately loaded color photograph. The notebook still contains the original plots and printed measurements.

## 3.1 Sampling and aliasing

The camera image is downsampled by **2, 4 and 8**. Results without prefiltering are compared with results using a Gaussian filter before resizing. The image and Fourier magnitude-spectrum plots help show what happens to fine detail and high-frequency content. A second comparison repeats the experiment on a grayscale version of the personal photograph.

**What to look for:** filtering before downsampling reduces the high-frequency content that can fold into lower frequencies. The cost is some smoothing, so the comparison is not simply about which picture looks sharper.

## 3.2 Quantization and SQNR

Uniform quantization is tested at **1, 2, 3, 4, 6 and 8 bits**. The implementation rounds normalized intensities to one of `2^bits` representable levels. Reconstruction error, mean-squared error (MSE) and signal-to-quantization-noise ratio (SQNR) are measured. Plots compare the measured SQNR increase with a reference slope of roughly **6 dB per bit**. Similar tests are carried out for the color photograph.

**What to look for:** the lower-bit results show much more visible banding; more quantization levels generally reduce distortion. The 6 dB line is a useful reference, not a guaranteed exact measurement on every image.

## 3.3 Color representation and chroma subsampling

The color photo is converted to **YCbCr**, separating luminance from chroma. The chroma planes are then reduced using **4:2:2** and **4:2:0** patterns and resized for reconstruction. The notebook compares reconstructed **4:4:4, 4:2:2 and 4:2:0** images side by side.

**What to look for:** chroma can often be reduced more aggressively than brightness without an equally large perceived loss, although fine color boundaries can suffer.

## 4. Information theory and prediction

The final section looks at information content in a few ways:

- Entropy of fair and biased dice, including joint entropy
- Pixel-value entropy and i.i.d. sequence experiments
- Joint/conditional entropy and mutual-information calculations for neighboring pixels and synthetic data
- A spatial predictor based on the average of the left and upper pixels, with prediction residuals
- Autoregressive **AR(1)** sequences, including comparisons across different correlation parameters

These experiments connect image redundancy with the idea behind predictive coding: when nearby values are related, the residual may be easier to describe than the original value.

## Opening or running

The notebook can be opened on GitHub with its saved figures intact. To rerun the sections involving the personal photograph, replace the Google Drive path `/content/drive/MyDrive/DT096A_Lab1/image-1.jpeg` with the location of an image available in the runtime. The original personal photograph was not included in the uploaded files, so it is **not** silently replaced with another image.

Dependencies: `numpy`, `matplotlib`, `Pillow`, `scikit-image`, `opencv-python`; the notebook also uses Colab Drive mounting for the personal photo. Execute cells in order because some later cells reuse previously defined variables.

## A practical note

The notebook contains exploratory cells as well as the main experiment sections. They have been retained with their outputs rather than rewriting the workflow into a new experiment after the fact.
