#### Dependecy requirements(libraries):

```
open-cv python 4.5.1.48
tensorflow 2.5.0
tensorflow-estimator 2.6.0
tensorflow-gpu 2.6.0 
scikit-image 0.18.1
numpy 1.19.5
imutils 0.5.4
```
#### Hardware Requirements
```
NVIDIA GPU Driver
CUDA Toolkit
CUPTI
cuDNN SDK 
```
Install these drivers from <a href ="https://www.tensorflow.org/install/gpu">here.</a>

#### Approach:

* Take snaps of the video every few seconds
* From those images,extract all the contours
* Find the bounding rectangles in these images
* Apply image segmentation to extract the part containing the license plate
* Recgnize the license plate characters using OCR

#### Methodology:

* Applying Gaussian Blur 
* Detecting the vertical edges(feature detection)
* Applying Otsu's Thresholding on the vertical edge image
* Applying Closing Morphological Transformation on thresholded image
* Extracting all the contours
* Applying image segmentation(adaptive thresholding,binarization,) to extract all the characters 
* Build a neural network to predict the characters on the extracted license plate.

#### Reference from which the model was built

<a href="https://www.geeksforgeeks.org/detect-and-recognize-car-license-plate-from-a-video-in-real-time/">https://www.geeksforgeeks.org/detect-and-recognize-car-license-plate-from-a-video-in-real-time/</a>

#### Tips:

* Downgrade tensorflow to version 1.14 or 1.15. In case youre working with tensorflow versions > 2.5(like me), change:

    * import tensorflow as tf to import tensorflow.compat.v1 as tf(for me the first one worked)
    
    * cnts = cnts[0] if imutils.is_cv2() else cnts[1] to cnts = cnts[1] if imutils.is_cv2() else cnts[0] as in OpenCV 3.4.X, the cv2.findContours() function returns 3 items, so to obtain the actual contours, you need to grab the 2nd return value. 
    
    * _, contours, hier = cv2.findContours(charCandidates, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE) to contours, hier = cv2.findContours(charCandidates, cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_SIMPLE).

    * tf.gfile.GFile to tf.io.gfile.GFile

    * tf.GraphDef() to tf.compat.v2.io.gfile.GFile()   

