# **Group-Query Attention (GQA)**

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