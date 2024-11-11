# **Multi-Query Attention (MQA)**

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