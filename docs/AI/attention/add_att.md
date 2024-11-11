# **Additive-Attention**
As far as i know, [Bahdanau et. al](https://arxiv.org/pdf/1409.0473) is the first who introduce "Attention Mechanism".
Neural Machine Translation (NMT) at that time based on RNN variants with fixed-length vector context is the background. It cann't translated "long sequences" token due to difficulties capturing the relationship of preciding tokens. So, they invented an dynamics vector context. 

The encoder of Additive Attention based on Bidirectional RNN (BiRNN) to capture the relationships of preceding and following tokens. Simply, BiRNN is an forward (from $x_{start}$ to $x_{end}$) and backward RNN (from $x_{end}$ to $x_{start}$) (donated by bottom part in Fig1), where $x$ is the input sequences. From forward and backward, RNN produced the hidden states $h_f$ and $h_b$ of each input sequences and concatetating both hidden states $h = \left[ h_f ; h_b\right]$. 
This annotation $h$ contains the summaries of preciding and following token due to concat the $h_f$ and $h_b$.

!!! info
    In terms of Transformers model, we can say hidden state of encoder ($h$) as query and hidden state of decoder ($s$) as key. 

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
!!! info
    The `+` operator makes this algorithm named by *additive*.

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