## Episode 1  3–5 分钟档 约 **1200–1500 中文字、40–50 个镜**头

VLA 的 KV Cache 到底缓存了什么？以 OpenVLA 为例

1. VLA 为什么需要 multimodal tokens
2. OpenVLA 的 vision encoder → projector → LLM
3. vision / language / action token 是什么
4. Transformer 中 Q/K/V 是什么
5. autoregressive decoding
6. 为什么历史 K/V 可以缓存
7. KV cache 的 tensor structure
8. 为什么 cache size 随 context length 增长
9. VLA KV cache 与纯 LLM KV cache 的区别
10. 因此 如果这些 token 的重要性不同，我们还能把它们一视同仁吗？

Chapter 1
OpenVLA sees the world:
1.1 VLA 为什么需要 multimodal tokens
1.2 OpenVLA 的 vision encoder → projector → LLM

Chapter 2
From image patches to tokens:
vision / language / action token 是什么

Chapter 3
What does a Transformer actually cache?
3.1 Transformer 中 Q/K/V 是什么
3.2 autoregressive decoding

Chapter 4
Why K and V can be reused:
4.1 为什么历史 K/V 可以缓存
4.2 KV cache 的 tensor structure
4.3 为什么 cache size 随 context length 增长

Chapter 5
The multimodal KV cache:
5.1 VLA KV cache 与纯 LLM KV cache 的区别
5.2 raise the question 如果这些V/L/A的token 的重要性不同，我们还能把它们一视同仁吗？



-----

“不要用 metaphor 替代 mechanism” 
Metaphors may introduce a concept, but every metaphor must be followed by the actual computational mechanism.

例如：

> KV cache is like reusable memory.

下一秒：
$$
K_{past},V_{past}
$$
然后：
$$
Q_{new}K_{past}^{T}
$$
这样就不会变成“AI 科普频道味”。



最重要的一招：让每个镜头有“数学对象”

### Shot A

旁白：

> Suppose the input contains T tokens.

画面：

```
x₁  x₂  x₃  x₄  ...  xₜ
│   │   │   │        │
▼   ▼   ▼   ▼        ▼

Q₁  Q₂  Q₃  Q₄  ...  Qₜ
K₁  K₂  K₃  K₄  ...  Kₜ
V₁  V₂  V₃  V₄  ...  Vₜ
```

### Shot B

旁白：

> When generating the next token, its query attends to the previous keys and retrieves their values.

画面：
$$
q_{t+1}
$$
向左扫描：
$$
k_1,k_2,\ldots,k_t
$$
然后：
$$
\alpha_1,\alpha_2,\ldots,\alpha_t
$$
再加权：
$$
\alpha_1v_1+\cdots+\alpha_tv_t
$$
这就不是“科普动画”了。





Use anything2explainer in RESEARCH_LECTURE mode.

Create a ~5-minute Chinese technical lecture:

Title:
《VLA 的 KV Cache 到底缓存了什么？以 OpenVLA 为例》

Audience:
Graduate students with basic Transformer knowledge.

Goal:
Build the technical foundation required to understand VLA KV-cache compression.

Required concepts:

1. OpenVLA architecture
2. Vision tokens
3. Language tokens
4. Action tokens
5. Transformer input sequence
6. Q / K / V
7. Attention
8. Autoregressive decoding
9. KV cache
10. KV-cache tensor structure and dimensions
11. Why VLA KV cache differs from a conventional LLM KV cache

An initial chapter structure for your reference:

Chapter 1 — OpenVLA sees the world

1.1 Why does a VLA need multimodal tokens?
1.2 OpenVLA architecture:
    Vision Encoder → Projector → LLM
1.3 How vision, language, and action information
    enter a shared Transformer context

Chapter 2 — From image patches to tokens

2.1 What is a vision token?
2.2 What is a language token?
2.3 What is an action token?
2.4 How these heterogeneous tokens form the input sequence

Chapter 3 — What does a Transformer actually cache?

3.1 Q, K, and V
3.2 Self-attention:
    softmax(QKᵀ / √dₖ)V
3.3 Autoregressive decoding
3.4 What changes when generating the next token?

Chapter 4 — Why can K and V be reused?

4.1 Redundant computation during autoregressive decoding
4.2 Why previously computed K/V can be reused
4.3 What exactly is stored in the KV cache?
4.4 KV-cache tensor structure and dimensions
4.5 Why cache size grows with context length

Chapter 5 — The multimodal KV cache

5.1 How is a VLA KV cache different from a conventional LLM KV cache?
5.2 Vision, language, and action tokens coexist in the same cache
5.3 Why might different KV entries have different importance?

Raise a Research question:

“If these vision, language, and action tokens carry
different amounts of task-relevant information,
should we still allocate the same memory to all of them?”

Initial time allocation:
Chapter 1   ~45–50 sec 
Chapter 2   ~45–55 sec 
Chapter 3   ~65–75 sec 
Chapter 4   ~80–90 sec 
Chapter 5   ~45–55 sec 
Opening + ending ~20–30 sec

Explanation principles:

- Visual priority: mechanism > analogy.
- Do not oversimplify technical mechanisms for accessibility.
- Use equations where they materially improve understanding.
- Define every symbol before using it.
- When describing a computational operation, visualize the actual
  data flow or state transition rather than using a generic icon.
- Keep the explanation technically faithful to OpenVLA.
- Avoid explaining quantization, eviction, rate-distortion,
  or RDKV in detail. They should only be foreshadowed at the end.
- Maintain a coherent running example throughout the video.
- Target roughly 5 minutes rather than trying to maximize
  the number of concepts covered.

Core conceptual flow:

Image
→ Vision Encoder
→ Vision Tokens

Language
→ Language Tokens

Past actions
→ Action Tokens

Vision + Language + Action Tokens
→ Transformer
→ Q / K / V
→ Attention
→ Autoregressive decoding
→ KV Cache

Important:
Make clear exactly what is stored in the KV cache,
why it can be reused during autoregressive decoding,
and why the cache becomes a heterogeneous multimodal memory
in the VLA setting.

End by motivating the research question like:

“Why should heterogeneous vision/language/action KV entries
receive equal memory allocation?” or “If these tokens have different importance,
should we still treat them equally?”