# Asymmetric Causal Sliding Window Attention

## Overview
In autoregressive text generation, future tokens must be masked to prevent the model from looking ahead. **Asymmetric Causal Sliding Window Attention** adapts local attention to this causal constraint.

## Mechanism
The sliding window opens exclusively backward into historical tokens. A query token at index $i$ only attends to keys in the range:
$$\left[ i - W, i \right]$$

All future tokens ($j > i$) are masked out.

## Key Applications
- **Autoregressive Decoders:** Foundational to modern LLMs (e.g., LLaMA, Mistral, GPT models) to enable efficient next-token prediction.

## Diagram

```mermaid
flowchart RL
    subgraph Context["Attention Window (Width W)"]
        Hist2["Token i-2"]
        Hist1["Token i-1"]
        Curr["Token i (Query)"]
    end
    subgraph Future["Masked (Future)"]
        F1["Token i+1"]
        F2["Token i+2"]
    end
    
    Curr -->|Attends| Hist1
    Curr -->|Attends| Hist2
    Curr -.->|Blocked| F1
    Curr -.->|Blocked| F2
    
    style Context fill:#d4edda,stroke:#28a745,stroke-width:2px
    style Future fill:#f8d7da,stroke:#dc3545,stroke-width:1px,stroke-dasharray: 5 5
```

---
[← Back to README](../README.md)
