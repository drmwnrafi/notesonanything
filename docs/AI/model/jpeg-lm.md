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

JPEG-LM treats image generation like "text generation". Since LLAMA-2 is an autoregressive model, the input should be in discrete tokens. 
There are many techniques to discretize image data (images are continuous data), such as VQ-VAE, ViT, ImageGPT, etc. 
This model converts images to bytes using JPEG and AVC/H.264 codecs for images and videos, respectively. 
You can see how JPEG codecs look by opening a .jpg file in a text editor or see [apendix](#excodec).

Since the output of JPEG codecs is in bytes, we can apply Byte-Pair Encoding (like encoding text in UTF-8, which also uses a byte representation). 
See [this blog post](../../tokenizer/bpe/) for more on BPE.

## **Training**
![pixel art](../media/ex.jpeg){ width="250" align=right}
JPEG-LM is pre-trained with LLAMA-2 7B from scratch. You can check my explanation about LLAMA-2 [here](../llama2/). <br>
Pre-training on LLAMA-2 is a good idea since JPEG-LM and LLAMA-2 differ in domains. 
As you can see in the [appendix jpeg codec](#excodec), the byte representation of the right PixelArt image. The JPEG codec is unreadable by humans and meaningless in terms of human language, unlike the purposes of LLaMA-2.<br>
For the training process, they use 256x256 pixels with a quality factor of 25 over 23 million images. 
They train BPE with 10k and set `vocab_size` to 320 with BPE. For efficiency, they concatenate all tokens and chunk them into `seq_len` 12k.
Then, training proceeds like a typical autoregressive model.<br>
&nbsp;

!!! quote "**My Thoughts**"
    I'm not entirely sure why JPEG-LM doesn't need positional encoding, but to the best of my knowledge, positional encoding in transformers-like models helps determine the meaning of the same tokens in different contexts, as word order is important. However, in JEPG codec, the order of tokens is inherently tied to the structure of the byte representation of an image. The byte sequence of a JPEG file includes specific markers, headers, and compressed data blocks, which organize the image content in a way that preserves spatial relationships. This allows the model to predict the next token without requiring explicit positional encoding.

    For example, the Discrete Cosine Transform (DCT) involves 64 basis images to determine the low and high frequencies of each patch. This converts the pixel domain to a frequency domain, which spatially decorrelates the image content. 
    This means the spatial relationships and frequencies in the image are embedded within the compression format itself, especially in the ordering of coefficients from the DCT blocks.

## **JPEG Codec**
JPEG stands for Joint Photographic Experts Group and is used to compress images with adjustable quality settings. JPEG-LM uses the JPEG codec to generate a string-basisd prompt, which is then tokenized with Byte Pair Encoding (BPE). An autoregressive model (a decode-only transformer) works only for discrete data, so since images are continuous data, they must first be discretized.

### **Color Space Transformation**

In this step, the RGB image is converted to YCbCr, where Y represents luminance, and Cb & Cr represent chrominance. Luminance captures brightness, while chrominance captures color information. To convert RGB to YCbCr, we use the following equations:

$$
\begin{align*}
Y &= 0.299 R + 0.587 G + 0.114 B \\
Cb &= - 0.1687 R - 0.3313 G + 0.5 B + 128 \\
Cr &= 0.5 R - 0.4187 G - 0.0813 B + 128
\end{align*}
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

| Before RLE | After RLE |
| ----------- | -------- |
| AAAABBBCCDAA | 4A3B2C1D2A |

The RLE version is further compressed using Huffman encoding. 

### **Huffman Encoding**

Huffman encoding compresses the RLE output by assigning shorter codes to more frequent symbols and longer codes to less frequent symbols.


## **Appendix**

### **JPEG Codecs**
<a id="excodec"></a>

| Original Bytes | UTF-8                          |
| ----------- | ------------------------------------ |
| FF D8 FF E0 00 10 4A 46 49 46 00 01 01 00 00 01 00 01 00 00 FF DB 00 43 00 08 06 06 07 06 05 08 | ����JFIF��C |
| 07 07 07 09 09 08 0A 0C 14 0D 0C 0B 0B 0C 19 12 13 0F 14 1D 1A 1F 1E 1D 1A 1C 1C 20 24 2E 27 20 |     $.' |
| 22 2C 23 1C 1C 28 37 29 2C 30 31 34 34 34 1F 27 39 3D 38 32 3C 2E 33 34 32 FF DB 00 43 01 09 09 | ",#(7),01444'9=82<.342��C |
| 09 0C 0B 0C 18 0D 0D 18 32 21 1C 21 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 |  2!!22222222222222222222 |
| 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 32 FF C0 |	222222222222222222222222222222�� |
| 00 11 08 00 10 00 10 03 01 22 00 02 11 01 03 11 01 FF C4 00 1F 00 00 01 05 01 01 01 01 01 01 00 | "�� |
| 00 00 00 00 00 00 00 01 02 03 04 05 06 07 08 09 0A 0B FF C4 00 B5 10 00 02 01 03 03 02 04 03 05 |  ��� |
| 05 04 04 00 00 01 7D 01 02 03 00 04 11 05 12 21 31 41 06 13 51 61 07 22 71 14 32 81 91 A1 08 23 | }!1AQa"q2���# |
| 42 B1 C1 15 52 D1 F0 24 33 62 72 82 09 0A 16 17 18 19 1A 25 26 27 28 29 2A 34 35 36 37 38 39 3A | B��R��$3br� %&'()*456789: | 
| 43 44 45 46 47 48 49 4A 53 54 55 56 57 58 59 5A 63 64 65 66 67 68 69 6A 73 74 75 76 77 78 79 7A | CDEFGHIJSTUVWXYZcdefghijstuvwxyz |
| 83 84 85 86 87 88 89 8A 92 93 94 95 96 97 98 99 9A A2 A3 A4 A5 A6 A7 A8 A9 AA B2 B3 B4 B5 B6 B7 | �������������������������������� |
| B8 B9 BA C2 C3 C4 C5 C6 C7 C8 C9 CA D2 D3 D4 D5 D6 D7 D8 D9 DA E1 E2 E3 E4 E5 E6 E7 E8 E9 EA F1 | �������������������������������� |
| F2 F3 F4 F5 F6 F7 F8 F9 FA FF C4 00 1F 01 00 03 01 01 01 01 01 01 01 01 01 00 00 00 00 00 00 01 | ����������� |
| 02 03 04 05 06 07 08 09 0A 0B FF C4 00 B5 11 00 02 01 02 04 04 03 04 07 05 04 04 00 01 02 77 00 |  ���w |
| 01 02 03 11 04 05 21 31 06 12 41 51 07 61 71 13 22 32 81 08 14 42 91 A1 B1 C1 09 23 33 52 F0 15 | !1AQaq"2�B���� #3R� |
| 62 72 D1 0A 16 24 34 E1 25 F1 17 18 19 1A 26 27 28 29 2A 35 36 37 38 39 3A 43 44 45 46 47 48 49 | br� $4�%�&'()*56789:CDEFGHI |
| 4A 53 54 55 56 57 58 59 5A 63 64 65 66 67 68 69 6A 73 74 75 76 77 78 79 7A 82 83 84 85 86 87 88 | JSTUVWXYZcdefghijstuvwxyz������� |
| 89 8A 92 93 94 95 96 97 98 99 9A A2 A3 A4 A5 A6 A7 A8 A9 AA B2 B3 B4 B5 B6 B7 B8 B9 BA C2 C3 C4 | �������������������������������� |
| C5 C6 C7 C8 C9 CA D2 D3 D4 D5 D6 D7 D8 D9 DA E2 E3 E4 E5 E6 E7 E8 E9 EA F2 F3 F4 F5 F6 F7 F8 F9 | �������������������������������� |
| FA FF DA 00 0C 03 01 00 02 11 03 11 00 3F 00 C5 F8 73 6B A5 78 7F C0 67 C4 D7 96 AF 7D 25 CD D9 | ���?��sk�x�g�ז�}%�� |
| 85 C5 90 F3 24 89 06 02 AC 99 23 68 0C 0B 10 3A 86 8C 9C F1 83 E2 35 AE 95 E2 0F 01 8F 13 59 DA | �Ő�$���#h:�����5����Y� | 
| BD 8C 96 D7 62 14 17 A3 CB 92 54 39 0C B1 E0 9D C0 B1 0C 01 E8 16 42 31 CE 5D F0 7F 50 D3 4E 8F | ����b�˒T9������B1�]�P�N� |
| 7B 6C B2 C7 61 75 6D 89 6E 64 6C 7E F9 49 6F DE B1 27 21 10 7C A7 3F 2A E4 1C E5 CD 1F 18 35 0D | {l��aum�ndl~�Ioޱ'!|�?*���5 | 
| 34 68 F6 56 CD 2C 77 F7 57 39 96 DA 45 C7 EE 54 15 FD EA 90 72 51 C7 CA 31 F2 B6 09 CE 50 54 5A | 4h�V�,w�W9��E��T��rQ��1� �PTZ |
| 17 BD B5 3C DB FF 00 B5 5F 93 E7 75 FE 7F 81 FF D9 | ��<���_��u���� |