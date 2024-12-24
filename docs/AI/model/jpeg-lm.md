# **JPEG-LM**

-------
**Paper PDF**: [https://arxiv.org/pdf/2408.08459](https://arxiv.org/pdf/2408.08459)

-------

I know this model from [Hu-Po's stream](https://www.youtube.com/watch?v=SkvyrgSzigo&t=3s) (shout out to him, you have to check out his YouTube channel). JPEG-LM is an autoregressive LLM (a conventional LLM) designed for image generation. What interests me about this paper is its claim that the model does not require convolutional layers or positional embeddings.

!!! quote 
    We use conventional LLM architectures (autoregressive transformers) without any vision-specific modifications (no convolutions, no 2D positional embeddings) to maximize the models’ generality. - [X. Han, et.al (2024)](https://arxiv.org/pdf/2408.08459)

## **Pre-processing**
!!! info
    For this blog, I only discuss image generation (the JPEG-LM paper also covers video generation). 

JPEG-LM treats image generation like "text generation." Since LLAMA-2 is an autoregressive model, the input should be in discrete tokens. There are many techniques to discretize image data (images are continuous data), such as VQ-VAE, ViT, ImageGPT, etc. This model converts images to bytes using JPEG and AVC/H.264 codecs for images and videos, respectively. You can see how JPEG codecs look by opening a .jpg file in a text editor.

Since the output of JPEG is in bytes, we can apply Byte-Pair Encoding (like encoding text in UTF-8, which also uses a byte representation). See [this blog post](../../tokenizer/bpe/) for more on BPE.

## **Training**
JPEG-LM is pre-trained with LLAMA-2 7B from scratch. You can check my explanation about LLAMA-2 [here](../llama2/).

Pre-training on LLAMA-2 is a good idea since JPEG-LM and LLAMA-2 differ in domains. As you can see in the image above, the JPEG codec is unreadable by humans and meaningless in terms of human language.

For the training process, they use 256x256 pixels with a quality factor of 25 over 23 million images. They train BPE with 10k and set `vocab_size` to 320 with BPE. For efficiency, they concatenate all tokens and chunk them into `seq_len` 12k.

Then, training proceeds like a typical autoregressive model.

## **JPEG Codec**
JPEG stands for Joint Photographic Experts Group and is used to compress images with adjustable quality settings. JPEG-LM uses the JPEG codec to generate a string-basisd prompt, which is then tokenized with Byte Pair Encoding (BPE). An autoregressive model (a decode-only transformer) works only for discrete data, so since images are continuous data, they must first be discretized.

### **Color Space Transformation**

In this step, the RGB image is converted to YCbCr, where Y represents luminance, and Cb & Cr represent chrominance. Luminance captures brightness, while chrominance captures color information. To convert RGB to YCbCr, we use the following equations:

$$
Y = 0.299 R + 0.587 G + 0.114 B \\
Cb = - 0.1687 R - 0.3313 G + 0.5 B + 128 \\
Cr = 0.5 R - 0.4187 G - 0.0813 B + 128
$$

Human eyes are more sensitive to brightness (luminance) than color (chrominance). Therefore, we can downsample chrominance by some factor without significant loss in visual quality.

### **Discrete Cosine Transform**

Before applying DCT, the original YCbCr image is divided into 8x8 blocks. Each block's pixel values are then shifted from a positive range to one centered on zero (from [0, 255] to [-128, 127]). After that, DCT is applied to each block using the following equation:

$$
DCT_{u,v} = \frac{1}{4} \text{S}(u) \text{S}(v) \sum_{x=0}^7 \sum_{y=0}^7 B_{x,y} \cos\left[\frac{(2x+1)u\pi}{16}\right] \cos\left[\frac{(2y+1)v\pi}{16}\right] 
$$

Here, $DCT_{u,v}$ is the DCT coefficient at coordinates (u, v); u represents horizontal counts within a block, and v represents vertical counts. $B_{x,y}$ is the pixel value at coordinate (x, y). $\text{S}$ is a normalizing scale factor, calculated by:

$$
S(z) = 
\begin{cases} 
\frac{1}{\sqrt{2}} & \text{if } z = 0 \\ 
1 & \text{otherwise} 
\end{cases}
$$

Each coordinate determines how much each of the 64 basis cosine functions influences the block. The Discrete Cosine Transform (DCT) converts spatial-domain data to frequency-domain data, helping to determine which pixels can be removed.

<img src ="../../media/dct.png" width = 350px style="display: block; margin: auto;">
<div align="center">64 basis cosine patterns</div>

These 64 basis cosines are the same for all images.

### **Quantization**

$$
Q_{Y} =
\begin{bmatrix}
16 & 11 & 10 & 16 & 24 & 40 & 51 & 61 \\
12 & 12 & 14 & 19 & 26 & 58 & 60 & 55 \\
14 & 13 & 16 & 24 & 40 & 57 & 69 & 56 \\
14 & 17 & 22 & 29 & 51 & 87 & 80 & 62 \\
18 & 22 & 37 & 56 & 68 & 109 & 103 & 77 \\
24 & 35 & 55 & 64 & 81 & 104 & 113 & 92 \\
49 & 64 & 78 & 87 & 103 & 121 & 120 & 101 \\
72 & 92 & 95 & 98 & 112 & 100 & 103 & 99
\end{bmatrix}
$$

Quantization introduces most of the information loss in this process. To quantize the DCT coefficients, we use the following equation:

$$
\text{Quant}_{x,y} = \text{round}\left(\frac{\text{DCT}_{x,y}}{Q_{x,y}}\right)
$$

### **Listing**

Each quantized 8x8 block is organized in a zigzag pattern, followed by run-length encoding and then Huffman encoding for further compression.

<img src="../../media/zz_list.png" width = 350px style="display: block; margin: auto;">

### **Run-Length Encoding**

Run-length encoding (RLE) simplifies redundant values in the quantized block to reduce memory usage. For example,

$$
\text{Before RLE: AAAABBBCCDAA}\\
\text{After RLE: 4A3B2C1D2A}
$$

The RLE version is further compressed using Huffman encoding. 

### **Huffman Encoding**

Huffman encoding compresses the RLE output by assigning shorter codes to more frequent symbols and longer codes to less frequent symbols.

!!! quote "**My Thoughts**"
    I’m not entirely sure why JPEG-LM doesn't need positional encoding, but to the best of my knowledge, the Discrete Cosine Transform (DCT) involves 64 basis images to determine the low and high frequencies of each patch. This converts the pixel domain to a frequency domain, which spatially decorrelates the image content. This means the spatial relationships and frequencies in the image are embedded within the compression format itself, especially in the ordering of coefficients from the DCT blocks. 