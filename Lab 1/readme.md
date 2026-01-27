# Lab 1

# Task 1

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

# Task 2

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
