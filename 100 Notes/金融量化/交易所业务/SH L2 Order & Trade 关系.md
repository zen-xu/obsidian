---
tags: 业务
---
> 假定有 2 笔 Order, 每笔 Order 会产生 3 笔 Trade

O1
```mermaid
flowchart LR
  subgraph Order1
	  T11@{ shape: circle}
	  T12@{ shape: circle}
	  T13@{ shape: circle}
  end
  T11 --> T12 --> T13
```