# **Byte-Pair Encoding**
---
Paper PDF: [https://arxiv.org/pdf/1508.07909](https://arxiv.org/pdf/1508.07909)

---

Byte-Pair Encoding (BPE) was introduced by [Sennrich et al.](https://arxiv.org/pdf/1508.07909) in 2016. The problem addressed in this paper relates to the limitations of traditional Neural Machine Translation (NMT) models when dealing with out-of-vocabulary (OOV) words, which can be challenging for agglutinative and compound words.

Imagine we have the following two words in a trained tokenizer:

| word~1~ | word~2~ |
|:--:|:--:|
| foot | ball |

If the word `football` appears in our dataset during the training process, the tokenizer would treat it as `<unk>` (unknown) tokens, as it does not exist in the vocabulary.

To address this issue, [Sennrich et al.](https://arxiv.org/pdf/1508.07909) proposed using byte-pair encoding (BPE). BPE allows vocabulary representation based on subwords with a fixed size. It replaces the most frequent byte pair with an unused byte.

Byte-pair encoding works as follows:

- Represent each word as a sequence of characters and add a special character at the end of each word.
- Count all character pairs.
- Replace the most frequent character pair with a new character (e.g., replace the pair ('C', 'B') with 'CB').
- Repeat iteratively until the desired vocabulary size (`vocab_size`) is reached.

```python
import re, collections

def get_stats(vocab):
    pairs = collections.defaultdict(int)
    for word, freq in vocab.items():
        symbols = word.split()
        for i in range(len(symbols) - 1):
            pairs[symbols[i], symbols[i+1]] += freq
    return pairs

def merge_vocab(pair, v_in):
    v_out = {}
    bigram = re.escape(' '.join(pair))
    p = re.compile(r'(?<!\S)' + bigram + r'(?!\S)')
    for word in v_in:
        w_out = p.sub(''.join(pair), word)
        v_out[w_out] = v_in[word]
    return v_out

vocab = {'l o w </w>': 5, 
         'l o w e r </w>': 2, 
         'n e w e s t </w>': 6,
         'w i d e s t </w>': 3}

num_merges = 10
for i in range(num_merges):
    pairs = get_stats(vocab)
    best = max(pairs, key=pairs.get)
    vocab = merge_vocab(best, vocab)
    print(best)
```

The output after performing bi-gram and `vocab_size = num_merges` 10,

1. `('e', 's')` &#8594; n e w {++es++} t </w>, w i d {++es++} t </w>
2. `('es', 't')` &#8594; n e w {++est++} </w>, w i d {++est++} </w> 
3. `('est', '</w>')` &#8594; n e w {++est</w>++}, w i d {++est</w>++}
4. `('l', 'o')` &#8594; {++lo++} w </w>, {++lo++} w e r </w>
5. `('lo', 'w')` &#8594; {++low++} </w>, {++low++} e r </w>
6. `('n', 'e')` &#8594;  {++ne++} w {++est</w>++}
7. `('ne', 'w')` &#8594; {++new++} {++est</w>++}
8. `('new', 'est</w>')` &#8594; {++newest</w>++}
9. `('low', '</w>')` &#8594; {++low</w>++}
10. `('w', 'i')` &#8594; {++wi++} d {++est</w>++}

[GPT-2](https://cdn.openai.com/better-language-models/language_models_are_unsupervised_multitask_learners.pdf) uses Unicode (e.g., UTF-8) rather than characters. First, the string is encoded with UTF-8, converting it to byte-level. This means each character (or symbol) is represented as one or more bytes, depending on the character's Unicode representation. The entire process then works the same as described above.
