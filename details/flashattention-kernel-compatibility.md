# The FlashAttention Kernel Compatibility Gap

## The Problem
Standard sliding window masks introduce non-contiguous memory reading indexing. While mathematically simple, implementing arbitrary masking in GPUs causes non-coalesced memory fetches, which nullifies the speedups of FlashAttention.

## Mitigation
**Block-Local FlashAttention Kernels** (used in vLLM, Hugging Face TGI) group the attention operation into coarse, contiguous $64 \times 64$ blocks. The sliding window constraint is applied on these block tiles rather than individual tokens, preserving GPU SRAM tiling speed.

## Diagram

```mermaid
flowchart TD
    subgraph GPU_SRAM["GPU SRAM Block Tiling"]
        Q_Tile["Query Block (64x64)"]
        K_Tile["Key Block (64x64)"]
    end
    
    Q_Tile -->|Coalesced MatMul| K_Tile
    K_Tile -->|Verify| BlockMask{"Block-Local Mask?"}
    BlockMask -->|Inside Window| Compute["Compute Attention"]
    BlockMask -->|Outside Window| Skip["Skip Block (Zero FLOPs)"]
```

---
[← Back to README](../README.md)
