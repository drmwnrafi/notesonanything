# **SpinalNet: Deep Neural Network with Gradual Input**

---
Paper PDF : [https://arxiv.org/pdf/2007.03347.pdf](https://arxiv.org/pdf/2007.03347.pdf)

---

![](https://miro.medium.com/v2/resize:fit:456/format:webp/1*Z_YQ94rfYHqLeBvdDwJ4nw.jpeg){align=right width="170px"}

![](https://miro.medium.com/v2/resize:fit:532/format:webp/1*pk8DLMWF6YMycCjkkhCKxQ.jpeg){align=right width="200px"}

SpinalNet purposed to achieve higher accuracy than DNN with fewer computations. This network was developed by H M Dipu Kabir, Moloud Abdar, and Seyed Mohammad Jafar Jalali. The network structure consists of input sub-layers, intermediate sub-layers, and an output layer. 

Spinal Net is mimicking human **Samotosensory System** and **Spinal Cord**. Samotosensory system is the sensory system responsibile for detecting and processing information related to touch, pressure, temperature, and pain. Spinal Cord is the part of the body that receives all the senses from the rest of the body. Samostosensory Systems and Spinal Cord are closely related,as the spinal cord serves as gateway for somatosensory information to reach the brain.

Spinal Net aim to mimic the following characteristics of human spinal cord: 

1. Gradual input and nerve plexus
2. Voluntary and involuntary actions
3. Attention to pain intensity

The nerve plexus is a complex network for sensory neuron to reach spinal cord. It network sends all tactile signals to the spinal cord gradually. Sensory neurons may also convey information to the lower motor before getting instruction from the brain, its called involuntary or reflex movements.

SpinalNet implement 3 characteristics of human spinal cord as mentioned above : 

1. Gradual input
2. Local output and probable global influence
3. Weights reconfigured during training

Similar to our spinal cord, the proposed SpinalNet takes inputs gradually and repetitively. Each layer of the SpinalNet contributes towards the local output (reflex). The SpinalNet also sends a modulated version of inputs towards the global output (brain). The NN training process configures weights based on the training data. Spinal cord neurons are also get configured for tuning the pain sensitivity of different sensories of our body. 

Fig (b). demonstrates the structure of SpinalNet. The network structure consists of input sub-layers, intermediate sub-layers, and an output layer. The input is split and sent to the intermediate sub-layers of multiple hidden layers. 

In Fig. (b), the intermediate sub-layers contain two neurons per hidden layer. The number of intermediate neurons can be changed according to the user. However, both the number of intermediate neurons and the number of inputs per layer are regularly kept small to reduce the number of multiplication. As typically the number of inputs and the intermediate hidden neurons per layer allocates a small amount, the network may have an under-fit shape. 

As a consequence, each layer receives inputs from the previous layer. Since the input is repeated, if one important feature of input does not impact the output in one hidden layer, the feature may impact the output in another hidden layer. The intermediate sub-layers contain a nonlinear activation function and the output layer contains the linear activation function. In Fig. (b), the input values are split into three rows. These rows are assigned to the different hidden layers in a repeated manner.