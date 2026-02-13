# Lab 4 - Morphological Image Processing

## Task 1: Dilation and Erosion

### Dilation Operation
```
A = imread('assets/text-broken.tif');
B1 = [0 1 0;
     1 1 1;
     0 1 0];    % create structuring element
A1 = imdilate(A, B1);
montage({A,A1})
```

> Change the structuring element (SE) to all 1's.  Instead of enumerating it, you can do that with the function _ones_:
```
B2 = ones(3,3);     % generate a 3x3 matrix of 1's
```
Note: text appears thicker
> Try making the SE larger.

Note: Text appears very thick and gaps between letters are closing, Hard to read
> Try to make the SE diagonal cross:
```
Bx = [1 0 1;
      0 1 0;
      1 0 1];
```
Note: Text isnt too thick with good spacing and holes are clear within letters

> What happens if you dilate the original image with B1 twice (or more times)?

Note: Text becomes thicker with each repeated dilation

```
A = imread('text-broken.tif');
B1 = [0 1 0;
     1 1 1;
     0 1 0];    % create structuring element
B2 = ones(3,3);     % generate a 3x3 matrix of 1's
B3 = ones(5,5);
B4 = [1 0 1;
      0 1 0;
      1 0 1];
A1 = imdilate(A, B1);
A2 = imdilate(A, B2);
A3 = imdilate(A, B3);
A4 = imdilate(A, B4);
A1_twice = imdilate(A1, B1);  
montage({A,A1,A2,A3,A4,A1_twice})
```
<img width="935" height="537" alt="image" src="https://github.com/user-attachments/assets/6b8954b6-0857-4c73-9d7f-6df4452c8f8b" />


### Erosion Operation

Explore erosion with the following:

```
clear all
close all
A = imread('assets/wirebond-mask.tif');
SE2 = strel('disk',2);
SE10 = strel('disk',10);
SE20 = strel('disk',20);
E2 = imerode(A,SE2);
E10 = imerode(A,SE10);
E20 = imerode(A,SE20);
montage({A, E2, E10, E20}, "size", [2 2])
```
Comment on the results.

Note: Pixels are removed from the foreground boundaries, This shrinking effect increases with a larger structuring element, thin diagonal lines are removed first and pads get thinner

## Task 2 - Morphological Filtering with Open and Close

### Opening = Erosion + Dilation
In this task, you will explore the effect of using Open and Close on a binary noisy fingerprint image.

1. Read the image file 'finger-noisy.tif' into _f_.
2. Generate a 3x3 structuring element SE.
3. Erode _f_ to produce _fe_.
4. Dilate _fe_ to produce _fed_.
5. Open _f_ to produce _fo_.
6. Show _f_, _fe_, _fed_ and _fo_ as a 4 image montage.

```
clear all
close all
f = imread('fingerprint-noisy.tif');
SE = strel('square',3);
fe = imerode(f, SE);
fed = imdilate(fe, SE);
fo = imopen(f, SE);
montage({f, fe, fed, fo});
```
<img width="905" height="658" alt="image" src="https://github.com/user-attachments/assets/2c9562f6-6cab-4c99-935a-39e8f9c16a56" />

> Comment on the the results.

Note: f: fingerprint has noise surrounding fingerprint with breaks and holes within the fingerprint
fe: ridges are thinner however noise has been removed, more gaps in the ridges now
fed: ridge thickness returns without noise that was removed by erosion. Gaps in ridges are filled back in
fo: is very similar to fed

> Explore what happens with other size and shape of structuring element.

Larger SE: removes more fingerprint details and very large SE's remove fingerprint entirely
Disk/square: disk has rounder smoothing than square

> Improve the image _fo_ with a close operation.

```
fcl = imclose(fo, SE);
```
Breaks in the fingerprint ridges are repaired

> Finally, compare morphological filtering using Open + Close to spatial filter with a **Gaussian filter**. Comment on your comparison.

```
f = imread('fingerprint-noisy.tif');
SE = strel('square',3);
fo = imopen(f, SE);
fcl = imclose(fo, SE);
w_gauss = fspecial('Gaussian', [7 7], 1.0)
g_gauss = imfilter(f, w_gauss, 0);
montage({f,fcl,g_gauss});
```
<img width="905" height="658" alt="image" src="https://github.com/user-attachments/assets/e7a990f5-56da-45fe-8eae-8fc1f4750493" />

Notes: Gaussian removes some noise however not all of it, the ridges on the fingerprint are smoother however overall there is a loss of detail. 
Morphological method is superior with a sharper image with less noise and better image retention. 

## Task 3 - Boundary detection 

The grayscale image 'blobs.tif' consists of blobs or bubbles of different sizes in a sea of noise. Further, the bubbles are dark, while the background is white.  The goal of this task is to find the boundaries of the blobs using the boundary operator 

First we turn this "inverted" grayscale image into a binary image with white objects (blobs) and black background. Do the following:

```
clear all
close all
I = imread('assets/blobs.tif');
I = imcomplement(I);
level = graythresh(I);
BW = imbinarize(I, level);
```

Now, use the boundary operation to compute the boundaries of the blobs. This is achieved by eroding BW  with SE, where SE is a 3x3 elements of 1's. The eroded image is subtract from BW. 

Diplay as montage {I, BW, erosed BW and boundary detected image}.  Comment on the result.

```
clear all
close all
I = imread('blobs.tif');
I = imcomplement(I);
level = graythresh(I);
BW = imbinarize(I, level);
SE = strel('square',3)
BWe = imerode(BW, SE);
B = BW - BWe;
montage({I, BW, BWe, B});
```

<img width="905" height="833" alt="image" src="https://github.com/user-attachments/assets/68f97321-fa1e-4e6a-8542-97f8fc4bf455" />

Notes: I: image is now inverted, blobs are bright and background is dark
BW: Otsu method threshold improves image segmentation, but there is still noise
BWe: Foreground boundaries are eroded, blobs have shrunk and noise is reduced
Boundary image: outlines of the blobs are visable as well as the origional image noise

> How can you improve on this result?

The result could be improved by using a Morphological Open and close process to remove noise and then use an erode process to generate the blob boundaries 

```
clear all
close all
I = imread('blobs.tif');
I = imcomplement(I);
level = graythresh(I);
BW = imbinarize(I, level);
SE = strel('square',3)
BW_clean = imopen(BW, SE);
BW_clean = imclose(BW_clean, SE);
BW_er = imerode(BW_clean, SE);
B = BW_clean - BW_er;
montage({I, BW, BW_clean, B});
```

<img width="905" height="833" alt="image" src="https://github.com/user-attachments/assets/a042eda4-da0e-4a43-bc5e-24af6e104bc5" />

## Task 4 - Function bwmorph - thinning and thickening

Matlab's Image Processing Toolbox includes a general morphological function *_bwmorph_* which implements a variety of morphological operations based on combinations of dilations and erosions.  The calling syntax is:

```
g = bwmorph(f, operations, n)
```
where *_f_* is the input binary image, *_operation_* is a string specifying the desired operation, and *_n_* is a positive integer specifying the number of times the operation should be repeated. (n = 1 if omitted.)

The morphological operations supported by _bwmorph_ are:

<p align="center"> <img src="assets/bwmorph.jpg" /> </p>

To test function *_bwmorph_* on thinning operation, do the following:

1. Read the image file 'fingerprint.tif' into *_f_*.
2. Turn this into a good binary image using method from the previous task. 
3. Perform thinning operation 1, 2, 3, 4 and 5 times, storing results in g1, g2 ... etc.
4. Montage the unthinned and thinned images to compare.

What will happen if you keep thinning the image?  Try thinning with *_n = inf_*.  (_inf_ is reserved word in Matlab which means infinity.  However, for _bwmorph_, it means repeat the function until the image stop changing.)

Modify your matlab code so that the fingerprint is displayed black lines on white background instead of white on black.  What conclusion can you draw about the relationship between thinning and thickening?

## Task 5 - Connected Components and labels

In processing and interpreting an image, it is often required to find objects in an image.  After binarization, these objects will form regions of 1's in background of 0's. These are called connected components within the image.  

Below is a text image containing many characters.  The goal is to find the **largest connected component** in this image, and then **erase it**.

<p align="center"> <img src="assets/text.png" /> </p>

This sounds like a very complex task. Fortunately Matlab provides in their Toolbox the function _bwconncomp_ which performs the morphological operation described in Lecture 6 slides 22 - 24. Try the following Matlab script:

```
t = imread('assets/text.png');
imshow(t)
CC = bwconncomp(t)
```

*_CC_* is a data structure returned by *_bwconncomp_* as described below.

<p align="center"> <img src="assets/cc.jpg" /> </p>

To determine which is the largest component in the image and then erase it (i.e. set all pixels within that componenet to 0), do this:

```
numPixels = cellfun(@numel, CC.PixelIdxList);
[biggest, idx] = max(numPixels);
t(CC.PixelIdxList{idx}) = 0;
figure
imshow(t)
```
These few lines of code introduce you to some cool features in Matlab.

1. **_cellfun_** applies a function to each element in an array. In this case, the function _numel_ is applied to each member of the list **_CC.PixelIdxList_**.  The kth member of this list is itself a list of _(x,y)_ indices to the pixels within this component.

2. The function **_numel_** returns the number of elements in an array or list.  In this case, it returns the number of pixels in each of the connected components.

3. The first statement returns **_numPixels_**, which is an array containing the number of pixels in each of the detected connected components in the image. This corresponds to the table in Lecture 6 slide 24.

4. The **_max_** function returns the maximum value in numPixels and its index in the array.

5. Once this index is found, we have identified the largest connect component.  Using this index information, we can retrieve the list of pixel coordinates for this component in **_CC.PixelIdxList_**.

## Task 6 - Morphological Reconstruction

In morphological opening, erosion typlically removes small objects, and subsequent dilation tends to restore the shape of the objects that remains.  However, the accuracy of this restoration relies on the similarly between the shapes to be restored and the structuring element.

**_Morphological reconstruction_** (MR) is a better method that restores the original shapes of the objects that remain after erosion.  

The following exercise demonstrates the method described in Lecture 7 slide 10.  A binary image of printed text is processed so that the letters that are long and thin are kept, while all others are removed.  This is achieved through morphological reconstruction.

MR requires three things: an **input image** **_f_** to be processed called the **_mask**, a **marker image** **_g_**, and a structuring element **_se_**.  The steps are:

1. Find the marker image **_g_** by eroding the mask with an **_se_** that mark the places where the desirable features are located. In our case, the desired characters are all with long vertical elements that are 17 pixels tall.  Therefore the **_se_** used for erosion is a 17x1 of 1's.

2. Apply the reconstruction operation using Matlab's **_imreconstruct_** functino between the **marker** **_g_** and the **mask** **_f_**. 

The step are:

```
clear all
close all
f = imread('assets/text_bw.tif');
se = ones(17,1);
g = imerode(f, se);
fo = imopen(f, se);     % perform open to compare
fr = imreconstruct(g, f);
montage({f, g, fo, fr}, "size", [2 2])
```

Comment on what you observe from these four images.

Also try the function **_imfill_**, which will fill the holes in an image (Lecture 6 slides 19-21).

```
ff = imfill(f);
figure
montage({f, ff})
```

## Task 7 - Morphological Operations on Grayscale images

So far, we have only been using binary images because they vividly show the effect of morphological operations, turning black pixels to white pixels insted of just change the shades of gray.

In this task, we will explore the effect of erosion and dilation on grayscale images. 

Try the follow:

```
clear all; close all;
f = imread('assets/headCT.tif');
se = strel('square',3);
gd = imdilate(f, se);
ge = imerode(f, se);
gg = gd - ge;
montage({f, gd, ge, gg}, 'size', [2 2])
```
Comments on the results.

## Challenges

You may like to attemp one or more of the following challenges. Unlike tasks in this Lab where you were guided with clear instructions, you are required to find your solutions yourself based on what you have learned so far.  

1. The grayscale image file _'assets/fillings.tif'_ is a dental X-ray corrupted by noise.  Find how many fills this patient has and their sizes in number of pixels.

2. The file _'assets/palm.tif'_ is a palm print image in grayscale. Produce an output image that contains the main lines without all the underlining non-characteristic lines.

3. The file _'assets/normal-blood.png'_ is a microscope image of red blood cells. Using various techniques you have learned, write a Matlab .m script to count the number of red blood cells.

---
## DRAW Week Assessment
---

The first half of this module is assessed on your effort in completing Lab 1 to Lab 4.  This is done through your repo or "logbook", which should record what you have done in Lab 1 to Lab 4, including explanations and reflections on observations.  

This assessment accounts for 15% of the module.  The preferred route of submitting your logbook is through GitHub, but you may use other tools such as Notion or Obsidian.  You must complete the [SURVEY](https://forms.cloud.microsoft/e/mgcDRn9QdM) and grant me access to them.  My GitHub account name is 'pykc'.

The deadline for this is **16.00 on Friday 13 February 2026**.  
