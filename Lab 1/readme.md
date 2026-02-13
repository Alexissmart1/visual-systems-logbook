# Lab 1

# Task 1 - Image Rotation

The Requirement:

You are required to write a function which rotates a grey scale image by theta degrees radian

Note the following:

The resulting image should be the same size as the original. (i.e. the matrix storing the image should have the same dimensions, so some clipping of the image may occur).

If a source pixel lies outside the image you should paint it black.

Use “nearest pixel” only: for example if the source pixel required is (34.43,46.667) you should use the pixel at the location (34,47) in the source image.

The rotation should be performed about the centre of the image.

```
load clown 

imshow(X, map)

Theta = 30;      
Out = rotate(X, Theta);

figure
imshow(Out, map)

function [Out] =  rotate(In, Theta)

width=size(In,1);
height=size(In,2);

cp = [round(size(In,1)/2), round(size(In,2)/2)];

tm = [ cos(Theta), -sin(Theta);
       sin(Theta), cos(Theta) ];

rtm = inv (tm);

for y=1:height
 for x=1:width
  p =[x,y];			
  tp = round(rtm*(p-cp)'+cp');	
  if tp(1)<1 | tp(2)<1 | tp(1)>width | tp(2)>height
   Out(x,y)=0;			
  else
   Out(x,y)=In(tp(1),tp(2));	
  end
 end
end
end
```

<img width="578" height="340" alt="image" src="https://github.com/user-attachments/assets/a328fc4d-c4ff-42b9-968a-08ac1a33f7c8" />


# Task 2 - Image Shearing

The Requirement:

You are required to write a function which shears the input image in both the x and y direction and centres the result

Note the following:

The resulting image should be the same size as the original. (i.e. the matrix storing the image should have the same dimensions, so some clipping of the image may occur).

If a source pixel lies outside the image you should paint it black.

Use “nearest pixel” only, as before.

You should centre the sheared result (i.e. the centre pixel of the image remains stationary).

The shear values (Xshear, and Yshear) should be expressed as fractions of the images width and height respectively.

```
load clown
imshow(X, map)

Out = Shear(X, 0.1, 0.5);

figure
imshow(Out, map)

function Out = Shear(In, xshear, yshear)

height = size(In,1);  
width  = size(In,2);   


cp = [round(width/2), round(height/2)];

tm = [ 1,      xshear;
       yshear, 1     ];

rtm = inv(tm);


Out = zeros(height, width, 'like', In);

for y = 1:height
    for x = 1:width
        p  = [x, y];                        
        tp = round(rtm * (p - cp)' + cp');   

        sx = tp(1); 
        sy = tp(2);  

        if sx < 1 || sy < 1 || sx > width || sy > height
            Out(y, x) = 0;
        else
            Out(y, x) = In(sy, sx);
        end
    end
end
end
```

<img width="578" height="340" alt="image" src="https://github.com/user-attachments/assets/26ea515f-d52c-4039-91ba-16e315f6a47d" />
