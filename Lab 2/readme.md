# Lab 2

# Task 10
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

# Task 11

```
RGB = imread('peppers.png');  
imshow(RGB)
I = rgb2gray(RGB);
figure              % start a new figure window
[R,G,B] = imsplit(RGB);
montage({R, G, B},'Size',[1 3])
```
<img width="1045" height="266" alt="image" src="https://github.com/user-attachments/assets/876700d8-101f-4f39-aaba-9c3a0e90f20d" />

# Task 12
