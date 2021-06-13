# Adversarial-Attack-on-OpenFace
Realization of the paper attached: `Unravelling Robustness of Deep Learning Based
Face Recognition against Adversarial Attacks`

## Execution
In order to successfully execute this code, open the .ipynb file in google collab. Upload the rest of the files in the files sections. Another file  I couldn't upload due to size also needs to be added in files section - download 
the file from this link - https://github.com/tzutalin/dlib-android/blob/master/data/shape_predictor_68_face_landmarks.dat  

Filename: `shape_predictor_68_face_landmarks.dat`
## Implementation of Attacks
To implement the digital adversarial attacks, we used OpenFace for Facial Recognition. Before moving on to the attacks, the implementation description of OpenFace Recognition System is provided. Followed by the OpenFace overview are the digital attacks that we implemented which includes Grid-based Occlusion, Most Significant Bit-based attack, and Eye-Region Based Occlusion. 

### OpenFace
OpenFace is a pretrained deep neural network model that uses FaceNet algorithm to extract facial features followed by the classification of the face. Before passing an input image to OpenFace classifier, we need to identify face in the image and then produce its projections. Once the landmarks are produced, we can load the OpenFace model using the pretrained weights.

### Face Identification and Projections:
The first step in facial recognition system is to identify the face located in an image. The face needs to be isolated from the background of the image. Face identification algorithm must be accurate enough to detect faces with poor and inconsistent lighting. Moreover, the angle or position of the faces can also vary greatly. We used Dlib for the face detection as it has better speed and accuracy over other algorithms. The detection works by first analyzing the pattern of the image using gradient points and then detecting part of the image where face is located using a trained pattern of Histogram of Oriented Gradient (HOG), which is a feature descriptor. 

After the face is detected from the image, we need to identify key points on a face, called landmarks. This can be done using face landmark estimation technique which detects 68 different points on the face locating eyes, nose, lips, and face edges.

### OpenFace Network
OpenFace Network
Once the preprocessing is done, OpenFace now extracts the facial features using FaceNet and produces numerical embedding of dataset images. These 128-dimensional embeddings of images are then used as train data along with their labels for training a machine learning model for classification of images. The model is saved and used for predictions. 


### Feeding Attacks

#### Grid-Based Occlusion 
This attack is a type of Image level distortion i.e. it is not specific to face and it randomly picks up the points from the image. Based upon the given parameter, numberOfgrids, we select the number of points along the upper and left boundaries of the image i.e., along y=0 and x=0 respectively. The numberOfgrids parameter signifies the number of grids to draw over the image i.e. number of points to choose to form grids. For every point, a corresponding point is choosen so that a line with every pixel on the line set to grayscale value of zero could be drawn. In order to choose corresponding pairs of point if x = 0 then x' would be equivalent to width of the image and if y = 0 then y' would be set to height of the image. After successfully selecting the pairs, a line is drawn to join both the points and the grayscale value of the pixels along the line is set to zero. 
To create grid lines over an image we loop over the corresponding points and and for each pair, we use cv2 library that has an inbuilt function - cv2.line() - to draw over the grid lines when given as input the image, corresponding points -one point as starting and the other as ending point, color of the line and thickness respectively.	

![image](https://user-images.githubusercontent.com/68595241/121817173-25224e80-cc99-11eb-964c-56ab02feb513.png)
![image](https://user-images.githubusercontent.com/68595241/121817182-366b5b00-cc99-11eb-9219-eb858c3065cc.png)

#### Eye-Region Based Occlusion
This is a Face-level distortion/attack as it requires the information of the facial landmarks. In order to identify the facial landmarks we have used the dlib library and cv2 library. Facial landmarks are utilized to restrict and speak to striking locales of the face, for example, to determine the location of eyes, eyebrows, nose, mouth, jaw, facial structure and lips. In order to locate these landmarks, we have used pythons dlib library. 
A shape predictor is used to localize the key features in an image. The pre-trained facial landmark detector we have used is "dlib.get_frontal_face_detector()" that is an inbuilt dlib library to approximate the location of 68 x and y-coordinates. These coordinates map to different facial structures on the face. Moreover, we move on to load the dlib shape predictor after we have successfully loaded the detector. The detector is responsible to find landmarks on an image and once these landmarks are found they are passed along with the image as a parameter to the predictor which then looks for these landmarks on the input image. The predictor returns the shape object containing 68 pairs of x and y coordinates specifying the facial landmarks. The pre-trained predictor model we used is "shape_predictor_68_face_landmarks.dat.bz2". 
Furthermore we have additionally utilized openface AlignDlib functionality for alignment during preprocessing of an image.This function preprocesses faces from an image so that may be passed on to neural network and they are also responsible to resize faces to an equal size and in our case 96 x 96. When faces are resized the landmarks for every image is at the similar area or location. In our project, AlignDlib takes as input "landmarks.dat" as a predictor model and aligns the faces by resizing them to same square dimensions i.e. (dimension x dimension). The landmarks regions over an image are displayed below and from these indexes we can extract x and y coordinates to manipulate the landmark region.


![image](https://user-images.githubusercontent.com/68595241/121817234-8c400300-cc99-11eb-95ba-1a8f6c8ecd58.png)


After successfully identifying facial landmarks using the DLIB library we draw over single occlusion band over the eye region of the image using the coordinates of the eyes landmark region identified using dlib library.


![image](https://user-images.githubusercontent.com/68595241/121817251-a2e65a00-cc99-11eb-8533-0ac86b877145.png)


![image](https://user-images.githubusercontent.com/68595241/121817267-b72a5700-cc99-11eb-879b-56d58a9503c5.png)


![image](https://user-images.githubusercontent.com/68595241/121817276-c5787300-cc99-11eb-87c6-ed19a0a50a1e.png)

#### Most Significant Bit-Based Attack
This attack is also an Image-Level Distortion. In this type of attack, random sets of pixels are selected from the image to flip their ith most significant bit. This produces color noise in the target image. For the implementation, three sets of pixels Y1, Y2 , Y3 are selected randomly such that each set contains the φ fraction of pixels from the image. This parameter φ determines the fraction of noise that needs to be added in the image, such that


`|Yi| = φ × Size of the image`

For each pixel Pj in Yi, the most significant bit of color values depending upon the value of i, is toggled using the following equation.

`Pjk = Pjk ⊕ 1`

The result of the most significant bit-based attack implemented on an image is shown below.

![image](https://user-images.githubusercontent.com/68595241/121817341-4c2d5000-cc9a-11eb-91fe-861570b06799.png)

This section sums up the implementation that we have done so far.

Group Members:


Shanzay Gauhar


Fatima Hasan


Nuzha Khalid


Samra Fakhar





