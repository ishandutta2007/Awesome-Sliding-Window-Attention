# Standard Vanilla Sliding Window Attention

## Overview
The **Standard Vanilla Sliding Window Attention** is the most direct implementation of local context restriction. It serves as the baseline for sparse attention configurations.

## Mechanism
A query token $i$ only computes attention with key tokens that fall within a symmetric window of size $W$.
$$\text{Scope}(i) = \{j \mid i - \frac{W}{2} \le j \le i + \frac{W}{2}\}$$

## Receptive Field Expansion
Although a single layer is strictly local, stacking multiple layers causes the effective receptive field to expand linearly with network depth. For a model with $L$ layers and window size $W$, the effective receptive field at layer $L$ is:
$$\text{Receptive Field} = L \times W$$

This hierarchy allows high-level representations to capture long-range semantic dependencies without requiring quadratic attention computation.

## Diagram

```mermaid
graph TD
    subgraph Layer3["Layer 3 (Top)"]
        L3["Token i (Receptive Field: 3 * W)"]
    end
    subgraph Layer2["Layer 2 (Intermediate)"]
        L2_left["Token i-W"]
        L2_mid["Token i"]
        L2_right["Token i+W"]
    end
    subgraph Layer1["Layer 1 (Bottom)"]
        L1["Local Windows of width W"]
    end
    
    L1 --> L2_left
    L1 --> L2_mid
    L1 --> L2_right
    L2_left --> L3
    L2_mid --> L3
    L2_right --> L3
```

---
[← Back to README](../README.md)
