# Dilated / Strided Sliding Window Attention

## Overview
Inspired by dilated convolutions in computer vision, **Dilated Sliding Window Attention** increases the attention field span without increasing the computational footprint.

## Mechanism
Instead of attending to contiguous keys, the sliding window skips adjacent keys at a regular, fixed periodic interval or dilation rate $d$. 
For a query token at index $i$, the model attends to key indices:
$$\{ i - k \times d \mid 0 \le k < W \}$$

## Advantages
- **Broader Context Window:** Multiplies the effective context window size by $d$ times.
- **Zero Additional Flops:** Keeps the absolute count of attention coordinate calculations identical to standard sliding window attention.

## Diagram

```mermaid
flowchart LR
    Q["Query Token i"] -->|attends (d=2)| K1["Key i-2"]
    Q -->|attends (d=2)| K2["Key i-4"]
    Q -->|attends (d=2)| K3["Key i-6"]
    Q -.->|skipped| S1["Key i-1"]
    Q -.->|skipped| S2["Key i-3"]
    Q -.->|skipped| S3["Key i-5"]
    
    style Q fill:#cce5ff,stroke:#004085,stroke-width:2px
    style K1 fill:#d4edda,stroke:#28a745
    style K2 fill:#d4edda,stroke:#28a745
    style K3 fill:#d4edda,stroke:#28a745
```

---
[← Back to README](../README.md)
