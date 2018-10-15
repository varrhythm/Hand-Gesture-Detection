# README.md file
This is README.me file For the Python Mini project based on Hand gesture detection


PROCEDURE :

The following steps are needed to execute the code successfully:

1.Download Python 3.6 or any version of Python which is 3 or above. This version of python has many built in libraries. But the for the successful execution of the project , we need to pip install using Command prompt , the following libraries opencv-python , matplotlib ,time,requests and numpy.

2.Once all the required softwares and libraries are downloaded then we can open the Python  IDLE 3.6 and then write the given code in it.
3.We import cv2,numpy as np ,pyautogui and time in the program.

4.Then we start Capturing the RGB image in the variable “cap” and declare two variables ct and ct & initialise them to 0 .
cap = cv2.VideoCapture(0)
ct=0
ct1=0

5.Then we give a time sleep of 5 seconds before the main part of the program is executed.

6.Then we take a frame out of the video captured and store it in the variable frame.
    _, frame = cap.read()

7. The RGB image is translated to HSV format.
    hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)

8.Now we define the range of the skin colour in HSV and store it in some variables:
    lower_blue = np.array([0,48,80])
    upper_blue = np.array([20,255,255])

9.Then  the HSV image is masked  to get only skin colors
    mask = cv2.inRange(hsv, lower_blue, upper_blue)

10.We take  Bitwise-AND of  the mask and original image and store it in “res”. This will find the bitwise of every pixels as well as their RGB values.
    res = cv2.bitwise_and(frame,frame, mask= mask)

11.Then we apply the blur affect like the “Gaussian affect”  so that the image becomes smooth.
    res = cv2.GaussianBlur(res,(10,10),0)
    blur = cv2.GaussianBlur(mask,(15,15),0)

12.Then the threshold is added to filter the image further more (like reducing the noise found in the image etc.)
               ret, thresh = cv2.threshold(blur, 127, 255, cv2.THRESH_BINARY+cv2.THRESH_OTSU)
    #thresh = cv2.adaptiveThreshold(blur,255,cv2.ADAPTIVE_THRESH_MEAN_C,cv2.THRESH_BINARY,11,2)

13. So once the image gets filtered , then we can go for detecting all the possible contours of the detected hand .The hand should be kept in a minimum distance from the webcam so that an accurate image is captured.Then we find the contour having the maximum area and store it under a variable.
    _,contours,hierarchy = cv2.findContours(thresh,cv2.RETR_TREE, cv2.CHAIN_APPROX_NONE)
    if not contours:
        continue
    cnt = contours[0]
    if(len(contours))>=0:
        c=max(contours, key=cv2.contourArea)
        (x,y),radius=cv2.minEnclosingCircle(c)
        M=cv2.moments(c)
    else:
        print("Sorry no contour found")

14. Now we start plotting the convex hull over  the traced contours.The straight line is indicated with a green whereas the sudden edges are marked by red dots. The convex hull is then drawn on the maximum contour  by removing the defects and  plotting the line with a green color and a red dot to indicate the sudden turns
                 cnt=c     (initialisation)
    if cv2.contourArea(cnt)<=1000:
        continue
    hull = cv2.convexHull(cnt,returnPoints = False)
    defects = cv2.convexityDefects(cnt,hull)
    #epsilon = 0.01*cv2.arcLength(cnt,True)
    #approx = cv2.approxPolyDP(cnt,epsilon,True)
    count=0;
    try:
        defects.shape
        for i in range(defects.shape[0]):
            s,e,f,d = defects[i,0]
            start = tuple(cnt[s][0])
            end = tuple(cnt[e][0])
            far = tuple(cnt[f][0])
            cv2.line(frame,start,end,[0,255,0],2)
            cv2.circle(frame,far,5,[0,0,255],-1)
            count=count+1

15. Based on the contour area the space bar  is pressed.If the area is greater than  2000 pixels,then the video                       is played and if the area is between 500 and 1500 pixels,then the video is paused. Otherwise an “error ” is printed.
if cv2.arcLength(cnt,True)>2000:
            while ct==0:
                print("ON")
                pyautogui.press('space')
                ct=1
                ct1=0
        if cv2.arcLength(cnt,True)>500 and cv2.arcLength(cnt,True)<=1500:
            while ct1==0:
                print("OFF")
                pyautogui.press('space')
                ct1=1
                ct=0
          except AttributeError:
          print("shape not found")    

16.	  In the end we show the mask,resolution and frame as the output.

            cv2.imshow('final',frame)
    cv2.imshow('mask', mask)
    cv2.imshow('res', res)

17.	To terminate the program we press Esc key(ASCII 27).

       
18.  Now with the code running in the background check whether the video played in vlc media player(Which inputs space bar to either start or stop)

   
