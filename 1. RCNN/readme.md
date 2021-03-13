# RCNN

## [code](https://github.com/rjnp2/R-CNN-from-scratch-in-python)
## Intuition
Instead of working on a massive number of regions, the RCNN algorithm proposes a bunch of boxes in the image and checks if any of these boxes contain any object. RCNN uses selective search to extract these boxes from an image (these boxes are called regions).

Let’s first understand what selective search is and how it identifies the different regions. There are basically four regions that form an object: varying scales, colors, textures, and enclosure. Selective search identifies these patterns in the image and based on that, proposes various regions. Here is a brief overview of how selective search works:

- It first takes an image as input: \
![1](https://github.com/rjnp2/Object_Detection/blob/main/1.%20RCNN/images/rcnn1.png)

- Then, it generates initial sub-segmentations so that we have multiple regions from this image: \
![1](https://github.com/rjnp2/Object_Detection/blob/main/1.%20RCNN/images/rcnn2.png)

- The technique then combines the similar regions to form a larger region (based on color similarity, texture similarity, size similarity, and shape compatibility): \
![1](https://github.com/rjnp2/Object_Detection/blob/main/1.%20RCNN/images/rcnn3.png)

- Finally, these regions then produce the final object locations (Region of Interest).

Below is a succint summary of the steps followed in RCNN to detect objects:

- We first take a pre-trained convolutional neural network.
- Then, this model is retrained. We train the last layer of the network based on the number of classes that need to be detected.
- The third step is to get the Region of Interest for each image. We then reshape all these regions so that they can match the CNN input size.
- After getting the regions, we train SVM to classify objects and background. For each class, we train one binary SVM.
- Finally, we train a linear regression model to generate tighter bounding boxes for each identified object in the image.

You might get a better idea of the above steps with a visual example. So let’s take one!

- First, an image is taken as an input:
  ![1](https://github.com/rjnp2/Object_Detection/blob/main/1.%20RCNN/images/rcnn4.png)

- Then, we get the Regions of Interest (ROI) using some proposal method (for example, selective search as seen above):
  ![1](https://github.com/rjnp2/Object_Detection/blob/main/1.%20RCNN/images/rcnn5.png)

- All these regions are then reshaped as per the input of the CNN, and each region is passed to the ConvNet:
  ![1](https://github.com/rjnp2/Object_Detection/blob/main/1.%20RCNN/images/rcnn6.png)

- CNN then extracts features for each region and SVMs are used to divide these regions into different classes:
  ![1](https://github.com/rjnp2/Object_Detection/blob/main/1.%20RCNN/images/rcnn7.png)

- Finally, a bounding box regression (Bbox reg) is used to predict the bounding boxes for each identified region:
  ![1](https://github.com/rjnp2/Object_Detection/blob/main/1.%20RCNN/images/rcnn8.png)

And this, in a nutshell, is how an RCNN helps us to detect objects.

## Problems with RCNN
So far, we’ve seen how RCNN can be helpful for object detection. But this technique comes with its own limitations. Training an RCNN model is expensive and slow thanks to the below steps:

- Extracting 2,000 regions for each image based on selective search
- Extracting features using CNN for every image region. Suppose we have N images, then the number of CNN features will be N*2,000
- The entire process of object detection using RCNN has three models:
  - CNN for feature extraction
  - Linear SVM classifier for identifying objects
  - Regression model for tightening the bounding boxes.
  
All these processes combine to make RCNN very slow. It takes around 40-50 seconds to make predictions for each new image, which essentially makes the model cumbersome and practically impossible to build when faced with a gigantic dataset.

