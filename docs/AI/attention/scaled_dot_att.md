# **Scaled Dot-Product Attention**
This method comes from a "Kang" paper, [Attention is All You Need](https://arxiv.org/pdf/1706.03762) by Vaswani et al. 

We came at the most famous equation in last 5 years on DL fields.

$$
\text{Attention}(Q,K,V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V
$$

Basically, Scaled Dot-Product Attention based on Dot-Product Attention but scaled with size embbeding $d_k$, $\frac{1}{\sqrt{d_k}}$. 

Query, key, value, and output are all vectors but applied a basic Linear Layers to pack it as matrix Q, K, V, respectively. Dot-product attention compute more faster and space efficient.

Scaled Dot-Product and Dot-Product have similar performance at small embedding $d_k$, while Additive Attention perform better.

```py
def ScaleDotProduct(Q:torch.Tensor, K:torch.Tensor, V:torch.Tensor, mask:torch.BoolTensor=None):
    """
    PDF Link : https://arxiv.org/abs/1706.03762
    """
    scores = torch.matmul(Q, K.transpose(-2, -1)) / (Q.size(-1)**0.5)
    if mask != None:
        scores = scores.masked_fill(mask.logical_not(), -torch.inf)
    scores = F.softmax(scores, dim=-1)
    scores = torch.matmul(scores, V)
    return scores
```