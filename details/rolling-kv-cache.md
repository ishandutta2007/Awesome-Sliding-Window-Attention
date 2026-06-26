# Rolling KV Cache

## Overview
In standard Transformers, the Key-Value (KV) cache grows linearly with generation length. The **Rolling KV Cache** resolves this by capping cache memory size.

## Mechanism
The system maintains a fixed-capacity VRAM cache equal to the window size $W$. When generating token $i$, its KV coordinates overwrite the memory slot of token $i - W$ using a modulo operation:
$$\text{Slot Index} = i \pmod W$$

## Advantages
- **Constant Memory Footprint:** VRAM consumption remains flat and unchanging whether generating 100 or 100,000 tokens.
- **Hardware Integration:** Combines well with hardware-level memory pooling strategies.

## Diagram

```mermaid
flowchart LR
    subgraph Cache["Rolling Cache Ring (Capacity W=4)"]
        Slot0["Slot 0 (Token 4)"]
        Slot1["Slot 1 (Token 1)"]
        Slot2["Slot 2 (Token 2)"]
        Slot3["Slot 3 (Token 3)"]
    end
    
    NewToken["New Token 5"] -->|index % 4 = 1| Slot1
    style Slot1 fill:#f8d7da,stroke:#dc3545,stroke-width:2px
```

---
[← Back to README](../README.md)
