# **Dot-Product Attention**
To the best of my knowledge, [Luong's](https://arxiv.org/abs/1508.04025) Dot-Product Attention or Multiplicative Attention is the first introduced Dot-Product Attention. Luong's Attention is a resambles of Bahdanau's Additive Attention. Luong's purposed 3 different alternatives alingment functions.

|FN [$a_t(h_t, \hat{h}_s$)]|Content|
|:--:|:--:|
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