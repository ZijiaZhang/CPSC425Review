# CPSC425Review

# Chapter 1: Imaging
## 1. Bare image sensor
- All scene points will contribute to all sensor points
## Pinhole camera
- Most rays are blocked (Only one ray will be pass through the hole and contribute to the sensor at each scene point .)
-  The projection image is upside down
- The image of a pinhole camera is in focus no matter what f’ is.
### Disadvantage
- If the pinhole is too big then the image is blurred
- If the pinhole is too small then the diffraction also blaring the image
- The pinhole can be dark because only a few rays pass through. And so it is dark.
## Perspective Projections
- the apparent size of objects depends on their distance to the camera
- the size of the imaged object is inversely proportional to the distance
- Two parallel lines converge at a vanishing point which lies on a horizon line
- The 3D point $P = \left[\begin{array}{r} x \\ y \\ z \end{array}\right] $ projects to $P’ = \left[\begin{array}{r} x’ \\ y’ \end{array} \right]$
- $x’ = f’ \displaystyle \frac{x}{z}$
- $y’ = f’ \displaystyle \frac{y}{z}$
## Weak perspective (including orthographic)
- has simpler mathematics
- accurate when object is small and/or distant
- useful for recognition
## Snell’s Law
- $n_1sin\alpha_1 = n_2sim\alpha_2$
## Thin lens equations
- $\frac{1}{z’} + \frac{1}{z} = \frac{1}{f}$
### Focal Lengths
- The incoming rays parallel to the optical axis converge to a single point a distance f behind the lens
### Spherical Aberration
- Because lenses are not perfect, often times refraction will change as you get off center. 
### Vignetting
- In a multiple lens system, some part of the beam never reaches the second lens.
### Chromatic Aberration
- Index of refraction depends on wavelength, so not all colours can be in equal in focus
# Chapter 2: Filtering
## Linear filter
- $k = \left \lfloor \frac{m}{2} \right \rfloor$
- $I’(X, Y) = \sum_{j=-k}^{k} \sum_{I=-k}^{k} F(i,j) I(X+I, Y +j)$
- K is the half size of the filter
- Usually the sum of the filters sum to 1 to keeps the mean of the image at the same value.
- We can also have the image value sum to 0.
### Superposition and scaling
An operation is linear if it satisfy both superposition and scaling
### Shift invariance
Output does not depend on absolute position 
### Box filter
- Smoothing
- doesn’t model lens defocus well
### Pillbox filter
- A better model for defocus
### Gaussian
- A better general blurring model
- Quantized and truncated:
	- Does not sum up to 1, make the image brighter or darker
	- In general, you want the Gaussian filter to capture -3$\sigma$ to $3\sigma$, for $\sigma = 1$ => 7x7 filter 
## Boundary Effects
### Ignore those locations
Make the computation undefined for the top and bottom k rows and the leftmost and rightmost k column
- Image size changes
- No information is created
###  Pad the image with zeros
Return zero whenever a value of I is required at some position outside the defined limits of X and Y
- maintains size
- you are effectively creating a black border, this can cause artifacts
###  Assume periodicity
The top row wraps around to the bottom row; the leftmost column wraps around to the rightmost column
- Best of both words, if the image is truly periodic  
### Reflect border
Copy rows/columns locally by reflecting over the edge
## Correlation vs. Convolution
- Rotate filter clockwise by 180 degrees!
- Any linear, shift invariant operation can be expressed as convolution
## Separability
- If a filter is separable, then it can be implemented in two 1D convolutions:
	1. Convolve each row with a 1D filter
	2. convolve each column with a 1D filter
This can reduce the amount of computation:
	- With naive implementation, there are $m \times m$ multiplications at each pixel <X,Y>, and there are $n \times n$ pixels in total. Then we have $m^2 \times n^2$ multiplications
	- With separate filters, at each pixel, <X,Y> there are 2m multiplications, and thus we have total of $2m \times n^2$ multiplications
- 2D Gaussian is the only (non trivial) 2D function that is both separable and rotationally invariant
## Convolution Theorem
IDK what it is
## Non-linear Filters
### Median Filter
Take the median value of the pixels under the filter
- Effective at reducing certain kinds of noise, such as impulse noise
### Bilateral Filter
- An edge-preserving non-linear filter 
	- The filter weights depend on spatial distance from the center pixel
	- The filter weights also depend on range distance from the center pixel
- Good for Denoising
# Chapter 3: Sampling
## Sampling Theory
- When can we reconstruct $i(x, y)$ exactly from  I(X, Y) ?
- Reconstruction involves some kind of interpolation
- For bandlimited signals, if you sample regularly at or above twice the maximum frequency (called the Nyquist rate), then you can reconstruct the original signal exactly 
- If we sample more than the required rate, (over sample) nothing bad happens.
- If we sample lower than the required rate, we might have:
	-  Things are missing (i.e., things that should be there aren’t). 
	- There are artifacts (i.e., things that shouldn’t be there are) 
### Reduce the Aliasing Artifacts 
- Over sampling
	- Sample more than you needed and average
- Smoothing before sampling
## Color Filters
### Two design choices
- What spectral sensitivity functions  $\lambda$ to use for each color filter
- How to spatially arrange (“mosaic”) different color filters?
### CFA Demosicing
Produce full RGB image from mosaiced sensor output
- Bilinear interpolation: Interpolate beween 4 neighbours
- Bicubic interpolation: Needs more neighbours, but may overblur the image
- Edge-aware interpolation

# Chapter 5: Template Matching
## Correlation
- The correlation is a dot product. And measures the similarity between the filter and each local image region.
- Normalized correlation varies between −1 and 1
- Normalized correlation attains the value 1 when the filter and image region are identical 
## Linear Filtering (Template Matching)
- Linear filtering the entire image computes the entire set of dot products, one for each possible alignment of the filter and the image 
- Detection can be done by comparing correlation map score to a threshold
### Issue
- Different scales
- Different orientation
- Different lighting condition
- Left vs. Right hand
- Partial Occlusions (Blocked)
- Different Perspective
- Motion / blur
### Advantage
- Works well in presence of noise 
- Relatively easy to compute 

### Scaled Representations
- Build a Gaussian pyramid of the image and ignore the template 2D Scale
	- template size constant, image scale varies
	- (efficient search) look first at coarse scales, refine at finer scales
	- find edges with different amounts of blur 
	- find textures with different spatial frequencies 
	- First apply the Gaussian filter then throw away half of the rows & columns
	- For every image reduction of 0.5, smooth by $\sigma = 1$
## Local Feature detection
## Derivative
	- X: [-1, 1]
	- Y: [-1 \\ 1]
	- Why, in general, should the weights of a filter used for differentiation sum to 0? 
	If your filter did not sum up to 0, then you would be weighing the value of one pixel more than the value of the other, and that would be wrong.
### Edge Detection
Smoothing + Derivative
- Sobel Edge Detector
	- Use central differencing to compute gradient image
	- Threshold to obtain edges 
- Marr / Hildreth Laplacian of Gaussian
	- A “zero crossings of a second derivative operator” approach 
	- Steps:
		1. Gaussian for smoothing 
		2. Laplacian for differentiation
		3. Locate the Zero Crossing
- Canny Edge Detector
	- Apply directional derivatives
	- Compute gradient magnitude and gradient direction
	- Non-maximum suppression 
		- thin multi-pixel wide “ridges” down to single pixel width 
	- Linking and thresholding
		- Low, high edge-strength thresholds
		- Accept all edges over low threshold that are connected to edge over high threshold
## Laplacian Pyramid 
- Create a Gaussian pyramid

- Take the difference between one Gaussian pyramid level and the next (before subsampling)  - the residual
- We can reconstruct the original image using the pyramid
## Boundary Detection
IDK What is this

 
	
