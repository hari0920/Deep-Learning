# Negative Space of Images

Negative space images are the images with just black and white contours. 

## Initial testing:
* Testing is done using Tensorflow on Imagenet dataset. 
Lets try to test how well negative space images are being recognised in Imagenet and AlexNet. 

Lets take the image of a dog, as they are being the category trained to a maximum. 

Image link: [https://s-media-cache-ak0.pinimg.com/736x/3d/21/6c/3d216cf64b7ffa13a8b93e5d9ba0c597.jpg]

Results On ImageNet:
* bow tie, bow-tie, bowtie (score = 0.93169)
* sunglasses, dark glasses, shades (score = 0.00528)
* sunglass (score = 0.00244)
* hair slide (score = 0.00132)
* long-horned beetle, longicorn, longicorn beetle (score = 0.00099)

It got it partially wrong. One reason, we can easily see a bow tie and the network attempted to detect the tie[edit]

Lets remove the bow tie from the image by cropping it to 368*161 from 736*552. Lets run.

Results:
* vase (score = 0.25941)
* cuirass (score = 0.04524)
* buckle (score = 0.03209)
* mask (score = 0.03193)
* breastplate, aegis, egis (score = 0.02177)

It comprehended the nose and mouth together to be a vase. Wow. Breastplate seriously? Maybe the eyes :P

# Now we will try to find why is it so?

One reason could be that the network doesnt learn much about white well pixels. Shape is important for an image, curvature. The depth of an image in white is literally nothing.

Lets try some filters to find out which can create such outlines or borders. 
[1] - Uses AI to detect boundaries. 

Points from [1]:
     * Predicting boundaries by exploiting object level features from a pretreained object classification network. ...Hmmm.. similar to ours;
     * High level object features inform the low level boundary detection. 
     * Uses trained VGG net.
     * Presents improvement on semantic boundary labeling, semantic segmentation and object proposal generation. 
     * Boundary prediction:
       		* Spectral methods - uses eigenvalue [MCG detector, gPb detector, PMI detector and Normalized cuts]
		* Supervised discriminative - Sketch tokens, Structured edges, and sparse code gradients(SCG)
		* Deep learning - N4 fields (dictionary learning and Nearest neighbor), DeepNet (CNN), DeepEdge (multi-scale bifurcated network to perform contour detection).
    * This paper doesnt use the multi-scale bifurcated network. (Can run in real time).
    * The paper avoids feature engineering by learning from human annotated data. 
    * Method and architecture:
		* Extract candidate contour points with a high recall. using SE edge detector. ** This step is done inorder to reduce the computational cost. ** 
		* Sample up the original image to 1100*1100. ** Done inorder to reduce the loss of information ** 
		* Uses VGGnet as a model because it has been trained with large number of object classes (1000s). ** To preserve spatial information we use a fully convolutional network. ** Spatial information is crucial for accurate boundary detection. 
		* Feature interpolation :
		  	  ^^ After the up-sampled image passes through all the 16 conv layers for a selected candidate contour point, we find its corresponding point in the feature maps. The values are not exact. SO we perform feature interpolation by finding the 4 nearest points and average their activation values. 
		* Feature interpolation helps to predict boundaries efficiently. 
		* We feed the 5504 dimensional feature vector to funlly connected layers that are optimized to human agreement criterion. Whaaat.
		* It aims at mimicking the judgement of human labelers.
		* finally fed into two fully connected layers. 
    * To learn the weights in the two fc layers, we train our model to optimize the least square error of the regression. 
    * BSDS500 dataset involves "orphan boundaries", these are the boundaries marked by only one or two human annotators. 

## Notes of caffe installation:
      Caffe installation is a pain. It can lead to soo many stupid bugs while installation. 

* Install numpy using `pip install numpy` and not `apt install python-numpy`. 
* Install cython before the `pip install -r requirements.txt` 
* Install scikit-image (This requires cython) 
* dont cmake before all the requirements are done. This can mainly lead to python wrapper not getting installed. 
* If there is a problem, feel free to delete the build directory. No harm. 
* Dont forget to set $PYTHON_PATH and CAFFE_ROOT. 
* `make all` fails in mac osx. No clue why. have to find out.

errors:
Import _caffe failed: Meaning pycaffe wasnt compiled due to some modules from pip not installed.
Import skiimage.io failed: scikit-image not installed; Check if cython is installed.

## Spectral clustering. 


Points from [2]:

    
Aim :

* How to find these contours?
* How to color based on these contours?
    

References:
[1] https://arxiv.org/pdf/1504.06201v3.pdf - High-for-low and low-for-high: Efficient boundary detection from deep object features and its application to high-level vision.

