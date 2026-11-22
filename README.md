# huicheng-anchor-ledger

汇澄天机·技能包防篡改**锚定账本**。本仓库只存指针与哈希，**不含任何技能包内容**。

## 它防什么

被锚定的包（`Liu5201988/huicheng-skillguard` 的 `mangpai-anchor` 分支，私有）自身有三层哈希校验，
加上本地裸库镜像共五层。最后一类残余攻击是「持凭据者强推远程、重写分支历史」。本账本把
每个锚定版本的 commit/tag 哈希登记在**另一个独立的、公开的、受分支保护的**仓库里：

- 公开仓库免费即可开分支保护：**禁强推、禁删分支、对管理员生效**；
- 强推私有分支想洗白，必须连本账本一起改写——而账本在本仓库的历史改不动，公开仓库的
  push 事件流也留痕；
- 账本内容只有哈希，不泄露包内容。

## 如何核验

```bash
# 取包侧锚点
git ls-remote https://github.com/Liu5201988/huicheng-skillguard.git | grep -E "mangpai|anchor"
# 与本仓库 anchors.json 逐条比对；对不上即说明私有分支被改写过
```

## 追加规则

每打一个新锚点版本，向 `anchors.json` 追加一条 entry 后正常 commit＋push（追加是快进推送，
不受保护影响）。禁止改写历史条目——本分支禁强推，改写会直接被拒。

## 条目

见 `anchors.json`。当前 1 条：v3.2.6。
