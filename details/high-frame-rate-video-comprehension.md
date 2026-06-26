# High-Frame-Rate Video Comprehension

## Overview
High-FPS video processing exhibits massive temporal redundancy. Applying full global self-attention across video frames is computationally impossible. Temporal sliding window attention focuses on temporal neighborhoods, reducing the workload.

## Key Implementation
- **Video Swin Transformer:** Employs local spatiotemporal window attention to extract short-term temporal shifts, capturing motion without cross-frame bottlenecks.

## Diagram

```mermaid
flowchart LR
    F1["Frame t-1"] -->|Temporal Local Attention| Model["Video Swin Transformer"]
    F2["Frame t"] -->|Temporal Local Attention| Model
    F3["Frame t+1"] -->|Temporal Local Attention| Model
    Model --> Classification["Action Recognition"]
```

---
[← Back to README](../README.md)
