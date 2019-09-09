---
id: Vote
title: Vote
sidebar_label: Vote
---

# 概要

投票是区块链系统重要自治机制，如果一个节点持续为IOST社区服务、贡献代码和参与治理，那么这个节点一定会赢得更多社区投票。获得票数多的节点有机会参与造块，并获得奖励。积极参与投票，对社区发展非常重要，所以系统会对投票者给与 token 奖励。

## 节点类型
在我们投票机制中，有三种类型的节点：候选人、合伙人节点和出块节点。  
调用投票合约的 [applyRegister](../6-reference/SystemContract.html#applyregisterapplicant-pubkey-location-url-netid-isproducer) 方法即可成为候选人。候选人在得票数超过 210 万且审核通过后，会成为合伙人节点或者出块节点（由调用 applyRegister 时传入的最后一个参数决定，true 为出块节点，false 为合伙人节点）。出块节点为需要造块的节点，合伙人节点不需要造块。  

## 投票规则

- 1 token 拥有 1 投票权，1 投票权只能投给 1 个候选人、合伙人节点或出块节点
- 1 个账户可以给多个节点投票，节点自己也可以给自己投票
- 只有合伙人节点和出块节点以及他们的投票者才能参与投票奖励分红
- 质押购买资源的 token 没有投票权
- 取消投票后，需要等待 7 天赎回 token，赎回中的Token没有投票奖励

## 奖励
全网每年会增发 2% 的 token。1% 的 token 为造块奖励，只奖励给出块节点。1% 的 token 为投票奖励，其中的一半会奖励给合伙人节点和出块节点，另一半奖励给他们的投票者。

### 造块奖励规则
- 造块奖励按照每个节点造块数量进行分配，由增发速率（每年 2%）、造块速率（0.5 秒 1 个块）可算得，每造一个块的奖励约为 3.3 iost
- 造块奖励需要节点主动领取，领取方式为调用系统合约的 [exchangeIOST](../6-reference/SystemContract.html#exchangeiostaccount-amount) 方法

### 投票奖励规则

#### 节点奖励
- 系统每隔 172800 个区块（大约 24 小时）自动增发一次 token，增发的 token 会进去节点奖励池，并按照增发的时刻每个节点的得票数，按比例分配奖励
- 任何账号都可以通过调用投票合约的 [topupCandidateBonus](../6-reference/SystemContract.html#topupcandidatebonusamount-payer) 方法给节点奖励池充值，并按照充值时刻每个节点的得票数，按比例分配该笔充值
- 投票奖励需要节点主动领取，领取方式为调用投票合约的 [candidateWithdraw](../6-reference/SystemContract.html#candidatewithdrawcandidate) 方法
- 投票奖励的 50% 会在奖励领取时，进入投票者奖励池
- 已经获得但还未领取的奖励，不受节点属性变化和票数变化影响，且没有过期时间，随时可领取


#### 投票者奖励
- 当节点领取奖励时，奖励的 50% 会进入该节点的投票者奖励池，并按照当前时刻每个投票者的投票数，按比例分配奖励
- 任何账号都可以通过调用投票合约的 [topupVoterBonus](../6-reference/SystemContract.html#topupvoterbonuscandidate-amount-payer) 方法给某节点的投票奖励池充值，并按照充值时刻每个投票者的投票数，按比例分配该笔充值
- 投票奖励需要投票者主动领取，领取方式为调用投票合约的 [voterWithdraw](../6-reference/SystemContract.html#voterwithdrawvoter) 方法
- 已经获得但还未领取的奖励，不受投票者追加或取消投票等操作的影响，且没有过期时间，随时可领取
