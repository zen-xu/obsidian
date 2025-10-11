## Book

L2 Book 的产生本质是根据 [[Order]]/[[Trade]] 产生 [[Book]] 的过程

| **维度**           | **上海（SH）**                                           | **深圳（SZ）**                                                          |
| ---------------- | ---------------------------------------------------- | ------------------------------------------------------------------- |
| **市价单**          | ❌ 无市价单                                               | ✅ 有市价单（本方最优 U，对方最优/五档最优/FAK/FOK 等）                                  |
| **Order 内容**     | Add / Delete；只揭示影响盘口的部分（被直接成交的不会出现在 Order 流）         | Add；揭示所有 Order（包括直接成交的）                                             |
| **Trade 内容**     | Trade（成交）；无 Cancel；集合竞价可能 BSFlag=Unknown             | Trade / Cancel；集合竞价和连续竞价均有                                          |
| **撤单揭示位置**       | 在 Order 流（Delete）                                    | 在 Trade 流（Cancel）                                                   |
| **盘前数据揭示**       | 09:25 一次性推送 09:15–09:25 的报单/成交                       | 09:15 开始实时揭示                                                        |
| **盘后数据揭示**       | 15:00 统一揭示                                           | 实时揭示                                                                |
| **定序字段**         | BizIndex（ChannelNo 辅助）                               | OrderIndex / TradeIndex                                             |
| **Index 特征**     | TradeIndex 单调递增；BizIndex 用于跨报盘机定序                    | OrderIndex 单调递增；TradeIndex = Max(bid, ask)+1,+2…；Cancel 时一侧有效，另一侧=0 |
| **BSFlag**       | 来自算法标注；集合竞价可能=Unknown                                | B/S；撤单=X；笼子单可能错因延迟                                                  |
| **FunctionCode** | Trade 恒为 F；Order 方向字段                                | Trade：成交/撤单区分；Order：方向字段                                            |
| **OrderKind**    | 仅限价单；A=报单，D=撤单                                       | 限价单、市价单、本方最优 U；1=对方最优/五档最优/FAK/FOK                                  |
| **笼子订单**         | ❌ 无                                                  | ✅ 创业板（2020-08-24 起）；揭示但不体现在 Book                                    |
| **消费规则**         | Passive Order 不能直接加盘口，需等 Trade 确认；Order+Trade 同步决定盘口 | Passive Order 可直接消费；笼子单需“入笼”，等 Trade 释放；市价单需收全 Trade 才能确认类型         |