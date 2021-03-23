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
  1. [RCNN](https://github.com/rjnp2/Object_Detection/tree/main/1.%20RCNN)
  2. Fast RCNN \
    Instead of running a CNN 2,000 times per image, we can run it just once per image and get all the regions of interest (regions containing some object). \
    Ross Girshick, the author of RCNN, came up with this idea of running the CNN just once per image and then finding a way to share that computation across the 2,000 regions. In Fast RCNN, we feed the input image to the CNN, which in turn generates the convolutional feature maps. Using these maps, the regions of proposals are extracted. We then use a RoI pooling layer to reshape all the proposed regions into a fixed size, so that it can be fed into a fully connected network. \
    Let’s break this down into steps to simplify the concept: \
      1. As with the earlier two techniques, we take an image as an input.
      2. This image is passed to a ConvNet which in turns generates the Regions of Interest.
      3. A RoI pooling layer is applied on all of these regions to reshape them as per the input of the ConvNet. Then, each region is passed on to a fully connected network.
      4. A softmax layer is used on top of the fully connected network to output classes. Along with the softmax layer, a linear regression layer is also used parallely to output bounding box coordinates for predicted classes.

  
