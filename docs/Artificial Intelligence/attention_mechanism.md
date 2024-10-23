# Attentions Mechanism
Attentions mechanism is a pivot of current deep learning models. Its allows the model to capture the relationships of input sequences. 
> Desclimer : The snippet code is only a attention code, not the whole Encoder-Decoder code.  
## Additive-Attention
As far as i know, [Bahdanau et. al](https://arxiv.org/pdf/1409.0473) is the first who introduce "Attention Mechanism".
Neural Machine Translation (NMT) at that time based on RNN variants with fixed-length vector context is the background. It cann't translated "long sequences" token due to difficulties capturing the relationship of preciding tokens. So, they invented an dynamics vector context. 

The encoder of Additive Attention based on Bidirectional RNN (BiRNN) to capture the relationships of preceding and following tokens. Simply, BiRNN is an forward (from $x_{start}$ to $x_{end}$) and backward RNN (from $x_{end}$ to $x_{start}$) (donated by bottom part in Fig1), where $x$ is the input sequences. From forward and backward, RNN produced the hidden states $h_f$ and $h_b$ of each input sequences and concatetating both hidden states $h = \left[ h_f ; h_b\right]$. 
This annotation $h$ contains the summaries of preciding and following token due to concat the $h_f$ and $h_b$.

> In terms of Transformers model, we can say hidden state of encoder ($h$) as query and hidden state of decoder ($s$) as key. 

The decoder fed annotations ($h_i$) and previous hidden states ($s_{t-i}$) to *alignment model* (attention scores) :

$$
e_{t, i} = a(s_{t-1}, h_i)
$$

where $a(\cdot)$ is

$$
a(s_{t-1}, h_i) = \text{v}^T \tanh(\text{W}_h h_i + \text{W}_s s_{t-1})
$$

$a$ can be parameterized by using FNN. The attention scores tell how good the input and the output matches. To get probability of attention scores, it applied to softmax.

$$
\alpha_{t,i} = \text{softmax}(e_{t,i})
$$

Then, applied a weighted sum of all source token to get context vector $c_i$:

$$
c_i = \sum_{i=start}^{end} \alpha_{t,i}h_i 
$$

This context vector $c_i$ guides the generation of the predicted token by summarizing information from the encoder's hidden states ($h_j$), weighted by attention probabilities that are computed based on the previous hidden state of the decoder ($s_{t−1}$). Generate a prediction ($y$) by using hidden state for time-i ($s_i$), last output ($y_{t-1}$), and context vector ($c_i$).

$$
y_i = \text{Decoder}(y_{t-1}, s_{i-1}, c_i)
$$

We can use RNN or RNN-like architeture to generate next token as Decoder.
> The `+` operator makes this algorithm named by *additive*.

```py
class AdditiveAttention(nn.Module):
    """
    PDF Link : https://arxiv.org/pdf/1409.0473
    """
    def __init__(self, embedding_size):
        super(AdditiveAttention, self).__init__()
        self.Wh = nn.Linear(embedding_size, embedding_size)
        self.Ws = nn.Linear(embedding_size, embedding_size)
        self.v = nn.Linear(embedding_size, 1)

    def forward(self, annotations, hidden_state):
        hidden_state = hidden_state.unsqueeze(1) # batch, 1, embedding_size
        scores = self.v(self.Wh(annotations) + self.Ws(hidden_state)) # batch, seq_len, 1
        weights = F.softmax(scores, dim=-1) # batch, seq_len, 1
        context = torch.sum(weights * annotations, dim=1) # batch, embedding_size
        return context
```
## Dot-Product Attention
To the best of my knowledge, [Luong's](https://arxiv.org/abs/1508.04025) Dot-Product Attention or Multiplicative Attention is the first introduced Dot-Product Attention. Luong's Attention is a resambles of Bahdanau's Additive Attention. Luong's purposed 3 different alternatives alingment functions.

|FN [$a_t(h_t, \hat{h}_s$)]|Content|
|--|--|
|$h_t^T \cdot \hat{h}s$|dot|
|$h_t^T \cdot \textbf{W}_a\hat{h}_s$|general|
|$v_a^T\tanh(\textbf{W}_a[h_t;\hat{h}_s])$|concat|

We can see it compare to Bandahau's Attention, Luong's Attention doesn't use the last decoder state ($s_{i-1}$) and lesser learnable parameter, it means Luong's Attention require less computation. As Luong et. al stated on their paper, their's version of Attention doesn't require a Bidirectional RNN.

```py
class MultiplicativeAttention(nn.Module):
    """
    PDF Link : https://arxiv.org/abs/1508.04025
    """
    def __init__(self, embedding_size):
        super(MultiplicativeAttention, self).__init__()
        self.Wa = nn.Linear(embedding_size, embedding_size)

    def forward(self, annotations, hidden_state):
        hidden_state = hidden_state.unsqueeze(1) # batch, 1, embedding_size
        scores =  self.Wa(annotations) @ torch.transpose(hidden_state, -2,-1) # batch, seq_len, 1
        weights = F.softmax(scores, dim=-1) # batch, seq_len, 1
        context = torch.sum(weights * annotations, dim=1) # batch, embedding_size
        return context
```

## Scaled Dot-Product Attention
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

## Multi-Head Attention (MHA)

## Mult-Query Attention (MQA)

```py
class MultiQueryAttention(nn.Module):
    """
    PDF Link : https://arxiv.org/pdf/1911.02150
    """
    def __init__(self, embedding_size:int, n_heads:int):
        super(MultiQueryAttention, self).__init__()
        assert embedding_size % n_heads == 0, (
            f"Embedding size ({embedding_size}) is not divisible by query heads ({n_heads})."
        )
        self.n_heads = n_heads
        self.head_dim = embedding_size // n_heads
        self.q_proj = nn.ModuleList([nn.Linear(embedding_size, self.head_dim) for _ in range(n_heads)])
        self.k_proj = nn.Linear(embedding_size, self.head_dim)
        self.v_proj = nn.Linear(embedding_size, self.head_dim)
        self.out_proj = nn.Linear(embedding_size, embedding_size)

    def forward(self, x:torch.Tensor, mask:torch.BoolTensor=None):
        batch, seq_len, _ = x.size()
        K = self.k_proj(x).view(batch, seq_len, 1, self.head_dim)
        V = self.v_proj(x).view(batch, seq_len, 1, self.head_dim)
        output = []
        for i in range(self.n_heads):
            Q = self.q_proj[i](x).view(batch, seq_len, 1, self.head_dim)
            output.append(ScaleDotProduct(Q, K, V, mask))
        output = torch.cat(output, dim=-1).contiguous().view(batch, seq_len, -1)
        return self.out_proj(output)
```

## Group-Query Attention (GQA)

[Group-Query Attention](https://arxiv.org/pdf/2305.13245) is a combination of multi head and multi query attention with one key and value heads of each sub-groups of query.

- **Multi-Head Attention (MHA)** : Every queries head have unique keys and values. Require big memory bandwidth, make it slow when inference. 
- **Multi-Query Attention (MQA)** : Every queries heads have single keys and values. Less memory bandwidth compare to MHA, but the quality become less and lead instability while training.
- **Grouped Query Attention (GQA)** : Queries divides to *G* groups. Each group have unique keys and values. Less memory bandwidth than MHA, and better quality than MQA

```py
class GroupedQueryAttention(nn.Module):
    """
    PDF Link : https://arxiv.org/pdf/2305.13245
    """
    def __init__(self, embedding_size, n_heads, n_kv_heads):
        super(GroupedQueryAttention, self).__init__()
        assert n_heads % n_kv_heads == 0, (
            f"Query heads ({n_heads}) is not divisible by key and value heads ({n_kv_heads}), "
            f"which will result in a non-integer number of groups."
        )

        self.embedding_size = embedding_size
        self.n_q_heads = n_heads
        self.n_kv_heads = n_kv_heads
        self.n_groups = self.n_q_heads // n_kv_heads
        self.head_dim = embedding_size // n_heads

        self.groups = nn.ModuleList([
            MultiQueryAttention(embedding_size, self.n_groups) for _ in range(n_kv_heads)
        ])
        self.out_proj = nn.Linear(embedding_size*n_kv_heads, embedding_size)

    def forward(self, x:torch.Tensor, mask:torch.BoolTensor=None):
        batch, seq_len, _ = x.size() 
        output = torch.cat([mqa(x, mask) for mqa in self.groups], dim=-1)
        output = output.contiguous().view(batch, seq_len, -1)
        return self.out_proj(output)
```
## Flash Attention

## Flash Attention-2
