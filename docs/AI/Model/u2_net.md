# **U^2^-NET**
---
Paper PDF : [https://arxiv.org/pdf/2005.09007.pdf](https://arxiv.org/pdf/2005.09007.pdf)

---

U^2^-Net or U2-Net or U-shaped Net is a U-Net like architecture. U2-Net is a two-level nested U-structure architecture that is designed for salient object detection (SOD). The architecture allows the network to go deeper, attain high resolution, without significantly increasing the memory and computation cost. This is achieved by a nested U-structure: on the bottom level, with a novel ReSidual U-block (RSU) module, which is able to extract intra-stage multi-scale features without degrading the feature map resolution; on the top level, there is a U-Net like structure, in which each stage is filled by a RSU block 

<img style="float:right; margin:5px; padding:5px; max-height:350px" src="https://i.pinimg.com/564x/99/55/a9/9955a9a368148d72f6417f93a93d0b4f.jpg">

## **Residual U-Block (RSU-L)**
RSU-L used to capture intra-stage multi-scale features. The structure of RSU-L <code>(C~in~, M, C~out~)</code> is shown in Fig. 1, where `L` is the number of layers in the encoder, <code>C~in~, C~out~</code>denote input and output channels, and `M` denotes the number of channels in the internal layers of RSU.

Three components of RSU-L :
1. Input convolutional layers, this step convert a feature map <code>(H × W × C~in~)</code> to intermediate map, this is a plain convolutional layer for local feature extraction. This step consists of convolutional layer with 3x3 filter size, Batch Normalization, and ReLU activation.

2. Encoder (downsampling) - Decoder (upsampling) U-Net like. In this step, the input is the output of step one and learns to extract and encode the multi-scale contextual information. The `L` letter in RSU-L refers to the depth of architecture (shown in Fig. 2 [L = 7, it's mean RSU-7]).  Larger `L` leads to deeper residual U-block (RSU), more pooling operations, larger range of receptive fields and richer local and global features. The output of downsampling at each level will be saved for skip connections, and concatenate in decoder for the better output. Same as downsampling, the decoder use progressive upsampling, to avoid loss of detail caused by direct upsampling with large scales.

3. The Add layers fuse local features (output of step one) and the multi-scale features (output of step two) to produce the output.


## **U^2^-Net Architecture**

<img style="float:right; margin:0px; padding:0px; max-height:350px" src="https://i.pinimg.com/564x/c2/f2/28/c2f228d178cd2b988b76ecc21321825a.jpg">

The U2-Net architecture is like U-Net but consists of Residual U-Block (RSU-L) at each stage, while U-Net use convolutional block. U^2^-Net is a nested U-structured with U^n^-Net formulation (n = 2). U2-Net mainly consists of three parts : a six stages encoder (some say last of them is a bridge), a five stages decoder, and saliency map fusion module attached with the decoder stages and the bridge stage.

`I : Input Channels`

`M : Intermediate Channels`

`O : Output Channels`

<img src="https://i.pinimg.com/564x/82/21/29/82212988f5ad8682eb049e6825178187.jpg" alt="U2-Net architecture" style="display: block; margin: auto;">

In picture above, U2-Net has 2 different values of `I, M, and O`,  but the structure of the RSU-L stage remains consistent throughout the network. `RSU-7, RSU-6, RSU-5, RSU-4, RSU-4F` are the encoders and types of RSU-L blocks used with different input parameters. Than, the 2 others `RSU-4F` are bridge and first decoder, the last three RSU-L `RSU-4, RSU-5, RSU-6, RSU-7` serve as decoders blocks. The last part, all of the outputs from bridge and decoders are concatenated to generate the final output using `sigmoid` activation function to classify.