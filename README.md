# Adversarial-Attack-on-OpenFace
Realization of the paper attached.

## Execution
In order to successfully execute this code, open the .ipynb file in google collab. Upload the rest of the files in the files sections. Another file  I couldn't upload due to size also needs to be added in files section - download 
the file from this link - https://github.com/tzutalin/dlib-android/blob/master/data/shape_predictor_68_face_landmarks.dat  

Filename: shape_predictor_68_face_landmarks.dat
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
	

