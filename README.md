# FashionSeg - Semantic Segmentation Dataset for Fashion Styles

##Abstract
In this project, I create a tensorflow-compatible fashion dataset for semantic segmentation based on style, rather than on individual apprel pieces. I apply flipping and rotation to the dataset for data augmentation and train a simple, UNet-like encoder-decoder network to recognize fashion style. Although the result is disappointing, there are valuable lessons learned that can hopefully be applied in similar projects in the future. 

##Motivation
Fashion has always been on the radar of computer vision researchers, although current CV application in the field is still limited due in part to the variety of apprel styles and non-uniform input data: a piece of clothing can look very different between user-generated images. Consequently, segmentation of fashion apprel from real-world images and videos remains a challenge. Research on both segmentation and design (see The 2020 CV for Fashion workshop: https://sites.google.com/view/cvcreative2020) focuses on extracting indivudal apprel information before identifying/building fashion styles. I propose a more direct means to achieve theses tasks: train models to recognize styles first. In order to do so, a dataset based on style is needed. 

##Dataset
Building a tensorflow dataset is a major part of this project. I use 18 user-generated, 700x500 images from Pinterest, belonging to 3 different fashion styles, i.e. "bohemian," "preppy," and "streetwear." I then use Oxford's VGG Image Annotator (https://www.robots.ox.ac.uk/~vgg/software/via/) to create masks for each image. I have visualized an image and its mask below: 
![1](https://user-images.githubusercontent.com/85959496/122109839-d3d6a400-cdeb-11eb-8ed8-274917e7073f.png)
![2](https://user-images.githubusercontent.com/85959496/122109863-d89b5800-cdeb-11eb-918a-047a75bbd368.png)

In the tensorflow dataset object, I convert the pixel in each mask to correspond to the class the pixel belongs in the original image (1-3 for styles, 0 for background). I split the dataset into testing and training sets by a 10/90 split since my dataset is relatively small. I apply horizontal and verticle flipping, and rotation to the training dataset for data augmentation.
![3](https://user-images.githubusercontent.com/85959496/122110061-1ac49980-cdec-11eb-916d-9558be170d32.png)

##Semantic Segmentation Model
I train an UNet-like encoder-decoder model for 2 epochs using Adam optimizer with learning rate from 0.1 to 0.0001. 

##Model Evaluation
Due to the simplicity of the model and short training time, the accuracies of the model never goes above 0.85 during training, for learning rates (0.1-0.0001). All the predicted masks seem to be dominated by 0s (the background label), which indicates the model is too weak to perform segmentation of apprel.

##Takeaway and Future Work
In this project, I built a fashion CV pipeline from start, i.e. individual images, to finish. Along the way, I learned about data processing, model construction and training in Tensorflow.  A reaon why a larger, UNet-like model wasn't used for training was due to the fact that 750x500 is not standard input size. Consecutive pooling and convolution need to be carefully deisgned so as not to inadvertently lose information (e.g. pooling a odd-sized dimension by an even number). Future work thus may include building a larger, standard-sized dataset, with more than 1 styles per image. Another way to fix this issue is to pre-process input data so images are cropped into standard-sized square before being fed into the model. 
