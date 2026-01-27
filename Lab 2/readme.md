# Lab 2

# Task 1 - Find your blind spot

when closing one eye and moving towards the computer screen, the circle disappeared and became white. Brain guesses what should be there by using surrounding information

# Task 2 - Ishihara Colour Test

Identifying numbers within the circles:
1/ 74
2/ 6
3/ 16
4/ 2
5/ 29
6/ 7
7/ 45
8/ 5
9/ 97
10/ 8
11/ 42
12/ 3

# Task 3 - Reverse colour

When staring at the image the cones become overstimulated and fatigued. Thus the cones reduce their response and sensetivity to certain colours so when the image changes to a white background a negative response of the flag image is produced
# Task 4 - Troxler's Fading
Peripheral details disappear when fixated on one point for a while. Occurs due to neural adaptation in the visual system. When a stimulus remains constant and unchanging, retinal and cortical neurons begin to reduce their response to it.

In video it causes the blue ring to disappear

# Task 5 - Brain sees what it expects

Blue table appears to be longer but theyre a similar size. 
The brains interpretation of depth and persception causes the image to be interpreted as 3d objects with assumptions of size and length. 

Similarly the brain uses context to interpret the squares as different colours when they are not

# Task 6 - The Grid Illusion

Black dots appear to replace white dots on the grid due to lateral inhibition

# Task 7 - Cafe Wall Illusion
Lines in the wall appear to bend or tilt when they are static and parallel

# Task 8 - the Silhouette Illusion

Lack of video information and context cause confusion with rotation and direction.
# Task 9 - the Incomplete Triangles

There could be a white triangle on top of a black boardered triangle. Brain is creating assumptions and creating information
# Task 10 - Convert RGB image to Grayscale
```
imfinfo('peppers.png')
```
```
Filename: 'C:\Users\Alex\Downloads\Lab2-Colour-Perception-main\Lab2-Colour-Perception-main\matlab\peppers.png'
               FileModDate: '22-Jan-2026 09:46:48'
                  FileSize: 711722
                    Format: 'png'
             FormatVersion: []
                     Width: 777
                    Height: 584
                  BitDepth: 24
                 ColorType: 'truecolor'
           FormatSignature: [137 80 78 71 13 10 26 10]
                  Colormap: []
                 Histogram: []
             InterlaceType: 'none'
              Transparency: 'alpha'
    SimpleTransparencyData: []
           BackgroundColor: []
           RenderingIntent: []
            Chromaticities: []
                     Gamma: []
               XResolution: []
               YResolution: []
            ResolutionUnit: []
                   XOffset: []
                   YOffset: []
                OffsetUnit: []
           SignificantBits: []
              ImageModTime: []
                     Title: []
                    Author: []
               Description: []
                 Copyright: []
              CreationTime: []
                  Software: []
                Disclaimer: []
                   Warning: []
                    Source: []
                   Comment: []
                 OtherText: {1×2 cell}
         AutoOrientedWidth: 777
        AutoOrientedHeight: 584
```
```
RGB = imread('peppers.png');  
imshow(RGB)

```
<img width="881" height="662" alt="image" src="https://github.com/user-attachments/assets/7c3b1de3-bdd6-43d8-a689-8a741f817aec" />

```
RGB = imread('peppers.png');  
imshow(RGB)
I = rgb2gray(RGB);
figure              % start a new figure window
imshow(I)
```
<img width="881" height="662" alt="image" src="https://github.com/user-attachments/assets/7edc13b3-1e81-47c2-9693-deee1b27183e" />
```

```
RGB = imread('peppers.png');  
imshow(RGB)
I = rgb2gray(RGB);
figure              % start a new figure window
imshow(I)
imshowpair(RGB, I, 'montage')
title('Original colour image (left) grayscale image (right)');
```
<img width="1045" height="420" alt="image" src="https://github.com/user-attachments/assets/81357344-50e2-4db0-832c-9519e459b61f" />
```

# Task 11 - Splitting an RGB image into separate channels

```
RGB = imread('peppers.png');  
imshow(RGB)
I = rgb2gray(RGB);
figure              % start a new figure window
[R,G,B] = imsplit(RGB);
montage({R, G, B},'Size',[1 3])
```
<img width="1045" height="266" alt="image" src="https://github.com/user-attachments/assets/876700d8-101f-4f39-aaba-9c3a0e90f20d" />

# Task 12 - Map RGB image to HSV space and into separate channels

```
RGB = imread('peppers.png');  
imshow(RGB)

HSV = rgb2hsv(RGB);
[H,S,V] = imsplit(HSV);
montage({H,S,V}, 'Size', [1 3])
```
<img width="782" height="219" alt="image" src="https://github.com/user-attachments/assets/9d3499c3-9f6c-4d89-9614-58f6ae972d2a" />

# Task 13 - Map RGB image to XYZ space

```
RGB = imread('peppers.png');  
imshow(RGB)

XYZ = rgb2xyz(RGB);
[X,Y,Z] = imsplit(XYZ);
montage({X, Y, Z},'Size',[1 3])
```

<img width="782" height="219" alt="image" src="https://github.com/user-attachments/assets/f99fff45-f82c-49b7-bb92-8e2d9823d984" />



