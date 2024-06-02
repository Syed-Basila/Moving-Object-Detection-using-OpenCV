# Moving-Object-Detection-using-OpenCV
Moving object detection and tracking using OpenCV


![mocode](https://github.com/Syed-Basila/Moving-Object-Detection-using-OpenCV/assets/123718024/8a5d483e-75c2-437d-9205-cfe4ee9fcddb)

## Overview

This project demonstrates a simple method for detecting and tracking moving objects in a video feed using OpenCV and Python. It utilizes techniques like resizing, Gaussian blur, thresholding, and contour detection.

## Features

- Real-time object detection and tracking
- Draws bounding rectangles around detected objects
- Outputs detection status

## Demo



https://github.com/Syed-Basila/Moving-Object-Detection-using-OpenCV/assets/123718024/c0ce1b19-2a43-4912-90e8-edde8ea8a279



## Prerequisites

Ensure you have the following installed:

- Python 3.x
- OpenCV
- imutils

You can install the required packages using pip:

```sh
pip install opencv-python imutils
```
## Code Explanation
Here's a brief explanation of the main parts of the code:

1.Import necessary libraries

```sh
import cv2  # For image processing
import time  # For adding delay
import imutils  # For resizing the image
```
2.Initialize the camera and set parameters

```sh
cam = cv2.VideoCapture(0)
time.sleep(1)
firstFrame = None
area = 500
```
3.Read frames from the camera and process them

- Resize the image
- Convert to grayscale
- Apply Gaussian blur
- Calculate the absolute difference with the first frame
- Apply thresholding and dilation
- Find contours and draw rectangles around moving objects

```sh
while True:
    _, img = cam.read()
    text = "Normal"
    img = imutils.resize(img, width=500)
    grayImg = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
    gaussianImg = cv2.GaussianBlur(grayImg, (21, 21), 0)

    if firstFrame is None:
        firstFrame = gaussianImg
        continue

    imgDiff = cv2.absdiff(firstFrame, gaussianImg)
    threshImg = cv2.threshold(imgDiff, 25, 255, cv2.THRESH_BINARY)[1]
    threshImg = cv2.dilate(threshImg, None, iterations=2)
    cnts = cv2.findContours(threshImg.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE)
    cnts = imutils.grab_contours(cnts)

    for c in cnts:
        if cv2.contourArea(c) < area:
            continue
        (x, y, w, h) = cv2.boundingRect(c)
        cv2.rectangle(img, (x, y), (x + w, y + h), (0, 255, 0), 2)
        text = "Moving Object detected"

    print(text)
    cv2.putText(img, text, (10, 20), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 0, 255), 2)
    cv2
```
