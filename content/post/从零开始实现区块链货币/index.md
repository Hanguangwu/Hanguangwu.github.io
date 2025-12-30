---
title: "从零开始实现区块链货币"
date: 2025-07-29T10:34:25+08:00
lastmod: 2025-07-29T10:54:22+08:00
draft: false
math: true
categories:
- 编程
tags:
- 区块链
- 虚拟货币
# description->需要自己编写的文章描述，是搜索引擎呈现在搜索结果链接下方的网页简介，建议设置
description: ""
weight: # 输入1可以顶置文章，用来给文章展示排序，不填就默认按时间排序
slug: ""
draft: false # 是否为草稿
comments: true
showToc: true # 显示目录
TocOpen: true # 自动展开目录
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
disableShare: true # 底部不显示分享栏

---

## 前言

{{< bilibili BV14E411v7eS >}}  

下面的代码和视频教程不一致，以下面的代码为准，截止到发文日期，下面代码可以正常运行。

## 区块链构建及哈希值校验

可运行代码如下所示：

```js
import pkg from 'crypto-js';

const { SHA256 } = pkg;

class Block {
    constructor(data, previousBlockHash) {
        this.data = data
        this.previousBlockHash = previousBlockHash
        this.blockHash = this.computeHash()
    }
    computeHash() {
        return SHA256(this.data + this.previousBlockHash).toString()
    }
}

// 链需要有一个 genesis blcok
class Chain {
    constructor(){
        this.chain = [this.bigBang()]
    }
    bigBang(){
        const genesisBlcok = new Block("bigbang", "")
        return genesisBlcok
    }
    getLatestBlock(){
        return this.chain[this.chain.length-1]
    }
    addBlcokToChain(newBlcok){
        //找到最新的block的哈希值
        newBlcok.previousBlockHash = this.getLatestBlock().blockHash
        newBlcok.blockHash = newBlcok.computeHash()
        this.chain.push(newBlcok)
    }
    // 验证当前链是否合法
    // 验证数据有没有被篡改
    // 我们需要验证当前链的previousBlockHash是否等于前一个区块的blcokHash
    validateChain(){
        if(this.chain.length === 1){
            if(this.chain[0].blockHash !== this.chain[0].computeHash()){
                return false
            }
            return true
        }
        // 从第二个区块this.chain[1]开始比较验证
        for(let i = 1; i <= this.chain.length-1; i++){
            const blockNeedToValidate = this.chain[i]
            //当前数据有没有被篡改
            if(blockNeedToValidate.blockHash !== blockNeedToValidate.computeHash()){
                console.log("数据已被篡改！")
                return false
            }
            //验证链连接
            const previousBlock = this.chain[i-1]
            if(blockNeedToValidate.previousBlockHash !== previousBlock.blockHash){
                console.log("前后区块链断裂")
                return false
            }
        }
        return true
    }
}

const THChain = new Chain()

const blcok1 = new Block('转账10元', '')
THChain.addBlcokToChain(blcok1)
const blcok2 = new Block('转账100元', '')
THChain.addBlcokToChain(blcok2)
const blcok3 = new Block('转账1000元', '')
THChain.addBlcokToChain(blcok3)


THChain.chain[1].data = '转账100000元'
THChain.chain[1].blockHash = THChain.chain[1].computeHash()
console.log(THChain)
console.log(THChain.validateChain())
```

把上面代码保存为`main.js`，然后执行`node main.js`即可输出如下的内容：

```
PS D:\Documents\blockchain\crashcourse> node .\main.js
Chain {
  chain: [
    Block {
      data: 'bigbang',
      previousBlockHash: '',
      blockHash: 'ada4c1ccf3539d5c78368d25723735218c54638cb3f9e4347fe204b5a3503b7a'
    },
    Block {
      data: '转账100000元',
      previousBlockHash: 'ada4c1ccf3539d5c78368d25723735218c54638cb3f9e4347fe204b5a3503b7a',
      blockHash: 'cb52196495a2a8aa4ff968bd2fa3111e76ceabc44c072174a14c0f132a49fa18'
    },
    Block {
      data: '转账100元',
      previousBlockHash: 'cb52196495a2a8aa4ff968bd2fa3111e76ceabc44c072174a14c0f132a49fa18',
      blockHash: 'acc447143f9e46dbf98a1c8868ce10094708865f32ed4d49f37b66bbcf7ede7a'
    },
    Block {
      data: '转账1000元',
      previousBlockHash: 'acc447143f9e46dbf98a1c8868ce10094708865f32ed4d49f37b66bbcf7ede7a',
      blockHash: '7f833391bb7aafc9a2ec1def3b8991a069f2bead0e3ff9f3ac3b1602e49cfae0'
    }
  ]
}
数据已被篡改！
false
```

```
PS D:\Documents\blockchain\crashcourse> node .\main.js
Chain {
  chain: [
    Block {
      data: 'bigbang',
      previousBlockHash: '',
      blockHash: 'ada4c1ccf3539d5c78368d25723735218c54638cb3f9e4347fe204b5a3503b7a'
    },
    Block {
      data: '转账100000元',
      previousBlockHash: 'ada4c1ccf3539d5c78368d25723735218c54638cb3f9e4347fe204b5a3503b7a',
      blockHash: '51475f91775224fee6ca4a9c844396d7fcd9734d8162f291400dac1f9e3157a4'
    },
    Block {
      data: '转账100元',
      previousBlockHash: 'cb52196495a2a8aa4ff968bd2fa3111e76ceabc44c072174a14c0f132a49fa18',
      blockHash: 'acc447143f9e46dbf98a1c8868ce10094708865f32ed4d49f37b66bbcf7ede7a'
    },
    Block {
      data: '转账1000元',
      previousBlockHash: 'acc447143f9e46dbf98a1c8868ce10094708865f32ed4d49f37b66bbcf7ede7a',
      blockHash: '7f833391bb7aafc9a2ec1def3b8991a069f2bead0e3ff9f3ac3b1602e49cfae0'
    }
  ]
}
前后区块链断裂
false
```


## PoW（工作量证明）和挖矿

`nonce`用于计数来改变`blockHash`。

```js
import pkg from 'crypto-js';

const { SHA256 } = pkg;

class Block {
    constructor(data, previousBlockHash) {
        this.data = data
        this.previousBlockHash = previousBlockHash
        this.nonce = 1
        this.blockHash = this.computeHash()
    }
    computeHash() {
        return SHA256(this.data + this.previousBlockHash + this.nonce).toString()
    }
    getAnswer(difficulty){
        //前difficulty位为0的哈希
        let answer = ''
        for(let i = 0; i < difficulty; i++){
            answer += '0'
        }
        return answer
    }
    //计算符合难度的哈希值
    mine(difficulty){
        while(true){
            this.blockHash = this.computeHash()
            if(this.blockHash.substring(0, difficulty) !== this.getAnswer(difficulty)){
                this.blockHash = this.computeHash()
                this.nonce++
            }else{
                break
            }
        }
        console.log("挖矿结束，哈希值为", this.blockHash)
    }
}

// 链需要有一个 genesis blcok
class Chain {
    constructor(){
        this.chain = [this.bigBang()]
        this.difficulty = 2
    }
    bigBang(){
        const genesisBlcok = new Block("bigbang", "")
        return genesisBlcok
    }
    getLatestBlock(){
        return this.chain[this.chain.length-1]
    }
    addBlockToChain(newBlcok){
        //找到最新的block的哈希值
        newBlcok.previousBlockHash = this.getLatestBlock().blockHash
        // newBlcok.blockHash = newBlcok.computeHash()
        newBlcok.mine(this.difficulty)
        this.chain.push(newBlcok)
    }
    // 验证当前链是否合法
    // 验证数据有没有被篡改
    // 我们需要验证当前链的previousBlockHash是否等于前一个区块的blcokHash
    validateChain(){
        if(this.chain.length === 1){
            if(this.chain[0].blockHash !== this.chain[0].computeHash()){
                return false
            }
            return true
        }
        // 从第二个区块this.chain[1]开始比较验证
        for(let i = 1; i <= this.chain.length-1; i++){
            const blockNeedToValidate = this.chain[i]
            //当前数据有没有被篡改
            if(blockNeedToValidate.blockHash !== blockNeedToValidate.computeHash()){
                console.log("数据已被篡改！")
                return false
            }
            //验证链连接
            const previousBlock = this.chain[i-1]
            if(blockNeedToValidate.previousBlockHash !== previousBlock.blockHash){
                console.log("前后区块链断裂")
                return false
            }
        }
        return true
    }
}

const THChain = new Chain()

const blcok1 = new Block('转账10元', '')
THChain.addBlockToChain(blcok1)
const blcok2 = new Block('转账100元', '')
THChain.addBlockToChain(blcok2)
const blcok3 = new Block('转账1000元', '')
THChain.addBlockToChain(blcok3)


THChain.chain[1].data = '转账100000元'
THChain.chain[1].blockHash = THChain.chain[1].computeHash()
console.log(THChain)
console.log(THChain.validateChain())
```

执行`node main.js`输出结果如下所示：

```
挖矿结束，哈希值为 0031333356747ea2eac85d87929e1a4ffe7b7ba7d7f8a617da56aae5736f76cc
挖矿结束，哈希值为 00854c4111894a94944474cee375ff20c8e24ea811fc7144d1931e5d72152880
挖矿结束，哈希值为 003bbd22c37e5abc75e84230e43a942f11859586fd24230b087903bb4d99a9a0
Chain {
  chain: [
    Block {
      data: 'bigbang',
      previousBlockHash: '',
      nonce: 1,
      blockHash: '3f725e072ada9321f567dea6c8a690c280536e76ee89f7d76e51c7e29be0ac00'
    },
    Block {
      data: '转账100000元',
      previousBlockHash: '3f725e072ada9321f567dea6c8a690c280536e76ee89f7d76e51c7e29be0ac00',
      nonce: 40,
      blockHash: 'f8dcaa815fea240246f3c5d0c9c0bdb04d9a9ae7164070688e658bd0cf0b372b'
    },
    Block {
      data: '转账100元',
      previousBlockHash: '0031333356747ea2eac85d87929e1a4ffe7b7ba7d7f8a617da56aae5736f76cc',
      nonce: 209,
      blockHash: '00854c4111894a94944474cee375ff20c8e24ea811fc7144d1931e5d72152880'
    },
    Block {
      data: '转账1000元',
      previousBlockHash: '00854c4111894a94944474cee375ff20c8e24ea811fc7144d1931e5d72152880',
      nonce: 415,
      blockHash: '003bbd22c37e5abc75e84230e43a942f11859586fd24230b087903bb4d99a9a0'
    }
  ],
  difficulty: 2
}
前后区块链断裂
false
```

## 存储交易信息和时间戳

```js
import pkg from 'crypto-js';

const { SHA256 } = pkg;

class Transation {
    constructor(from, to, amount){
        this.from = from
        this.to = to
        this.amount = amount
    }
}

class Block {
    constructor(transations, previousBlockHash) {
        this.transations = transations
        this.previousBlockHash = previousBlockHash
        this.nonce = 1
        this.timestamp = Date.now()
        this.blockHash = this.computeHash()
    }
    computeHash() {
        return SHA256(
            JSON.stringify(this.transations) + 
            this.previousBlockHash + 
            this.nonce + 
            this.timestamp
        ).toString()
    }
    getAnswer(difficulty){
        //前difficulty位为0的哈希
        let answer = ''
        for(let i = 0; i < difficulty; i++){
            answer += '0'
        }
        return answer
    }
    //计算符合难度的哈希值
    mine(difficulty){
        while(true){
            this.blockHash = this.computeHash()
            if(this.blockHash.substring(0, difficulty) !== this.getAnswer(difficulty)){
                this.blockHash = this.computeHash()
                this.nonce++
            }else{
                break
            }
        }
        console.log("挖矿结束，哈希值为", this.blockHash)
    }
}

// 链需要有一个 genesis blcok
class Chain {
    constructor(){
        this.chain = [this.bigBang()]
        this.transactionPool = []
        this.minerReward = 50
        this.difficulty = 2
    }
    bigBang(){
        const genesisBlcok = new Block("bigbang", "")
        return genesisBlcok
    }
    getLatestBlock(){
        return this.chain[this.chain.length-1]
    }
    addTransactionToPool(transation){
        this.transactionPool.push(transation)
    }
    addBlockToChain(newBlcok){
        //找到最新的block的哈希值
        newBlcok.previousBlockHash = this.getLatestBlock().blockHash
        // newBlcok.blockHash = newBlcok.computeHash()
        newBlcok.mine(this.difficulty)
        this.chain.push(newBlcok)
    }
    mineTransactionPool(minerRewardAddress){
        // 发放矿工奖励
        const minerRewardTransaction = new Transation("", minerRewardAddress, this.minerReward)
        this.transactionPool.push(minerRewardTransaction)
        // 挖矿
        const newBlcok = new Block(this.transactionPool, this.getLatestBlock().blockHash)
        newBlcok.mine(this.difficulty)
        // 添加区块到区块链
        // 清空 transactionPool
        this.chain.push(newBlcok)
        this.transactionPool = []
    }
    // 验证当前链是否合法
    // 验证数据有没有被篡改
    // 我们需要验证当前链的previousBlockHash是否等于前一个区块的blcokHash
    validateChain(){
        if(this.chain.length === 1){
            if(this.chain[0].blockHash !== this.chain[0].computeHash()){
                return false
            }
            return true
        }
        // 从第二个区块this.chain[1]开始比较验证
        for(let i = 1; i <= this.chain.length-1; i++){
            const blockNeedToValidate = this.chain[i]
            //当前数据有没有被篡改
            if(blockNeedToValidate.blockHash !== blockNeedToValidate.computeHash()){
                console.log("数据已被篡改！")
                return false
            }
            //验证链连接
            const previousBlock = this.chain[i-1]
            if(blockNeedToValidate.previousBlockHash !== previousBlock.blockHash){
                console.log("前后区块链断裂")
                return false
            }
        }
        return true
    }
}

const THChain = new Chain()

const t1 = new Transation('addr1', 'addr2', 10)
const t2 = new Transation('addr2', 'addr1', 5)
THChain.addTransactionToPool(t1)
THChain.addTransactionToPool(t2)
// console.log(THChain)
THChain.mineTransactionPool('addr3')
console.log(THChain)
console.log(THChain.chain[1])
console.log(THChain.chain[1].transations)

```

执行`node main.js`，输出结果如下所示：

```
挖矿结束，哈希值为 000a03f3e1ce638391e099ea4406bcea3f66097cf6576efebb37be8b3b4b97c4
```

`THChain`如下所示：

```
Chain {
  chain: [
    Block {
      transations: 'bigbang',
      previousBlockHash: '',
      nonce: 1,
      timestamp: 1751182697906,
      blockHash: '347dad0da967b9ce740a84ed4dc3f2e1d26e93564b054cfaa44be31540cb4393'
    },
    Block {
      transations: [Array],
      previousBlockHash: '347dad0da967b9ce740a84ed4dc3f2e1d26e93564b054cfaa44be31540cb4393',
      nonce: 49,
      timestamp: 1751182697906,
      blockHash: '000a03f3e1ce638391e099ea4406bcea3f66097cf6576efebb37be8b3b4b97c4'
    }
  ],
  transactionPool: [],
  minerReward: 50,
  difficulty: 2
}
```

`THChain.chain[1]`内容如下所示：

```
Block {
  transations: [
    Transation { from: 'addr1', to: 'addr2', amount: 10 },
    Transation { from: 'addr2', to: 'addr1', amount: 5 },
    Transation { from: '', to: 'addr3', amount: 50 }
  ],
  previousBlockHash: '347dad0da967b9ce740a84ed4dc3f2e1d26e93564b054cfaa44be31540cb4393',
  nonce: 49,
  timestamp: 1751182697906,
  blockHash: '000a03f3e1ce638391e099ea4406bcea3f66097cf6576efebb37be8b3b4b97c4'
}
```

`THChain.chain[1].transations`内容如下所示：

```
[
  Transation { from: 'addr1', to: 'addr2', amount: 10 },
  Transation { from: 'addr2', to: 'addr1', amount: 5 },
  Transation { from: '', to: 'addr3', amount: 50 }
]
```

## Secp256k1 椭圆曲线算法加密文档


```js
import { createRequire } from 'module';
const require = createRequire(import.meta.url);
const EC = require('elliptic').ec;

// Example: Using the elliptic curve secp256k1
const ec = new EC('secp256k1');

// Generate a key pair
const keyPair = ec.genKeyPair();
console.log('Private Key:', keyPair.getPrivate('hex'));
// 输出Private Key: 93a4d31c0eb2f9549cd03ae6022e96ce5d30e835dabeeaa057c3c0c18c2b3356
console.log('Public Key:', keyPair.getPublic('hex'));
// 输出Public Key: 0498ea8ba292c4ded976cc65487bc2c0c6d1197cc4627e13db324698c2b34a5bd40c068dcbb076370150d479e9a8b493f08d38246e322acd125fc1c50cfba0bbd8

// const ecLib = require('elliptic').ec
// const ec = new ecLib('secp256k1')

import pkg from 'crypto-js';

const { SHA256 } = pkg;

// 生成密钥对
const key = ec.genKeyPair()
// 转为hex string格式
console.log('private', key.getPrivate('hex'))
// 输出private 0f3c481e97450418b3d9870b5867bfac68dddf2aff1010d58f01945bb1a6d005
console.log('public', key.getPublic('hex'))
// 输出public 04c9068c475db693100df9d67edb5d2de7fc42b5f178dfd0dd4669a8e0336fe83b28add893b46d39eeccbf34adb5ecebc29cfa8b2856408f246c226ad05bf2d3b8
const doc = "V我50"
const hashedDoc = SHA256(doc).toString()
const hexSignature = key.sign(hashedDoc, 'base64').toDER('hex')

console.log('hashed doc', hashedDoc)
//输出hashed doc b581639349e48a9670696266e0f664765ca7011a4bc850d2a174d6ef9886e0e5
console.log('signature', hexSignature)
//输出signature 3045022100d326c6ddc8fcc360bb0e81b1d004599e242f89655cdcc2e34c4c8a218effd4f602202cf090c1e0c783d8815ab6bd5a607a29a1440c07b2204b931b4ee22f215c7f59

// 接收者验证签名
const publicKey = ec.keyFromPublic(key.getPublic('hex'), 'hex')
console.log(publicKey.verify(hashedDoc, hexSignature))
// 输出true
// 篡改文档
const temperedDoc = 'V我51'
const hashedTemperedDoc = SHA256(temperedDoc).toString()
console.log(publicKey.verify(hashedTemperedDoc, hexSignature))
// 输出false
```

## 数字签名验证交易合法性和虚拟货币发行

公钥充当地址。

```js
import pkg from 'crypto-js';

const { SHA256 } = pkg;

import { createRequire } from 'module';
const require = createRequire(import.meta.url);
const EC = require('elliptic').ec;

// Example: Using the elliptic curve secp256k1
const ec = new EC('secp256k1');

class Transation {
    constructor(from, to, amount){
        this.from = from
        this.to = to
        this.amount = amount
    }
    computeHash(){
        return SHA256(this.from + this.to + this.amount).toString()
    }
    sign(key){
        this.signature = key.sign(this.computeHash(), 'base64').toDER('hex')
    }
    isValid(){
        if(this.from == ""){
            return true
        }
        const keyObj = ec.keyFromPublic(this.from, 'hex')
        return keyObj.verify(this.computeHash(), this.signature)
    }
}

class Block {
    constructor(transations, previousBlockHash) {
        this.transations = transations
        this.previousBlockHash = previousBlockHash
        this.nonce = 1
        this.timestamp = Date.now()
        this.blockHash = this.computeHash()
    }
    computeHash() {
        return SHA256(
            JSON.stringify(this.transations) + 
            this.previousBlockHash + 
            this.nonce + 
            this.timestamp
        ).toString()
    }
    getAnswer(difficulty){
        //前difficulty位为0的哈希
        let answer = ''
        for(let i = 0; i < difficulty; i++){
            answer += '0'
        }
        return answer
    }
    //计算符合难度的哈希值
    mine(difficulty){
        this.validateBlockTransactions()
        while(true){
            this.blockHash = this.computeHash()
            if(this.blockHash.substring(0, difficulty) !== this.getAnswer(difficulty)){
                this.blockHash = this.computeHash()
                this.nonce++
            }else{
                break
            }
        }
        console.log("挖矿结束，哈希值为", this.blockHash)
    }
    validateBlockTransactions(){
        for(let transaction of this.transations){
            if(!transaction.isValid()){
                console.log('Invalid transaction found in transactions.')
                return false
            }
        }
        return true
    }
}

// 链需要有一个 genesis blcok
class Chain {
    constructor(){
        this.chain = [this.bigBang()]
        this.transactionPool = []
        this.minerReward = 50
        this.difficulty = 2
    }
    bigBang(){
        const genesisBlcok = new Block("bigbang", "")
        return genesisBlcok
    }
    getLatestBlock(){
        return this.chain[this.chain.length-1]
    }
    addTransactionToPool(transation){
        if(!transation.isValid()){
            throw new Error('Invalid transaction!')
        }
        console.log('Valid transaction.')
        this.transactionPool.push(transation)
    }
    addBlockToChain(newBlcok){
        //找到最新的block的哈希值
        newBlcok.previousBlockHash = this.getLatestBlock().blockHash
        // newBlcok.blockHash = newBlcok.computeHash()
        newBlcok.mine(this.difficulty)
        this.chain.push(newBlcok)
    }
    mineTransactionPool(minerRewardAddress){
        // 发放矿工奖励
        const minerRewardTransaction = new Transation("", minerRewardAddress, this.minerReward)
        this.transactionPool.push(minerRewardTransaction)
        // 挖矿
        const newBlcok = new Block(this.transactionPool, this.getLatestBlock().blockHash)
        newBlcok.mine(this.difficulty)
        // 添加区块到区块链
        // 清空 transactionPool
        this.chain.push(newBlcok)
        this.transactionPool = []
    }
    // 验证当前链是否合法
    // 验证数据有没有被篡改
    // 我们需要验证当前链的previousBlockHash是否等于前一个区块的blcokHash
    validateChain(){
        if(this.chain.length === 1){
            if(this.chain[0].blockHash !== this.chain[0].computeHash()){
                return false
            }
            return true
        }
        // 从第二个区块this.chain[1]开始比较验证
        for(let i = 1; i <= this.chain.length-1; i++){
            const blockNeedToValidate = this.chain[i]
            if(!blockNeedToValidate.validateBlockTransactions()){
                console.log("发现非法交易！")
                return false
            }
            //当前数据有没有被篡改
            if(blockNeedToValidate.blockHash !== blockNeedToValidate.computeHash()){
                console.log("数据已被篡改！")
                return false
            }
            //验证链连接
            const previousBlock = this.chain[i-1]
            if(blockNeedToValidate.previousBlockHash !== previousBlock.blockHash){
                console.log("前后区块链断裂")
                return false
            }
        }
        return true
    }
}

// 生成密钥对
const keyPairSender = ec.genKeyPair()
// 转为hex string格式
const privateKeySender = keyPairSender.getPrivate('hex')
const publicKeySender = keyPairSender.getPublic('hex')
// 生成密钥对
const keyPairReceiver = ec.genKeyPair()
// 转为hex string格式
const privateKeyReceiver = keyPairReceiver.getPrivate('hex')
const publicKeyReceiver = keyPairReceiver.getPublic('hex')


const THChain = new Chain()

const t1 = new Transation(publicKeySender, publicKeyReceiver, 10)
t1.sign(keyPairSender)
console.log(t1)
console.log(t1.isValid())
THChain.addTransactionToPool(t1)
THChain.mineTransactionPool(publicKeyReceiver)
console.log(THChain)
console.log(THChain.chain[1])
console.log(THChain.chain[1].transations)
```

执行`node main.js`输出结果如下所示：

```
Transation {
  from: '046d2a81cbea046efad0f4ee6c1a204cef488ba8e8a89d295d8c63df835b059f445b2063c1a0e59ebebd7a4990edb0f53af3b0c039d91c50555e95aa4464c5e59e',
  to: '047284ba130395028d20cc246609c3166f4735565e5a88be42e4aec4289edc43e68c8d0f0765f7cb6cb2a72c21275a0eba7decabbb6247a6cb8b0d9c9fdd02b619',
  amount: 10,
  signature: '3045022100f456fee1aa23f3a07cc5f9014dcd4a8d765978c333b8eb7c2c0f09cb6bd6417c0220159de6f4a56e7ba82dc20bf2d980f28878f6eceacdc2ce06a8f0306f1f7670e8'
}
true
Valid transaction.
挖矿结束，哈希值为 00ea4c08fd250e8d69d0e9483f3b8dea52ea1807569c6355a20dad037a6a370a
```

`THChain`如下所示：

```
Chain {
  chain: [
    Block {
      transations: 'bigbang',
      previousBlockHash: '',
      nonce: 1,
      timestamp: 1751188802768,
      blockHash: '9543633b1d6e714b4594bba1dd2f406180ce3456ae1b46f403b65b527bc8bc2e'
    },
    Block {
      transations: [Array],
      previousBlockHash: '9543633b1d6e714b4594bba1dd2f406180ce3456ae1b46f403b65b527bc8bc2e',
      nonce: 151,
      timestamp: 1751188802790,
      blockHash: '00ea4c08fd250e8d69d0e9483f3b8dea52ea1807569c6355a20dad037a6a370a'
    }
  ],
  transactionPool: [],
  minerReward: 50,
  difficulty: 2
}
```

`THChain.chain[1]`内容如下所示：

```
Block {
  transations: [
    Transation {
      from: '046d2a81cbea046efad0f4ee6c1a204cef488ba8e8a89d295d8c63df835b059f445b2063c1a0e59ebebd7a4990edb0f53af3b0c039d91c50555e95aa4464c5e59e',
      to: '047284ba130395028d20cc246609c3166f4735565e5a88be42e4aec4289edc43e68c8d0f0765f7cb6cb2a72c21275a0eba7decabbb6247a6cb8b0d9c9fdd02b619',
      amount: 10,
      signature: '3045022100f456fee1aa23f3a07cc5f9014dcd4a8d765978c333b8eb7c2c0f09cb6bd6417c0220159de6f4a56e7ba82dc20bf2d980f28878f6eceacdc2ce06a8f0306f1f7670e8'
    },
    Transation {
      from: '',
      to: '047284ba130395028d20cc246609c3166f4735565e5a88be42e4aec4289edc43e68c8d0f0765f7cb6cb2a72c21275a0eba7decabbb6247a6cb8b0d9c9fdd02b619',
      amount: 50
    }
  ],
  previousBlockHash: '9543633b1d6e714b4594bba1dd2f406180ce3456ae1b46f403b65b527bc8bc2e',
  nonce: 151,
  timestamp: 1751188802790,
  blockHash: '00ea4c08fd250e8d69d0e9483f3b8dea52ea1807569c6355a20dad037a6a370a'
}
```

`THChain.chain[1].transations`内容如下所示：

```
[
  Transation {
    from: '046d2a81cbea046efad0f4ee6c1a204cef488ba8e8a89d295d8c63df835b059f445b2063c1a0e59ebebd7a4990edb0f53af3b0c039d91c50555e95aa4464c5e59e',
    to: '047284ba130395028d20cc246609c3166f4735565e5a88be42e4aec4289edc43e68c8d0f0765f7cb6cb2a72c21275a0eba7decabbb6247a6cb8b0d9c9fdd02b619',
    amount: 10,
    signature: '3045022100f456fee1aa23f3a07cc5f9014dcd4a8d765978c333b8eb7c2c0f09cb6bd6417c0220159de6f4a56e7ba82dc20bf2d980f28878f6eceacdc2ce06a8f0306f1f7670e8'
  },
  Transation {
    from: '',
    to: '047284ba130395028d20cc246609c3166f4735565e5a88be42e4aec4289edc43e68c8d0f0765f7cb6cb2a72c21275a0eba7decabbb6247a6cb8b0d9c9fdd02b619',
    amount: 50
  }
]
```

## 附录

### ES6 与 CommonJS 的区别

#### 模块定义与导入

1. ES6模块

- 使用 `export` 和 `import` 关键字
- 静态编译，在编译时确定依赖关系
- 支持按需导入和导出
- 示例：

```javascript
// 导出
export const name = 'example';
export default function func() {}

// 导入
import func, { name } from './module';
```

2. CommonJS模块

- 使用 `module.exports` 和 `require()`
- 动态加载，运行时处理依赖
- 整体导出对象
- 示例：

```javascript
// 导出
module.exports = {
  name: 'example',
  func: function() {}
};

// 导入
const module = require('./module');
```

#### 加载机制

- ES6：静态加载，编译时确定
- CommonJS：动态加载，运行时加载

#### 导出方式

- ES6：支持具名导出和默认导出
- CommonJS：只能导出单个对象

### 比特币的共识机制与加密算法

#### POW（工作量证明）共识机制

1. 基本原理

- 矿工通过解决复杂数学问题来验证和打包交易
- 找到特定哈希值，满足难度要求
- 消耗大量计算资源，保证网络安全

2. 工作流程

- 收集未确认交易
- 组装区块
- 计算区块哈希
- 通过不断调整随机数（Nonce）尝试满足难度要求
- 首个解决问题的矿工获得记账权和奖励

#### Secp256k1椭圆曲线加密算法

1. 算法特点

- 基于椭圆曲线离散对数问题
- 比特币专用的特定曲线
- 安全性高，密钥长度较短

2. 加密原理

- 使用私钥生成公钥
- 通过数学运算确保私钥不可逆推导
- 支持数字签名和地址生成

3. 密钥特征

- 私钥：256位随机数
- 公钥：通过椭圆曲线点乘生成
- 地址：公钥哈希后Base58编码

这两个机制共同构成了比特币网络的安全基础，确保了交易的可靠性和网络的去中心化特性。











