---  
layout: post  
categories: programming  
tags: video   
  
---  
  
  
# Shots Detection On Matlab  
  
  
## Overview  
Modern computer vision has come up with a great many mathematic method of computing difference between two images. Apparently, the images between video shots are almost different enough to distinguish by comparing color histogram or invariant matrix.  
In this case, a useful application for shots detection on vs2010 is developed, based on invariant matrix method and color histogram analysis. The image data is mainly processed on matlab.  
  
### Env  
Environment required —— vs2010+opencv+win64  
Input —— video sequences  
Output —— sorted edge pictures  
  
### Archive Files  
matlab/ —— folder that contains some matlab dependencies   
loadfile.exe —— an auxiliary exe that initialize input images  
saveshots/ —— folder stores output images  
shotDetection.exe —— prime exe that detect the edge of shots  
VideoSeq/ —— stores original undetected video frames  
  
## Algorithm  
The built-in data structure, IplImage in openCV is a useful tool to store the video frames. In order to make it easier, I wrote loadfile.txt to read the frames from video and save them with regular names.  
Main Function  
ColorMoment(IplImage* img1, IplImage* img2);  
A user-defined funtion I wrote, which calculates the matrix invariants, specifically computing the former three matrix invariants of each pair of frames.  
  
double Histogram(IplImage* img1,IplImage* img2);  
Mainly call the built-in function in openCV, but has many manual options, on some attributes.  
  
  
## Demo  
Type in "1" : Detect shots by invariant matrix method  
Type in "2" : Detect shots by color histogram analysis  
  
### Invariant matrix method  
  
![d4](http://o8e9d1ezz.bkt.clouddn.com/d2222.jpg)    
  
The program will automatically read the images under VideoSequence folder, and analyze the difference between adjacent images frame by frame.  
  
Eventually, an Eular-distance table of each detection will be printed on the screen.  
  
By bare eyes, we could easily find the difference between the data, where normal Eular-distance is about e-13~e-11, while around edges, the number could be e-9.5.  
  
### Color Histogram Analysis    
  
![d3](http://o8e9d1ezz.bkt.clouddn.com/d33333.jpg)    
  
When using Color Histogram, we set the threshold at 0.5, so the computed Histogram-distance could tell the difference.  
  
Normally, when the adjacent images are almost the same, the Histogram-distance is above 0.95. When it comes to edges, the difference is obvious and the Histogram-distance plummets to 0.3.  
  
Additionally, I computed some other histograms such as correl and chisqr in opencv, which leads to the same conclusion.  
  
### Accurate outputs    
  
![d2](http://o8e9d1ezz.bkt.clouddn.com/d44444.jpg)  
  
As I tried dozens of times with different data, this program could somewhat accruately output edge images between continuous shots. At least it could be applied in some tiny   
  
