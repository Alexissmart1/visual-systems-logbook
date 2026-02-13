# Lab 3 - Intensity Transformation and Spatial Filtering

## Task 1 - Contrast enhancement with function imadjust

### Importing an image

Check what is on image file *_breastXray.tif_* stored in the assets folder and read the image data into the matrix *_f_*, and display it:

```
clear all
imfinfo('assets/breastXray.tif')
f = imread('assets/breastXray.tif');
imshow(f)
```
Output:
```
ans = struct with fields:
                     Filename: 'C:\Users\alexc\Downloads\Lab3-Intensity-transformation-main\Lab3-Intensity-transformation-main\matlab\breastXray.tif'
                  FileModDate: '28-Jan-2026 14:28:39'
                     FileSize: 275657
                       Format: 'tif'
                FormatVersion: []
                        Width: 482
                       Height: 571
                     BitDepth: 8
                    ColorType: 'grayscale'
              FormatSignature: [73 73 42 0]
                    ByteOrder: 'little-endian'
               NewSubFileType: 0
                BitsPerSample: 8
                  Compression: 'Uncompressed'
    PhotometricInterpretation: 'BlackIsZero'
                 StripOffsets: [25 10147 20269 30391 40513 50635 60757 70879 81001 91123 101245 111367 121489 131611 141733 151855 161977 172099 182221 192343 202465 212587 222709 232831 242953 253075 263197 273319]
              SamplesPerPixel: 1
                 RowsPerStrip: 21
              StripByteCounts: [10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 10122 1928]
                  XResolution: 118.1102
                  YResolution: 118.1102
               ResolutionUnit: 'Centimeter'
                     Colormap: []
          PlanarConfiguration: 'Chunky'
                    TileWidth: []
                   TileLength: []
                  TileOffsets: []
               TileByteCounts: []
                  Orientation: 1
            AutoOrientedWidth: 482
           AutoOrientedHeight: 571
                    FillOrder: 1
             GrayResponseUnit: 0.0100
               MaxSampleValue: 255
               MinSampleValue: 0
                 Thresholding: 1
                       Offset: 275471
             ImageDescription: '
```
<img width="845" height="765" alt="image" src="https://github.com/user-attachments/assets/5b7b17eb-e862-4c6d-9624-87af2d41562b" />


Check the dimension of _f_ on the right window pane of Matlab. Examine the image data stored:

```
f(3,10)             % print the intensity of pixel(3,10)
imshow(f(1:241,:))  % display only top half of the image
```
output:
```
ans = uint8
28
```

<img width="740" height="380" alt="image" src="https://github.com/user-attachments/assets/4f760543-6ecc-4bdd-881d-5ce98829cd88" />


To find the maximum and minimum intensity values of the image, do this:
```
[fmin, fmax] = bounds(f(:))
```
output:
```
fmin = uint8
21
fmax = uint8
255
```
**Test yourself**: Display the right half of the image. Capture it for your logbook.

```
imshow(f(:,241:482))
```
<img width="500" height="710" alt="image" src="https://github.com/user-attachments/assets/27e6d6ca-d117-4641-9f4f-ec2753ca349b" />

### Negative image

To compute the negative image and display both the original and the negative image side-by-side, do this:

```
g1 = imadjust(f, [0 1], [1 0])
figure              % open a new figure window
montage({f, g1})
```
<img width="935" height="543" alt="image" src="https://github.com/user-attachments/assets/cf9504e0-88fb-47d6-bae3-3014fccfc93a" />

### Gamma correction

Try this:
```
g2 = imadjust(f, [0.5 0.75], [0 1]);
g3 = imadjust(f, [ ], [ ], 2);
figure
montage({g2,g3})
```

<img width="935" height="543" alt="image" src="https://github.com/user-attachments/assets/2ff05d0b-436d-4ed8-9e63-17d1eb3fdfb4" />

_g2_ has the gray scale range between 0.5 and 0.75 mapped to the full range.

_g3_ uses gamma correct with gamma = 2.0 as shown in the diagram below. [ ] is the same as [0 1] by default.

<img width="452" height="402" alt="image" src="https://github.com/user-attachments/assets/2324d559-4565-4bd4-86d8-ae297eb47ddf" />


This produces a result similar to that of g2 by compressing the low end and expanding the high end of the gray scale.  It however, unlike g2,  retains more of the details because the intensity now covers the entire gray scale range.  _function montage_ stitches together images in the list specified within { }.

## Task 2: Contrast-stretching transformation

Instead of using the *_imadjust function_*, we will apply the constrast stretching transformation function in Lecture 4 slide 4 to improve the contrast of another X-ray image.  The transformation function is as shown here:

<img width="429" height="387" alt="image" src="https://github.com/user-attachments/assets/906dcb04-8d54-461e-a39c-0b7c62aaed62" />


The equation of this function is:

$$s = T(r) = {1 \over 1 + (k/r)^E}$$

where *_k_* is often set to the average intensity level and E determines steepness of the function. Note that the 

```
clear all       % clear all variables
close all       % close all figure windows
f = imread('assets/bonescan-front.tif');
r = double(f);  % uint8 to double conversion
k = mean2(r);   % find mean intensity of image
E = 0.9;
s = 1 ./ (1.0 + (k ./ (r + eps)) .^ E);
g = uint8(255*s);
imshowpair(f, g, "montage")
```
_eps_ is a special Matlab constant which has the smallest value possible for a double precision floating point number on your computer.  Adding this to _r_ is necessary to avoid division by 0.

Matlab function *_mean2_* computes the average value of a 2-D matrix.  Since the equation operates on floating numbers, we need to convert the image intensity, which is of type _uint8_ to type _double_ and store it in _r_.  We then compute the contrast stretched image by applying the stretch function element-by-element, and store the result in _s_.  

The intensity values of s are normalized to the range of [0.0 1.0] and is in type _double_.  Finally we scale this back to the range [0 255] and covert back to _uint8_.

Discuss the results with your classmates and record your observations in your logbook.

<img width="905" height="1255" alt="image" src="https://github.com/user-attachments/assets/5461a2c5-7d4a-41f0-976f-d1c90976698f" />

Contrast in the image has improved making it easier to see tissue and other features
## Task 3: Contrast Enhancement using Histogram

### PLotting the histogram of an image

Matlab has a built-in function _imhist_ to compute the histogram of an image and plot it.  Try this:

```
clear all       % clear all variable in workspace
close all       % close all figure windows
f=imread('assets/pollen.tif');
imshow(f)
figure          % open a new figure window
imhist(f);      % calculate and plot the histogram
```
output:
<img width="758" height="639" alt="image" src="https://github.com/user-attachments/assets/48acd35f-1fe4-4b5a-bfee-b76f8b141f55" />
<img width="905" height="679" alt="image" src="https://github.com/user-attachments/assets/a26ef91e-afaa-46f8-9f1b-1d826091fe60" />

It is clear that the intesity level of this image is very much squashed up between 70 to 140 in the range [0 255].  One attempt is to stretch the intensity between 0.3 and 0.55 of full scale (i.e. 255) with the built-in function _imadjust_ from the previous tasks. Try this:

```
close all
g=imadjust(f,[0.3 0.55]);
montage({f, g})     % display list of images side-by-side
figure
imhist(g);
```
output:

<img width="905" height="459" alt="image" src="https://github.com/user-attachments/assets/98e52094-d95a-4f76-a38e-a4f84bee89e9" />
<img width="905" height="679" alt="image" src="https://github.com/user-attachments/assets/4a30bd9c-5904-45a4-9333-773e6d9abb8f" />


The histogram of the adjusted image is more spread out.  It is definitely an improvement but it is still not a good image.

### Histogram, PDF and CDF

Probability distribution function (PDF) is simply a normalised histogram.  Cumulative distribution function (CDF) is the integration of cumulative sum of the PDF.  Both PDF and CDF can be obtained as below.  Note that _numel_ returns the total number of elements in the matrix.  The following code computs the PDF and CDF for the adjusted image _g_, and plot them side-by-side in a single figure.  The function _subplot(m, n, p)_ specifies which subplot is to be used.

```
g_pdf = imhist(g) ./ numel(g);  % compute PDF
g_cdf = cumsum(g_pdf);          % compute CDF
close all                       % close all figure windows
imshow(g);
subplot(1,2,1)                  % plot 1 in a 1x2 subplot
plot(g_pdf)
subplot(1,2,2)                  % plot 2 in a 1x2 subplot
plot(g_cdf)
```
output:
<img width="758" height="639" alt="image" src="https://github.com/user-attachments/assets/231f99f7-3317-4454-9a2f-1800ba9a0767" />
<img width="758" height="639" alt="image" src="https://github.com/user-attachments/assets/ce0e63bd-ac03-492a-9b28-900ea435abeb" />

### Histogram Equalization

To perform histogram equalization, the CDF is used as the intensity transformation function.  The CDF plot made earlier is bare and axes are not labelled nor scaled.  The following code replot the CDF and make it looks pretty.  It is also an opportunity to demonstrate some of Matlab's plotting capabilities.

```
x = linspace(0, 1, 256);    % x has 256 values equally spaced
                            %  .... between 0 and 1
figure
plot(x, g_cdf)
axis([0 1 0 1])             % graph x and y range is 0 to 1
set(gca, 'xtick', 0:0.2:1)  % x tick marks are in steps of 0.2
set(gca, 'ytick', 0:0.2:1)
xlabel('Input intensity values', 'fontsize', 9)
ylabel('Output intensity values', 'fontsize', 9)
title('Transformation function', 'fontsize', 12)
```
output:
<img width="935" height="702" alt="image" src="https://github.com/user-attachments/assets/4df8dd47-d723-407c-aad0-27cf26458a57" />


The Matlab function _histeq_ computes the CDF of an image, and use this as the intensity transformation function to flatten the histogram.  The following code will perform this function and provide plots of all three images and their histogram.

```
h = histeq(g,256);              % histogram equalize g
close all
montage({f, g, h})
figure;
subplot(1,3,1); imhist(f);
subplot(1,3,2); imhist(g);
subplot(1,3,3); imhist(h);
```
output:
<img width="905" height="833" alt="image" src="https://github.com/user-attachments/assets/dbb97c15-d087-484c-a783-3fa0a8be1522" />
<img width="905" height="679" alt="image" src="https://github.com/user-attachments/assets/9543cc4d-4a5a-4008-9b81-dcd01a45de57" />

## Task 4 - Noise reduction with lowpass filter

In Lecture 5, we consider a variety of special filter kernels, including: Averaging (box), Gaussian, Laplacian and Sobel. In this task, you will explore the effect of each of this on an image.  In this task, you will explore two type of smoothing filter - the moveing average (box) filter and the Gaussian filter.

Before filtering operation can be performed, we need to define our filter kernel.  Matlab provides a function called _fspecial_, which returns different types of filter kernels.  The table below shows the types of kernels that can generated.

<p align="center"> <img src="assets/fspecial.jpg" /> </p><BR>

Import an X-ray image of a printed circuit board.

```
clear all
close all
f = imread('assets/noisyPCB.jpg');
imshow(f)
```
<img width="935" height="817" alt="image" src="https://github.com/user-attachments/assets/7d917658-c49c-407c-900d-62df6eea70ab" />

The image is littered with noise which is clearly visible.  We shall attempt to reduce the noise by using Box and the Gaussian filters.

Use the function _fspecial_ to produce a 9x9 averaging filter kernel _ and a 7 x 7 Gaussian kernel with sigma = 0.5  as shown below:

```
w_box = fspecial('average', [9 9])
w_gauss = fspecial('Gaussian', [7 7], 1.0)
```
Note that the coefficients are scaled in such a way that they sum to 1.

Now, apply the filter to the image with:
```
g_box = imfilter(f, w_box, 0);
g_gauss = imfilter(f, w_gauss, 0);
figure
montage({f, g_box, g_gauss})
```
<img width="852" height="1279" alt="image" src="https://github.com/user-attachments/assets/ccc1ad8c-910c-4d56-a086-50367c2b09eb" />

Comment on the results.  
with the 9x9 filter the noise is reduced but the image appears blured and it lacks sharpness, the 7x7 image appears clearer but some noise is still visable. A larger Kernel size reduces noise but increases blur

## Task 5 - Median Filtering

In both cases with Average and Gaussian filters, noise reduction is companied by reducing in the sharpness of the image.  Median filter provides a better solution if sharpness is to be preserved.  Matlab provides the function _medfilt2(I, [m n], padopt)_ for such an operation.  [m n] defines the kernel dimension. _padopt_ specifies the padding option at the boundaries.  Default is 'zero', which means it is zero-padded.

Try this:
```
g_median = medfilt2(f, [7 7], 'zero');
figure; montage({f, g_median})
```
<img width="935" height="459" alt="image" src="https://github.com/user-attachments/assets/8f68c50f-8561-47bd-9a1c-2973d29888fe" />

Comment on the results.
More noise is reduced from the gaussian filter with edges appearing to be sharper

## Task 6 - Sharpening the image with Laplacian, Sobel and Unsharp filters

Now that you are familiar with the Matlab functions _fspecial_ and _imfilter_, explore with various filter kernels to sharpen the moon image stored in the file _moon.tif_. The goal is to make the moon photo sharper so that the craters can be observed better.

```
clear; close all; clc

f = imread('moon.tif');
f = im2double(f);

% Contrast Enhancement
f_ce = imadjust(f, [], [], 0.9);

% Laplacian sharpening
w_lap = fspecial('laplacian', 0.2);         
lap = imfilter(f_ce, w_lap, 0);             
alpha = 0.5;                                  
g_lap = f_ce - alpha * lap;

% Sobel sharpening
w_sobel = fspecial('sobel');
gx = imfilter(f_ce, w_sobel, 0);
gy = imfilter(f_ce, w_sobel', 0);
gradmag = sqrt(gx.^2 + gy.^2);
k = 0.5;                                     
g_sobel = f_ce + k * gradmag;

% Unsharp mask
w_gauss = fspecial('gaussian', [9 9], 2.0);  
blur = imfilter(f_ce, w_gauss, 0);
alpha = 1.5;                                
mask = f_ce - blur;
g_unsharp = f_ce + alpha * mask;

figure
montage({f, f_ce, g_lap, g_sobel, g_unsharp})
title('Original | Contrast-enhanced | Laplacian | Sobel | Unsharp')
```

<img width="890" height="712" alt="moon" src="https://github.com/user-attachments/assets/a9fe1676-cc5b-464b-9cf2-49d66ad4810f" />



## Task 7 - Test yourself Challenges

* Improve the contrast of a lake and tree image store in file _lake&tree.png_ use any technique you have learned in this lab. Compare your results with others in the class.

```
clear; close all; clc

f = imread('lake&tree.png');

% Gamma correction
g = imadjust(f, [], [], 0.7);

% Histogram equalization
h = histeq(f, 256);

% Contrast-stretch
r = double(f);  % uint8 to double conversion
k = mean2(r);   % find mean intensity of image
E = 5;
s = 1 ./ (1.0 + (k ./ (r + eps)) .^ E);
c = uint8(255*s);

montage({f, g, h, c}, "Size", [2 2]);
title('Original | gamma correction | Histogram equalization | contrast-stretch')
```

<img width="1084" height="1375" alt="image" src="https://github.com/user-attachments/assets/28ee2a96-81ac-4776-8fc7-efff3d5e49dc" />


* Use the Sobel filter in combination with any other techniques, find the edge of the circles in the image file _circles.tif_.  You are encouraged to discuss and work with your classmates and compare results.

```
clear; close all; clc

f = imread('circles.tif');
f = im2double(f);

% Sobel filter
w_sobel = fspecial('sobel');
gx = imfilter(f, w_sobel, 0);   
gy = imfilter(f, w_sobel', 0);    
gradmag = sqrt(gx.^2 + gy.^2);

level = graythresh(gradmag);
BW = imbinarize(gradmag, level);

figure
montage({f, gradmag, BW}, "Size", [1 3]);
```

<img width="1232" height="348" alt="image" src="https://github.com/user-attachments/assets/8ebb4818-f9cb-4500-8132-461f7b045652" />

* _office.jpg_ is a colour photograph taken of an office with badd exposure.  Use whatever means at your disposal to improve the lighting and colour of this photo.

```
clear; close all; clc

f = imread('office.jpg');
f = im2double(f);

% Gamma Correction
g_gamma = imadjust(f, [], [], 0.5); 

% Histogram Equalization
h = histeq(g_gamma, 256);

% Contrast Adjustment
g_contrast = imadjust(h, stretchlim(h), []);

figure;
montage({f, g_contrast});
```

<img width="1232" height="439" alt="image" src="https://github.com/user-attachments/assets/fd27d9c5-6aed-4e12-854b-bbeb3551cf62" />

