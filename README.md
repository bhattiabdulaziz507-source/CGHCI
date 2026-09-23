Week 02 Lab – Image Processing

Perceptual Computing / Computer Graphics and Human-Computer Interaction

This repository contains my Week 02 Lab work on basic image processing using Python and NumPy.

The lab follows a four-step approach for each problem:

Inputs and output

Mathematical rule

Small manual example

Python implementation and testing

Student Information

Name: Abdul Aziz

Roll Number: 2K24/CSME/2

Course: Perceptual Computing / Computer Graphics and Human-Computer Interaction

Repository: CGHCI

Lab: Week 02

Technologies Used

Python 3

NumPy

Jupyter Notebook / Google Colab

Worked Samples

1. RGB to Grayscale

A color pixel has red, green, and blue components. The grayscale luminosity value is calculated using:

Y = 0.299R + 0.587G + 0.114B

For (R, G, B) = (180, 120, 60):

Y = 0.299(180) + 0.587(120) + 0.114(60)
  = 131.1

After rounding:

131

Python implementation:

def rgb_to_grayscale(r: int, g: int, b: int) -> int:
    luminosity = 0.299 * r + 0.587 * g + 0.114 * b
    return int(round(luminosity))

2. Brightness and Contrast Adjustment

The brightness and contrast operation uses:

O(x,y) = alpha * I(x,y) + beta

The output is clipped to the valid image range:

0 to 255

Example:

Input:
[[100, 150],
 [200,  50]]

alpha = 1.5
beta = -20

Expected output:

[[130, 205],
 [255,  55]]

Python implementation:

import numpy as np

def adjust_brightness_contrast(image: np.ndarray, alpha: float, beta: float) -> np.ndarray:
    image_float = image.astype(np.float32)
    adjusted = alpha * image_float + beta
    return np.clip(adjusted, 0, 255).astype(np.uint8)

Five Problems

1. Thresholding

Thresholding converts a grayscale image into black (0) and white (255).

Rule

If pixel >= threshold → 255
Otherwise → 0

Example with threshold 128:

Input:
20   128  200
100  150  250

Output:
0    255  255
0    255  255

Python implementation:

def threshold_image(image: np.ndarray, threshold: int) -> np.ndarray:
    result = np.where(image >= threshold, 255, 0)
    return result.astype(np.uint8)

2. Image Memory

This problem calculates the memory required by an uncompressed image.

Formula

bytes = width * height * bpp / 8

Example:

Width = 1920
Height = 1080
Bits per pixel = 24

1920 * 1080 * 24 / 8
= 6,220,800 bytes

Python implementation:

def display_memory_bytes(width: int, height: int, bpp: int) -> int:
    return width * height * bpp // 8

3. Average of 9 Pixels

This problem finds the average of a 3 x 3 image block.

Example:

10  20  10
30  50  30
10  20  10

The sum is:

190

Therefore:

190 / 9 = 21

Rounded result:

21

Python implementation:

def mean_filter_3x3(neighborhood: np.ndarray) -> int:
    return int(round(np.sum(neighborhood) / 9))

4. Contrast Stretching

Contrast stretching changes values from one range to another.

Formula

new = (value - old_min) / (old_max - old_min)
      * (new_max - new_min) + new_min

Example:

Input values:
[50, 100, 150]

Old range:
[50, 150]

New range:
[0, 255]

Result:

[0, 127.5, 255]

Python implementation:

def contrast_stretch(
    image: np.ndarray,
    in_min: float,
    in_max: float,
    out_min: float = 0.0,
    out_max: float = 255.0,
) -> np.ndarray:

    image_float = image.astype(np.float32)

    result = (
        (image_float - in_min)
        / (in_max - in_min)
        * (out_max - out_min)
        + out_min
    )

    return np.clip(result, out_min, out_max).astype(np.float32)

5. Sobel Edge Detection

The Sobel operator is used to find the strength of an edge in a 3 x 3 grayscale block.

Gx Kernel

-1   0   1
-2   0   2
-1   0   1

Gy Kernel

-1  -2  -1
 0   0   0
 1   2   1

The edge strength is calculated using:

strength = sqrt(gx * gx + gy * gy)

Example:

20  20  200
20  20  200
20  20  200

Result:

Gx = 720
Gy = 0
Edge Strength = 720

Python implementation:

def sobel_response(block: np.ndarray) -> tuple[float, float, float]:
    gx_kernel = np.array([
        [-1, 0, 1],
        [-2, 0, 2],
        [-1, 0, 1]
    ], dtype=np.float32)

    gy_kernel = np.array([
        [-1, -2, -1],
        [0, 0, 0],
        [1, 2, 1]
    ], dtype=np.float32)

    gx = float(np.sum(block * gx_kernel))
    gy = float(np.sum(block * gy_kernel))
    strength = float(np.sqrt(gx * gx + gy * gy))

    return gx, gy, strength

Testing

The notebook includes test cases using Python assert statements and NumPy testing functions.

The implementations are tested for:

RGB to Grayscale

Brightness and Contrast Adjustment

Thresholding

Image Memory Calculation

3×3 Mean Filter

Contrast Stretching

Sobel Edge Detection

The sample tests check that the calculated results match the expected results.

Learning Outcomes

After completing this lab, I practiced:

Writing Python functions for image processing

Working with NumPy arrays

Applying mathematical formulas in Python

Performing pixel-level operations

Converting RGB values to grayscale

Applying thresholding

Calculating image memory

Calculating a 3×3 mean filter

Performing contrast stretching

Understanding Sobel edge detection

Testing implementations using expected results
