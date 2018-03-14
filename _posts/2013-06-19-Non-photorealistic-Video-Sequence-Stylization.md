---
layout: post
date:   2013-06-19
categories: image-processing

---


# Non-photorealistic Video Sequence Stylization

## Overview
Usually, we make animations through strenuous work of drawing, editing and composing. However, as a real video could somehow be easily recorded even by a cellphone, it is efficient to produce animations by stylizing videos.

Among all the non-photorealistic animation, video sequence stylization is an useful and widely-applied approach of making real videos into comic-like animations.

My purpose of this program is to briefly stylize the brush and mesh with certain algorithm, in which case to turn a movie into a cartoon.

## Configuration
### Environment and IO  
Environment required —— vs2010+opencv+win64  
Input —— video sequences and user options  
Output —— stylized video sequences  

### Archive Files  
brush.h —— definition of brush type  
loadfile.txt —— initialization of original data  
video_stylized.cpp —— main algorithm  


## Algorithm
The algorithm about how to use the brush is briefly summarized below. For more detailed explanation, please check the source code.

Input the mesh and a single frame
Confirm the thresholds

```c++
#define PI 3.1416
#define BRUSHLENGTH 30
#define BRUSHWIDTH 10
#define BRUSHTHETA 1.04719 
```

```c++
//Initialize the brush
Brush brush=new Brush(length,width,theta,cvcolor)
//Map the mesh to image
for(int u = 0; u < netsize.width; u++){    
   for(int v = 0; v < netsize.height; v++){    
       int i=1.0*u/netsize.width*imagesize.width;  
           int j=1.0*v/netsize.height*imagesize.height;    
 
//map each cell in the mesh to that in the image, acquiring pixel information
brush.BrushColor = cvGet2D(source, j, i); 
```




```c++
//Calculate the gaps between brushes
point1.x= i+brush.BrushLength/2*cos(brush.BrushTheta)+brush.BrushWidth/2*sin(brush.Bru shTheta);
point1.y = j-brush.BrushLength/2*sin(brush.BrushTheta)+brush.BrushWidth/2*cos(brush.Bru shTheta);
```


```c++
//Draw the brush
netbased_stylization(framesize,net1,videoframe,stylizedframe)
```



## Demo

![d1](http://o8e9d1ezz.bkt.clouddn.com/d1.jpg)
To start, there are 3 options of brush size: small medium large
Small: the original density of pixels
Medium: halve the size in small mode
Large: halve the size in medium mode

![d2](http://o8e9d1ezz.bkt.clouddn.com/d2.jpg)

As we can see, the whole image has been slightly blurred on pixels, which seems like a picture, though some edges could be still sharp. However, watched in distance, it still remains its overall structure and characteristics, which is acceptable in real videos.



![d3](http://o8e9d1ezz.bkt.clouddn.com/d3.jpg)
Medium brush uses twice as big as small brush. Obviously, the image is a little bit more twisted than that in small mode.


![d4](http://o8e9d1ezz.bkt.clouddn.com/d4.jpg)
In large mode, the big size of brush makes the whole image patched like an oil painting. Predictably, if we could optimize the size of brush, it can output a ideal stylized image.