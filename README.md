
# Digital Image Representation and Coding

**Course:** Multidimensional Visual Representation and Compression
**University:** Mid Sweden University

This lab explores how digital images are represented through sampling, quantization, color components, and statistical properties. The experiments use a standard grayscale test image and a separate color photograph to examine how reducing image information affects visual quality and how relationships between pixels can be used for more efficient representation.

## 1. Image Sampling and Aliasing

### 1.1 Downsampling the grayscale image

The `512 × 512` camera image was used to study the effect of reducing spatial resolution.

Downsampling was performed using factors of **2, 4, and 8**, initially by selecting every second, fourth, or eighth pixel.

Two approaches were compared:

- Downsampling without prefiltering.
- Downsampling after applying a Gaussian low-pass filter.

For the filtered images, the Gaussian standard deviation was set to half the downsampling factor (`sigma = factor / 2`).

The resulting images were compared visually to observe how image details changed as fewer samples were retained.

Without prefiltering, downsampling introduced aliasing, particularly around edges and fine image structures. Gaussian filtering reduced these effects by suppressing high-frequency components before sampling, although some sharpness was lost.

### 1.2 Frequency-domain analysis

The Fourier magnitude spectrum was calculated to examine how downsampling affected image frequencies.

The original and downsampled images were compared using log-magnitude spectra.

The frequency-domain results helped explain the visual differences between filtered and unfiltered downsampling, particularly the importance of limiting high-frequency content before reducing the sampling rate.

### 1.3 Sampling a color photograph

The same downsampling factors were applied to a separate photograph after converting it to grayscale.

This provided another comparison using an image containing different textures, boundaries, and fine details.

A more focused comparison was also performed for downsampling by a factor of 4, including the corresponding frequency spectra.

## 2. Image Quantization

### 2.1 Uniform grayscale quantization

The grayscale camera image was quantized using:

- 1 bit
- 2 bits
- 3 bits
- 4 bits
- 6 bits
- 8 bits

The image intensities were first normalized to the range `[0, 1]`.

For each bit depth, the available intensity values were restricted to `2^bits` uniformly spaced reconstruction levels.

The quantization operation was implemented using:

```python
M = 2 ** bits
quantized = np.round(np.clip(image, 0, 1) * (M - 1)) / (M - 1)
```

The reconstructed images were displayed together to compare the effect of reducing the number of intensity levels.

At lower bit depths, smooth intensity transitions became less continuous, producing visible banding and loss of tonal detail. Increasing the bit depth reduced these effects.

### 2.2 Quantization error and SQNR

Quantization performance was evaluated using:

- Mean Squared Error (MSE)
- Signal-to-Quantization-Noise Ratio (SQNR)

MSE measures the average squared difference between the original and quantized images.

SQNR compares the average signal power with the average quantization-error power.

Both measurements were calculated for the tested bit depths and plotted to examine how reconstruction quality changed.

A theoretical reference trend of approximately **6 dB SQNR improvement per additional bit** was also included for comparison.

This reference describes an approximate high-resolution quantization trend rather than an exact result for every image.

### 2.3 Color image quantization

The color photograph was quantized by applying the same uniform quantization procedure independently to its RGB channels.

Reconstructed images using **2, 4, and 8 bits per channel** were displayed.

MSE and SQNR were calculated for the tested bit depths, and the measured SQNR values were compared with the theoretical reference trend.

A separate plot excluding the 8-bit endpoint was used to examine the lower-bit-depth measurements more clearly.

## 3. Color Representation and Chroma Subsampling

### 3.1 RGB to YCbCr conversion

The color photograph was converted from RGB to YCbCr.

The three components were examined separately:

- **Y:** Luminance information
- **Cb:** Blue-difference chroma information
- **Cr:** Red-difference chroma information

Displaying the components separately helped distinguish the image's brightness structure from its color information.

### 3.2 Chroma subsampling

Three chroma sampling arrangements were compared.

| Format | Implementation |
|---|---|
| 4:4:4 | Full-resolution luminance and chroma |
| 4:2:2 | Horizontal pairs of chroma samples averaged |
| 4:2:0 | Chroma samples averaged over 2 × 2 neighborhoods |

For the subsampled versions, the reduced chroma components were resized back to their original dimensions before conversion to RGB.

The reconstructed color images were then compared visually.

Reducing chroma resolution affected color detail, particularly around small colored boundaries, while the luminance component retained its original spatial resolution.

## 4. Information Theory and Image Entropy

### 4.1 Entropy of probability distributions

Shannon entropy was calculated using:

\[
H(X)=-\sum_i p_i\log_2 p_i
\]

The entropy function was tested using two probability distributions:

- A fair six-sided die.
- A biased six-sided die with probabilities `[0.10, 0.20, 0.10, 0.20, 0.20, 0.20]`.

The experiment compared uncertainty in uniform and nonuniform distributions.

Joint probability distributions were also constructed for independent dice to examine the relationship between individual and joint entropy.

### 4.2 Image entropy and histograms

Pixel histograms were calculated for the grayscale camera image and the grayscale version of the color photograph.

The normalized histograms were used as probability distributions for estimating image entropy.

A sequence of **100,000 independent samples** was also generated from an image histogram.

The experiment compared image pixel statistics with an independently sampled sequence having the same marginal intensity distribution.

This helped separate the information contained in an intensity histogram from the spatial relationships between neighboring pixels.

### 4.3 Joint entropy, conditional entropy, and mutual information

Horizontally neighboring pixel pairs were extracted from the grayscale photograph.

Their joint distribution was used to estimate:

- Joint entropy
- Conditional entropy
- Mutual information

These measurements were used to examine how much information neighboring pixels share.

Additional synthetic experiments with independent and dependent random variables were included to compare different levels of statistical dependence.

## 5. Predictive Coding

### 5.1 Spatial prediction

A simple image predictor was implemented using the average of the left and upper neighboring pixels.

The predicted value was calculated from previously available neighboring samples.

Prediction residuals were obtained by subtracting the predicted values from the original pixel values.

The residual image was displayed, and its statistical distribution and entropy were examined.

Because nearby image pixels are often correlated, prediction can reduce redundancy by representing the differences between pixels rather than their original intensities.

The original image and prediction residuals were compared to examine this effect.

### 5.2 Correlated AR(1) sequences

Autoregressive sequences were generated to study how correlation affects statistical behavior and predictability.

The experiments used sequences containing **100,000 samples**.

An initial experiment used `alpha = 0.9`, followed by comparisons with:

- `alpha = 0.2`
- `alpha = 0.5`
- `alpha = 0.9`
- `alpha = 0.99`

Changing the correlation parameter made it possible to observe how strongly each sample depended on the preceding sample.

These experiments provided a simple statistical comparison with the dependencies observed between neighboring image pixels.

## Main Observations

The experiments showed how different representation methods affect the information retained in an image.

Sampling without sufficient filtering introduced aliasing, while Gaussian prefiltering reduced these artifacts at the cost of some detail.

Quantization reduced the number of available intensity values, with lower bit depths producing more visible distortion and higher quantization error.

Chroma subsampling demonstrated that color resolution can be reduced separately from luminance resolution.

The entropy and prediction experiments showed how statistical dependencies between neighboring pixels can be used to reduce redundancy, which is an important idea behind predictive image coding.
