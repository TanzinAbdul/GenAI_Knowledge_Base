# Large Language Models (LLMs) & Autoregressive Transformers

A comprehensive deep dive into the philosophy, scalable multi-head attention mechanics, and mathematical foundations driving modern autoregressive Large Language Models.

---

## 🧭 Theoretical Foundations & History

### The Bottleneck of Recurrence (Pre-2017)
Before the modern LLM era, sequential natural language data was processed using **Recurrent Neural Networks (RNNs)**, **Long Short-Term Memory (LSTM)** networks, or **Gated Recurrent Units (GRUs)**. 

These architectures suffered from two catastrophic bottlenecks:
1. **Sequential Computation:** An RNN must process token $t_1$ before it can process token $t_2$. This sequential nature made it impossible to parallelize training across massive modern GPU clusters.
2. **Vanishing Gradients & Context Decay:** Despite gating mechanisms, LSTMs struggled to maintain long-range dependencies. Information from thousands of tokens prior would gradually wash away.

### The Attention Revolution (2017)
In 2017, Vaswani et al. published the historic paper, *["Attention Is All You Need"](https://arxiv.org/abs/1706.03762)*, introducing the **Transformer** architecture. 

The Transformer completely discarded recurrence. Instead, it relied entirely on **Self-Attention mechanisms**, allowing the network to look at every token in a sequence simultaneously. This unlocked massive parallelization and gave birth to modern scaling laws, eventually leading to massive autoregressive models like GPT-4, Llama, and Claude.

```text
RNN Structure (Sequential):      Token 1 ──> [Cell] ──> Token 2 ──> [Cell] ──> Token 3
Transformer (Parallel):          [Token 1, Token 2, Token 3] ───> [Multi-Head Attention] (All cross-attend simultaneously)
