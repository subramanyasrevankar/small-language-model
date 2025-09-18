 🧠 Small Language Model (Toy Transformer)
📖 Description

This is a small language model that I built by watching tutorials. The main purpose is to understand how transformers like GPT actually work from the inside.

I learned about embeddings, attention, transformer blocks, and even gradient accumulation. This project is not about making a huge model, but more about understanding the concepts step by step.

🔑 How It Works (What I Understood)
1. Embedding Layer

First, every token (word) is converted into a high-dimensional vector.

The vector size is 768 (embedding vector).

2. Masked Multi-Head Attention
flowchart TD
  A[Input Tokens] --> B[Embedding Vector (768)]
  B --> C[Query (4×768)]
  B --> D[Key (4×768)]
  B --> E[Value (4×768)]
  C --> F[Q × Kᵀ = Attention Score (4×4)]
  D --> F
  F --> G[Softmax + Scaling]
  G --> H[Attention Weights]
  H --> I[Multiply with Values]
  I --> J[Context Vector (4×768)]


Causal Masking: the upper triangle (above diagonal) is masked as zero so the model can’t see the future.

3. Transformer Block
flowchart LR
  A[Input] --> B[LayerNorm 1]
  B --> C[Multi-Head Attention]
  C --> D[Dropout]
  D --> E[Residual Connection +]
  E --> F[LayerNorm 2]
  F --> G[Feed Forward NN]
  G --> H[Dropout]
  H --> I[Output (still 768)]


We can stack 12+ transformer blocks, but the shape remains 768.

4. Output

Final neural network outputs shape = batch_size × sequence_length × vocab_size.

Example: 4 × 50257 (4 tokens, vocab size 50,257).

Each word gets matched with the vocabulary token ID.

Output is a logits matrix → (4×50257).

5. Gradient Accumulation
flowchart TD
  A[Want Batch Size = 1024] --> B[GPU Only Fits 32]
  B --> C[Run 32 Iterations]
  C --> D[Accumulate Gradients]
  D --> E[Update Parameters]
  E --> F[Simulated Large Batch Training]


Instead of waiting until 1024 samples, we update after every 32 steps, simulating the larger batch size.

📝 Sample Outputs

Here are some example generations from the model:

Input Prompt:

Once upon a time there was a pumpkin.


Model Output:

Once upon a time there was a pumpkin. It was very special. 
The pumpkin wanted to paint with its family. 
So one day, a family decided...

2 laser soldiers traveled and drove it grow into the sky. laughed like the whole.

The pumpkin stopped and smiled and felt happy for the tube. It sounded like it...


Input Prompt:

A little girl went to the woods


Model Output:

A little girl went to the woods and he was looking at the animals 
and he saw a little boy with a big smile on its face. 
He knew she...

One day, the girl called Jeff went for a walk. 
He saw something pretty in a nearby old structure. 
It was a kind of creature...

✅ What I Learned

Converting tokens to embeddings (768).

How attention scores are calculated and turned into context vectors.

Why causal masking is needed.

The flow of a transformer block.

How gradient accumulation helps when batch size is too large.

How a small transformer can generate simple text.
