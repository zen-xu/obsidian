---
tags: 量化
---
> 假定有 2 笔 Order, 每笔 Order 会产生 3 笔 Trade

```mermaid
flowchart TD

  T11@{ shape: circle}
  T12@{ shape: circle}
  T13@{ shape: circle}
  T21@{ shape: circle}
  T22@{ shape: circle}
  T23@{ shape: circle}

  subgraph Order1
    direction LR
    T11 --> T12 --> T13
  end

  subgraph Order2
    direction LR
    T21 --> T22 --> T23
  end

  Order1 ~~~ Order2
  
  style Order1 fill:None;
  style Order2 fill:None;
  classDef order1 fill:#f9f;
  classDef order2 fill:#bbf;
  class T11,T12,T13 order1;
  class T21,T22,T23 order2;
```
## 例1
```mermaid
---
title: SH 交易所产生数据的顺序
config:
  layout: elk
---
flowchart LR
  O1@{ shape: circle }
  T11@{ shape: circle }
  T12@{ shape: circle }
  T13@{ shape: circle }
  O2@{ shape: circle }
  T21@{ shape: circle }
  T22@{ shape: circle }
  T23@{ shape: circle }
  
  T11 --> T12 --> T13 --> O1
  O1 --> T21
  T21 --> T22 --> T23 --> O2
  
  classDef order1 fill:#f9f;
  classDef order2 fill:#bbf;
  class O1,T11,T12,T13 order1;
  class O2,T21,T22,T23 order2;
```
- 在这个序列里 [[biz_index]] 递增
- 同一笔订单的 [[trade|Trade]] 和 [[order|Order]] 是连续排布在一起, 所以 [[biz_index]] 也连续
---

```mermaid
---
title: 收到数据的顺序
config:
  layout: elk
---
flowchart LR
  O1@{ shape: circle }
  T11@{ shape: circle }
  T12@{ shape: circle }
  T13@{ shape: circle }
  O2@{ shape: circle }
  T21@{ shape: circle }
  T22@{ shape: circle }
  T23@{ shape: circle }
  
  T11 --> T12 --> O1 --> O2 --> T13
  T13 --> T21 --> T22 --> T23
  
  classDef order1 fill:#f9f;
  classDef order2 fill:#bbf;
  class O1,T11,T12,T13 order1;
  class O2,T21,T22,T23 order2;
```
- 我们假定收到的 [[trade|Trade]] / [[order|Order]] 流本身是有序的
- 这种情况下, 由于我们能收到 `O1`, 所以我们在收到 `T21` 时才能确定 `O1` 已经补充完全, 即可直接发送至下游
## 例 2
```mermaid
---
title: SH 交易所产生数据的顺序
config:
  layout: elk
---
flowchart LR
  T11@{ shape: circle }
  T12@{ shape: circle }
  T13@{ shape: circle }
  O2@{ shape: circle }
  T21@{ shape: circle }
  T22@{ shape: circle }
  T23@{ shape: circle }
  
  T11 --> T12 --> T13 --> T21
  T21 --> T22 --> T23 --> O2
  
  classDef order1 fill:#f9f;
  classDef order2 fill:#bbf;
  class O1,T11,T12,T13 order1;
  class O2,T21,T22,T23 order2;
```
- 在这个序列里 [[biz_index]] 递增
- 同一笔订单的 [[trade|Trade]] 和 [[order|Order]] 是连续排布在一起, 所以 [[biz_index]] 也连续
- <mark style="background: #FFB8EBA6;">这个序列里 O1 并没被交易所生成, 意味着 O1 全部成交</mark>, 上交所不会产生 [[order|Order]] 数据, 所以只有 `O2` 的 3 笔 [[trade|Trade]] 
---
```mermaid
---
title: 收到数据的顺序 case1
config:
  layout: elk
---
flowchart LR
  T11@{ shape: circle }
  T12@{ shape: circle }
  T13@{ shape: circle }
  O2@{ shape: circle }
  T21@{ shape: circle }
  T22@{ shape: circle }
  T23@{ shape: circle }
  
  T11 --> T12 --> O2 --> T13 --> T21
  T21 --> T22 --> T23
  
  classDef order1 fill:#f9f;
  classDef order2 fill:#bbf;
  class O1,T11,T12,T13 order1;
  class O2,T21,T22,T23 order2;
```
- 我们假定收到的 [[trade|Trade]] / [[order|Order]] 流本身是有序的
- 这种情况下, 我们必须收到 `O2` 及 `T21` 后, 才能确定 `O1` 已补充完全, 然后可发生至下游
    - `O2` 确认了 [[order|Order]] 流中已不存在 `O1`
    - `T21` 确认了 [[trade|Trade]] 流中已不存在 `O1` 相关的 [[Trade]] 数据

---

```mermaid
---
title: 收到数据的顺序 case2
config:
  layout: elk
---
flowchart LR
  T11@{ shape: circle }
  T12@{ shape: circle }
  T13@{ shape: circle }
  O2@{ shape: circle }
  T21@{ shape: circle }
  T22@{ shape: circle }
  T23@{ shape: circle }
  
  T11 --> T12 --> T13 --> T21
  T21 --> T22 --> T23 --> O2
  
  classDef order1 fill:#f9f;
  classDef order2 fill:#bbf;
  class O1,T11,T12,T13 order1;
  class O2,T21,T22,T23 order2;
```
- 我们假定收到的 [[trade|Trade]] / [[order|Order]] 流本身是有序的
- 