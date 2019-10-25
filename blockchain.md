---
presentation:
  slideNumber: true
---

<!-- slide -->

# Blockchain 区块链

<!-- slide -->

# Definition

顾名思义，区块链

- 区块：就是包含了（交易）数据信息的一个数据块
- 链：由加密算法将区块一个接一个地顺序相连起来

每一个区块包含了它前一个区块的加密哈希，时间戳，以及交易信息  
（形成了一个 Merkle Tree）

<!-- slide -->

# History

区块链这个概念是由一个叫做 Satoshi Nakamoto（中本聪）于 2008 年发明的。  
他创建了世界上第一个开放的虚拟货币区块链，也就是我们如今的 Bitcoin (比特币)

<!-- slide -->

[Bitcoin paper](https://bitcoin.org/bitcoin.pdf)

![Screenshot from 2019-09-08 16-11-58](https://i.loli.net/2019/09/08/9d6uHq7owljCnDR.png)

<!-- slide -->

# 现阶段的货币的信用问题

第三方金融机构发放货币可能产生信用问题

<!-- note
首先，人民币是有价值的。为什么这些人民币纸币有价值呢？因为这是国家发行的，而国家说他是有价值，我们人民群众也认可国家。所以，人民币可以在我们之间交易，2元人民币可以买包方便面，10元可以买包巧克力。

然后我们思考一下，人民币会永远有这样的价值吗？不一定。什么情况下人民币会失去价值？亡国了！中央银行为了解决国库空虚，无限制地增发货币！这并不是不可能，世界上某些国家曾经，甚至正在上演这样的事情。人民币有价值，是国家向我们保证人民币是没问题的，你们可以放心使用。当发行方的信任出现问题时，货币的价值也就没办法得到保证了。
-->

货币交易依赖于第三方的金融机构。对于金融机构，完全不可逆的交易是无法实现的，导致了信用问题没有解决。

<!-- note
假如存在可逆交易，那么谁来决定是否可逆？如果是买卖双方决定能否可逆，那么只需要双方重新逆向转账一遍就可以解决了。这里面，只需要交易双方的共识。如果是第三方决定能否可逆，那么谁来决定这个第三方有权做判定？支付宝？银联？央行？比特币要解决的是货币层面去信任的问题。交易行为只在发生转账的双方之间开展，所有的信任关系都有这两者自己决定。比特币的PEER TO PEER，只考虑价值的传递。至于是否会有欺诈、后悔、退货等等，这些不是技术手段能解决的，所以得有使用者自己想办法处理吧。
-->

<!-- slide -->

比特币使用加密算法证明来解决传统的信任问题。允许两方在不通过第三方的情况下进行交易。同时保证这些交易是不可逆的。

<!-- slide -->

# Transactions

我们用一条电子签名链来定义电子货币的交易

![Screenshot from 2019-09-08 16-58-11](https://i.loli.net/2019/09/08/GT6Az7lirPfh5Z4.png)

- 没有解决双花（Double Spend）问题 <!-- .element: class="fragment" data-fragment-index="1" -->

<!--
没有解决双花（Double Spend）问题
https://www.jianshu.com/p/56617e91b12a
-->

<!-- slide -->

# Proof of Work (PoW)

![Screenshot from 2019-09-08 17-33-02](https://i.loli.net/2019/09/08/N6DTF4XrZlOvjwV.png)

```javascript
const previousHash =
  "00000273FF34FCE19D6B804EFF5A3F5747ADA4EAA22F1D49C01E52DDB7875B4B";
const transactions = { random: "data" }; // Maybe transactions
const nonce = 1; // Number once
const difficulity = 5;

const hash = hash(previousHash, transactions, nonce, difficulty);
// 6B86B273FF34FCE19D6B804EFF5A3F5747ADA4EAA22F1D49C01E52DDB7875B4B
// keep increasing nonce, and calculate until hash has 5 leading 0s
```

<!-- slide -->

# Problem solved

1. 如果我们要修改区块链中的过去一个区块的交易数据，那么就需要重新计算它的 hash 以及它之后所有区块的 hash。（很花时间）。

2. 一个 CPU 一张投票。只有最长的链才会被使用。

3. Proof of Work 会根据每小时内区块产生的数量而改变。<!-- 这样就保证了未来硬件升级了，但是每个小时还是会产生这么多的区块 -->

<!-- slide -->

# Network

区块链网络的运行

1. New transactions are broadcast to all nodes
   > 新的交易会被广播到所有的节点
2. Each node collects new transactions into a block
   > 每个节点收集新的交易到一个区块
3. Each node works on finding a difficult proof-of-work for its block
   > 每一个节点开始根据 PoW 算法来挖掘区块

<!-- slide -->

4. When a node finds a proof-of-work, it broadcasts the block to all nodes
   > 当一个节点完成了 PoW 挖到了一个区块，广播这个区块到所有的节点
5. Nodes accept the block only if all transactions in it are valid and not already spent
   > 节点接受这个区块如果这个区块中的所有交易都是有效的，并且没有被“双花”
6. Nodes express their acceptance of the block by working on creating the next block in the chain, using the hash of the accepted block as the previous hash.
   > 节点通过继续挖区块进而产生区块链中的下一个区块的形式表示接受了刚刚传输过来的区块

<!-- slide -->

一个节点只会考虑最长的链作为正确的链并且持续为它添加区块。  
如果两个节点同时广播了不同版本的下一个区块，一些节点可能先收到第一个版本的区块，另一些节点可能先收到第二个版本的区块。这个时候会产生两条链。  
应该在最先收到区块的链上工作，同时保存另一条链。持续工作直到一条链的长度更长，舍弃其他的链。

<!-- slide -->

# Incentive

1. 一般来讲，一个区块中的第一笔交易是一个特殊的交易。  
   这个交易生成新的货币作为这个区块创建者的奖励（挖矿奖励）。同时整个比特币系统中会有新的货币产生。产生的新货币数量和挖矿者使用的 CPU 时间和电量相关。

2. 同时挖矿者还会收到一笔交易者支付的 transaction fee（手续费）

<!-- slide -->

![Screenshot from 2019-09-08 18-54-23](https://i.loli.net/2019/09/08/6JWwl2GuxAcPz7Q.png)

<!-- slide -->

# Reclaiming Disk Space

在我们拥有足够多的区块之后，我们可以对过去区块中的交易数据进行压缩。所有的交易信息都用 Merkle Tree 来保存，现在我们只保存 root hash （根部的哈希）

![Screenshot from 2019-09-08 18-17-15](https://i.loli.net/2019/09/08/HeREGvmpBbAwM9V.png)

一个没有任何交易信息的 Block Header （区块头部）差不多是 80 bytes。假设我们没 10 分钟生成一个区块，那么一年我们会生成 `80 bytes * 6 * 24 * 365 = 4.2MB`。可以看出这并不会占据太大的空间。

<!-- slide -->

# Simplified Payment Verification

![Screenshot from 2019-09-08 18-31-14](https://i.loli.net/2019/09/08/kwjxbJWlK4y1uFd.png)

<!-- slide -->

# Privacy

比特币的钱包账户是由一对私钥和公钥组成，没有人知道你真的是谁。

- ![Screenshot from 2019-09-08 16-58-11](https://i.loli.net/2019/09/08/GT6Az7lirPfh5Z4.png) <!-- .element: class="fragment" data-fragment-index="1" -->

<!-- slide -->

# 其他区块链相关

1. Ethereum 以太坊智能合约
2. PoS（Proof of Stake）算法

<!-- slide -->

# Questions?

<!-- slide -->

# References

- [Bitcoin paper](https://bitcoin.org/bitcoin.pdf)
