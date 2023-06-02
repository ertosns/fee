---
title: fine-grain base fee with first price tip auction in fixed slot length.
author: ertosns
date: 2/6/2023
---

# fixed slot length
- fixed slot length dictates that there is limited amount of computation possible during single slot block gas, or block computational cost is fixed let's call it C, let's distinguish between block computational cost capacity CC, and actual block cost C.

# fee volatility
- sudden increase/decrease in demand to fixed supply would lead to spikes in fee cost, and it' can't be accommodated for nor maintained increase in demand, such issues are not auction mechanism design issue, but scalability issues that could mitigated through zk rollups, sharding, parallel tx processing, faster consensus protocols, etc.

# base fee burning
- burning base fee `b` based off parent blocks computational cost, with two conditions: base fee is based off previous blocks computational cost to avoid malicious attacks from current miner to drive fee up, secondly base fee is deterministically calculated rather than agreement between user, and leader in which collusion can occur off-chain to evade base fee burning.

## why fee is burned
- reduce inflation, raise DRK value.
- increase cost of spamming, and fake transaction.

## tatonment and control
- eip1559[^1] propose base fee update rule and discovery of market clearing price, and report on the protocol[^2] propose interpolation of base fee between the two different block gas costs at 12.5M, and 25M for fine grained base fee results.
- however due to fixed slot length, and inability to mitigate sudden increase/decrease in demand more fine grain base fee technique is required, that tatonment addressed above is bad approximation of PID controller.

### base fee based off PID control
- fine grain tatonment can be achieved through PID controller that can reduce spikes in base fee price due to change in demand, with feedback from previous block's cost C.
- for example base fee f at block i is $$ f[i] = f[i-1] + K_1e[i] + K_2 e[i-1] + K_3e[i-2] $$
- where $e[i]$ is error function in previous block i-1, $K_1$, $K_2$, $K_3$ are discrete controller parameters (to be tuned).
- $e[i]$ is the difference between block computation cost capacity, and block cost:
  $$ e[i] = CC[i-1] - C[i-i]$$

# tip first price auction
- tip is similar to eip1559 is a priority fee implemented as first price auction due to simplicity, and undue trusted third party required in more efficient auctions i.e vickery auction.
- client/user, or tx creators propose tx tip `t`, assuming client's fee $ = t+b = $ max cap fee, then auction utility is nonzero, and max cap isn't required to be added by client in tx header, only tx tip is sufficient.


# computation unites
- since slots has limited computation unites, computation unites is better suited for fee estimation than gas price based of block size, or type of contract, every dedicated contract call better have some unite of computation for more accurate estimation of cost, otherwise, lead might ditch computationally expensive txs. in that sense, there are two attributes of txs, tips, and computational cost.

# delay due to empty slots.
- empty slots with no leads would accumulate pending txs, and would increase base fee on subsequent slots, and users would increase tips for inclusion.

[^1]: https://eips.ethereum.org/EIPS/eip-1559
[^2]: http://timroughgarden.org/papers/eip1559.pdf
