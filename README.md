# Object Detection

## What is Object Detection?
Object Detection is a common Computer Vision problem which deals with identifying and locating object of certain classes in the image. Interpreting the object localisation can be done in various ways, including creating a bounding box around the object or marking every pixel in the image which contains the object (called segmentation).

The below image is a popular example of illustrating how an object detection algorithm works. Each object in the image, from a person to a kite, have been located and identified with a certain level of precision.

![1](https://github.com/rjnp2/Object_Detection/blob/main/images/2.png)

It allows for the recognition, localization, and detection of multiple objects within an image which provides us with a much better understanding of an image as a whole. It is commonly used in applications such as image retrieval, security, surveillance, and advanced driver assistance systems (ADAS).

Object Detection can be done via multiple ways:

- Feature-Based Object Detection, Viola Jones Object Detection, SVM Classifications with HOG Features \
  Applying standard, computer-vision based object detection methods (i.e., non-deep learning methods) such as sliding windows and image pyramids — this method is typically used in .

- Deep Learning Object Detection \
  Taking the pre-trained network and using it as a base network in a deep learning object detection framework (i.e., Faster R-CNN, SSD, YOLO).

Object detection was studied even before the breakout popularity of CNNs in Computer Vision. While CNNs are capable of automatically extracting more complex and better features, taking a glance at the conventional methods can at worst be a small detour and at best an inspiration.

Object detection before Deep Learning was a several step process, starting with edge detection and feature extraction using techniques like SIFT, HOG etc. These image were then compared with existing object templates, usually at multi scale levels, to detect and localize objects present in the image.

## BY USING DEEP LEARNING

- **Method: The traditional object detection pipeline**

  Let’s start with the simplest deep learning approach, and a widely used one, for detecting objects in images – Convolutional Neural Networks or CNNs.
  
  We pass an image to the network, and it is then sent through various convolutions and pooling layers. Finally, we get the output in the form of the object’s class. For each input image, we get a corresponding class as an output. 

  Let’s look at how we can solve a general object detection problem using a CNN.

  1. First, we take an image as input:

      ![1](https://github.com/rjnp2/Object_Detection/blob/main/images/3.png)

  2. Then we divide the image into various regions:

      ![1](https://github.com/rjnp2/Object_Detection/blob/main/images/4.png)

  3. We will then consider each region as a separate image.
  4. Pass all these regions (images) to the CNN and classify them into various classes.
  5. Once we have divided each region into its corresponding class, we can combine all these regions to get the original image with the detected objects:

      ![1](https://github.com/rjnp2/Object_Detection/blob/main/images/5.png)

   The problem with using this approach is that the objects in the image can have different aspect ratios and spatial locations. For instance, in some cases the object might be covering most of the image, while in others the object might only be covering a small percentage of the image. The shapes of the objects might also be different (happens a lot in real-life use cases).

   As a result of these factors, we would require a very large number of regions resulting in a huge amount of computational time. So to solve this problem and reduce the number of regions, we can use region-based CNN, which selects the regions using a proposal method.

- **Understanding Region-Based Convolutional Neural Network/ Base network of an object detection framework** \
  The second method to deep learning object detection allows you to treat your pre-trained classification network as a base network in a deep learning object detection framework (such as Faster R-CNN, SSD, or YOLO).

  The benefit here is that you can create a complete end-to-end deep learning-based object detector.
  
  Base networks are your common (classification) CNN architectures, including:
  - VGGNet
  - ResNet
  - MobileNet
  - DenseNet
  
  Typically these networks are pre-trained to perform classification on a large image dataset, such as ImageNet, to learn a rich set of discerning, discriminating filters.

  1. Intuition of RCNN
      Instead of working on a massive number of regions, the RCNN algorithm proposes a bunch of boxes in the image and checks if any of these boxes contain any object. RCNN uses selective search to extract these boxes from an image (these boxes are called regions).

Let’s first understand what selective search is and how it identifies the different regions. There are basically four regions that form an object: varying scales, colors, textures, and enclosure. Selective search identifies these patterns in the image and based on that, proposes various regions. Here is a brief overview of how selective search works:

It first takes an image as input:

Then, it generates initial sub-segmentations so that we have multiple regions from this image:

The technique then combines the similar regions to form a larger region (based on color similarity, texture similarity, size similarity, and shape compatibility):

Finally, these regions then produce the final object locations (Region of Interest).
Below is a succint summary of the steps followed in RCNN to detect objects:

We first take a pre-trained convolutional neural network.
Then, this model is retrained. We train the last layer of the network based on the number of classes that need to be detected.
The third step is to get the Region of Interest for each image. We then reshape all these regions so that they can match the CNN input size.
After getting the regions, we train SVM to classify objects and background. For each class, we train one binary SVM.
Finally, we train a linear regression model to generate tighter bounding boxes for each identified object in the image.
You might get a better idea of the above steps with a visual example (Images for the example shown below are taken from this paper) . So let’s take one!

First, an image is taken as an input:

Then, we get the Regions of Interest (ROI) using some proposal method (for example, selective search as seen above):

All these regions are then reshaped as per the input of the CNN, and each region is passed to the ConvNet:

CNN then extracts features for each region and SVMs are used to divide these regions into different classes:

Finally, a bounding box regression (Bbox reg) is used to predict the bounding boxes for each identified region:

And this, in a nutshell, is how an RCNN helps us to detect objects.

2.2 Problems with RCNN
So far, we’ve seen how RCNN can be helpful for object detection. But this technique comes with its own limitations. Training an RCNN model is expensive and slow thanks to the below steps:

Extracting 2,000 regions for each image based on selective search
Extracting features using CNN for every image region. Suppose we have N images, then the number of CNN features will be N*2,000
The entire process of object detection using RCNN has three models:
CNN for feature extraction
Linear SVM classifier for identifying objects
Regression model for tightening the bounding boxes.
All these processes combine to make RCNN very slow. It takes around 40-50 seconds to make predictions for each new image, which essentially makes the model cumbersome and practically impossible to build when faced with a gigantic dataset.

Here’s the good news – we have another object detection technique which fixes most of the limitations we saw in RCNN.
