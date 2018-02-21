# **German Traffic Sign Recognition** 

## Author : Sandeep Patil

### Objective of this project to classify German Traffic signs using convolution neural network and Tensorflow API.   
#### My final model was able to achieve 97.7% validation accuracy.  
---

[//]: # (Image References)

[training_distribution]: ./summary_images/training_data_distribution.png "distribtion"
[training_distribution1]: ./summary_images/training_data_distribution1.png "distribtion"

[sample_images]: ./summary_images/sample_images.png "sample images"
[new_images]: ./summary_images/new_images.png "new images"
[top_5_results]: ./summary_images/top_5_results.png "top 5 probabilities"
[top_5_results1]: ./summary_images/top_5_results1.png "top 5 probabilities"
[top_5_results2]: ./summary_images/top_5_results2.png "top 5 probabilities"
[top_5_results3]: ./summary_images/top_5_results3.png "top 5 probabilities"
[top_5_results4]: ./summary_images/top_5_results4.png "top 5 probabilities"
[top_5_results5]: ./summary_images/top_5_results5.png "top 5 probabilities"
[top_5_results6]: ./summary_images/top_5_results6.png "top 5 probabilities"
[top_5_results7]: ./summary_images/top_5_results7.png "top 5 probabilities"


[loss_1]: ./summary_images/loss_1.png "loss"
[validation_1]: ./summary_images/validation_1.png "validation accuracy"
[loss_2]: ./summary_images/loss_2.png "loss"
[validation_2]: ./summary_images/validation_2.png "validation accuracy"
[loss_3]: ./summary_images/loss_3.png "loss"
[validation_3]: ./summary_images/validation_3.png "validation accuracy"
[loss_4]: ./summary_images/loss_4.png "loss"
[validation_4]: ./summary_images/validation_4.png "validation accuracy"
[loss_5]: ./summary_images/loss_5.png "loss"
[validation_5]: ./summary_images/validation_5.png "validation accuracy"



[augmented_data]: ./summary_images/augmented_data.png "augmented data"
[improved_contrast]: ./summary_images/improved_contrast.png "improved constast"


### The details of training data 

* Number of training examples: 34799  
* Number of testing examples : 12630  
* Image data shape : 32 x 32 x 3  
* Number of classes: 43  

The distribution of images of each class was not even. Following is bar chart showing numbers of images for each class.

![Training distribution][training_distribution] 

Following are some input image samples :

![Sample images][sample_images] 

---

### The neural network architecture  

** I started with LeNet model and did following modifications. **
* Increased number of filters in convolution layers.
* Increased number of neurons of fully connected layers.
* Added drop out layers after each fully connected layer. 

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x1 Gray scale image   					| 
| Convolution 5x5     	| 1x1 stride, valid padding, outputs 28x28x8 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 14x14x8  				|
| Convolution 5x5	    | 1x1 stride, valid padding, outputs 10x10x24   |
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 5x5x24  					|
| Flatten  	      		| outputs 600				 					|
| Fully connected L1	| Outputs 256  									|
| RELU					|												|
| Dropout				| 50%											|
| Fully connected L2	| Outputs 128  									|
| RELU					|												|
| Dropout				| 50%											|
| Fully connected L3	| Outputs 43 logits 							|
| Softmax				| Multi class classifier						|

---

### The Model training 
Following hyper parameters used during training of model.

### Approach 1 : Do not use pre processing or data augmentation 
  
For this case only the input size was 32 x 32 x 3 (RGB)

** Hyper parameters **
Total input images : 34799 ; Batch size : 128 ;  Number of epochs : 10 ; Number of iterations per epoch : 272 ;  Constant Learning rate : 0.001 ;

** Results ** 
* Validation accuracy : 87.3 %
* There is sudden recovery of loss at start this could be higher learning rate for model. 
* There is gap between trainig accuracy and validation accuracy that shows that eighter model has become complex or we need additional data.  

| Loss over iterations |  Training and validation accuracy | 
|---|---|
|![loss][loss_1] | ![validation accuracy][validation_1] | 

---

### Approach 2 : Do pre processing or but not do data augmentation

** Pre processing done on images : **
* Improve contrast by converting RGB to HLS and equalized histogram of lightness channel 
* Convert image to gray scale
* Normalize data by assuming mean of 128 and converting to standard deviation of 1. 

![improved contrast][improved_contrast]

** Hyper parameters **
It were same as approach 1

** Results ** 
* Validation accuracy was 95.6
* The loss reduction is near to idea.
* There is still gap between training accuracy and validation accuracy that shows that either model has become complex or we need additional data.

| Loss over iterations |  Training and validation accuracy | 
|---|---|
|![loss][loss_2] | ![validation accuracy][validation_2] | 



### Approach 3 : Do pre processing and data augmentation


** I added augmented data in following way ** 
1. Rotate image from -20 to +20 degrees.
2. Translate image content by 5 pixels.
3. Shear images by 10 units.

![data augmentation][augmented_data]

** Data augmentation helped to have more examples for each class **
** Also I tried to make even distribution for each class ** 

Following is new data distribution 

![Augmented data distribution][training_distribution1] 


** Hyper parameters **  
Total input images : 159155; Batch size : 128 ;  Number of epochs : 10 ; Number of iterations per epoch : 1244 ;  Constant Learning rate : 0.001 ;

** Results for data augmentation and pre processing ** 
* Validation accuracy : more than 96.9% achieved
* The loss reduction was little bit sudden at start, this could be because of high learning rate
* The gap between training accuracy and validation accuracy got narrowed which is good sign and we can say that model is not over fitted.

| Loss over iterations |  Training and validation accuracy | 
|---|---|
|![loss][loss_3] | ![validation accuracy][validation_3] | 

### Approach 4 : Do pre processing, data augmentation and add drop out 

** In addition to approach 3, I Added drop outs after first 2 fully connected layers. **
** 50% random neurons dropped during training cycles ** 

** Hyper parameters **  
It were same as approach 3

** Results for data augmentation and pre processing and drop out** 
* Validation accuracy : more than 97.6% achieved
* The loss reduction was near to idea curve and it has zig zag pattern due to use of drop out
* The validation accuracy is more than training accuracy this is because of use of drop out during training.
* The validation accuracy after 6000 iterations is consistent with training accuracy which shows that there is no over fitting.

| Loss over iterations |  Training and validation accuracy | 
|---|---|
|![loss][loss_4] | ![validation accuracy][validation_4] | 

### Approach 5 : Do pre processing, data augmentation, add drop out and learning rate decay

** I applied learning rate decay to allow model to converge slowly at later stage, I increased learning rate by 10% and decayed learning it till 96% **
** Learning rate varies from 0.0011 at 1st epoch to 0.00096 at 15th epoch.  **

** Hyper parameters **  
Batch size : 128 ;  Number of epochs : 15 ; Number of iterations per epoch : 272 ;  Constant Learning rate : 0.001 ;

** Results for data augmentation and pre processing and drop out and learning rate decay** 
* Validation accuracy : near to 97.7% achieved. (After epoch 13 it was 98.1%)
* The loss reduction was near to idea curve and it has zig zag pattern due to use of drop out.
* The validation accuracy is more than training accuracy this is because of use of drop out during training.
* The validation accuracy after 6000 iterations is consistent with training accuracy which shows that there is no over fitting.


| Loss over iterations |  Training and validation accuracy | 
|---|---|
|![loss][loss_5] | ![validation accuracy][validation_5] | 

### Results on test data set 
** The accuracy obtained on 12630 test images is 95.5%

### Testing model on new images 
I down loaded following images from web and resized it to 32x32x3 
 
![new images][new_images] 

Following are top 5 predictions by model

|.|.|
|--|--|
|![top 5 probabilities][top_5_results1]|![top 5 probabilities][top_5_results2]|
|![top 5 probabilities][top_5_results3]|![top 5 probabilities][top_5_results4]|
|![top 5 probabilities][top_5_results5]|![top 5 probabilities][top_5_results6]|
|![top 5 probabilities][top_5_results7]||

### Analysis of prediction by model on new images
* 5 out of 7 images get classified correctly with distinct margin. 
* 1st image of 'Right-of-way at the next intersection' get correctly classified with possibility of almost 1.0
* 2nd image 'Keep right' : The correct class probability was around 0.12 and it was at third place. This image get classified as 'End of no passing by vehicles over 3.5 metric tons' Possible issues are : both sign types have circular shapes and the test image contains lot of white pixels and same was the case for 'End of no passing by vehicles over 3.5 metric tons' image 
* 3rd image 'Wild animals crossing' : The correct class probability was 0.05 which was at third place. This image get classified as 'bicycles crossing' Possible issue are : Both signs are rectangle in shape and have white background and black paint. The second best possibility was double curve and with blur eyes its content look similar to figure of a dear sign.
* 4th image 'Road work' get correctly classified with possibility of almost 1.0
* 5th image 'Speed limit (30 km/h)' got correctly classified with possibility of 0.7
* 6th image 'Stop sign' got correctly classified with possibility of almost 1.0
* 7th image 'Priority road' got correctly classified with possibility of almost 1.0
