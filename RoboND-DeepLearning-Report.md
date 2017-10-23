## Project: Preception
---
[//]: # (Image References)

[ConfusionMatix1]: ./images/ConfusionMatix1.JPG
[ObjectRecognision1]: ./images/ObjectRecognision1.JPG

[ConfusionMatix2]: ./images/ConfusionMatix2.JPG
[ObjectRecognision2]: ./images/ObjectRecognision2.JPG

[ConfusionMatix3]: ./images/ConfusionMatix3.JPG
[ObjectRecognision3]: ./images/ObjectRecognision3.JPG

#### Introduction
Goal of Follow Me project is to architect and train a deep learning model that will make a simulated quadcopter follow a specific person called hero.

There were number of techniques used to complete this project which is not limited to Fully Convolution Model, 1 by 1 convolutions, Schematic segmentation, skip connections and many more. Details and architecture and approach would be covered in this report. 

---


#### API’s used
Follow me project used open source Keras as its core API’s. This was used due to its performance efficiency and user-friendly API’s to architect deep learning network.

---


#### Approach
The model of the project was designed in a way identify a particular person in a crowd rather than mentioning a given sample is the person of interest.

Semantic Segmentation concepts were used to accomplish the above-mentioned task. Semantic Segmentation classify each pixel of the image to different classes there by grouping them. These classes would be used to identify the target image of interest.

Fully Convolution Network (FCN) was used to implement Sematic Segmentation technique. 

FCN uses multiple layers of encoder, decoder with 1 by 1 convolution layer in the middle.  This is done to derive progressive details about the image as the layer proceeds.

Finally, IoU Matrix was used to validate the performance of the Semantic segmentation exercise. This is used to calculate the performance of each pixel per class.

---


#### FCN Network Architecture
As mentioned about FCN comprises of three components.
1. Encoder
2. 1 by 1 convolution
3. Decoder

##### Encoder
Encoder has a series of convolution layer with increasing filter depth which is used to pre-train and extract details from the image

##### 1 by 1 convolution
1 by 1 convolution used instead of fully connected layer to preserve spatial information on passing through FCN. It preserves information in 4D instead of 2D as in fully connected layer

##### Decoder
Every Encoder convolution layer has a corresponding decoder layer to retrieves image size by using Transposed Convolution. 

This information is further used in segmentation of each pixel in the image.

---


#### FCN Implementation
My final implementation of the project had two layers of encoder and decoder used to enhance accuracy

Skip connections were used to combine encoding layers to that of corresponding decoding layers as denoted in the diagram. This was used to avoid losing of information in the process of encoding and decoding multiple layers. It uses information from multiple resolution there by preserving information.

---


#### Hardware
Initially the project was ran on my MacBook with 2.8GHz CPU speed. With my experience with Segmentation lab I was quite aware of the incapability of CPUs to handle FCN training. CPU was always hot not Jupyter notebook ran into errors.

Later I used EC2 instance as mentioned in the lab manual and was able to run multiple experiments in that setup.

I played around different batch sizes starting for 256, 128, 64, 32. 

---


#### Training Experiments
I ran many rans close to 7 trails with different setting and FCN layers on encoding. Note Decoding had same number of layers as of Encoding in each experiment.

Results of some of them as in the tabel below

Parameter                   | Trial 1 | Trial 2 | Trail 3| Trial 4 | Trial 5
---                         | ---     | ---     | ---    | ---     | 
Number of Encoding layers   | 2       | 2       | 3      | 3       | 2
learning rate               | 0.001   | 0.001   | 0.001  | 0.001   | 0.003
batch size                  | 256     | 128     | 64     | 32      | 32
num epochs                  | 100     | 100     | 100    | 100     | 100
steps per epoch             | 10      | 10      | 100    | 100     | 100
validation steps            | 50      | 50      | 50     | 50      | 50
workers                     | 2       | 2       | 5      | 5       | 5
Duration to compute(hours)  | 5       | 4       | 2.5    | 3       | 1.5
Final IoU score             | 0.34    | 0.36    | 0.35   | 0.37    | 0.405

I initially thought that increasing the number of layers can be beneficial however the IoU score degraded beyond 3 layers. The EC2 instance could handle 5 worker threads.

Increasing the learning rate from 0.001 to 0.003 also helped in improving the score 0.35 to 0.37.
Finally Increasing steps_per_epoch from 10 to 100 helped in attaining the required score after many experiments.


As show in the training curve graph, loss decreases with Epoch. 
# INSERT GRAPHS
Third iteration showed a high loss of loss: 0.0603 - val_loss: 0.0994

Fifty iteration showed loss of loss: 0.0198 - val_loss: 0.0282

Hundredth iteration showed loss: 0.0149 - val_loss: 0.0290

There are many scope for improvement as explained in the “Future Enhancement” session.

---


#### Evaluation
The training model is evaluated with respect to IoU matrix of various class. In this case its background, other people and hero.

The configuration in this training model shows and very good IoU score of 0.99 for background. However the IoU score of hero can be made better from 0.202. This could be due to high number of false negative.

Final score was evaluated to be 0.405

---


#### Future Enhancements
There are many more turning options that can be tried out for a better result. Here are some of my thoughts
1. Add 4 layers of encoder and decoder and just have skip connection for first and last pair.
2. Remove 1 by 1 convolution layer in-between
3. Modify batch size of 64, 128, 256 with 3 layers of encoding and decoding.

I would like submit the report and try these options in the mean while for a better score.
