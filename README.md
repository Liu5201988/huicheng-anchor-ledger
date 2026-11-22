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

## 灾难恢复规程（2026-11-22 演练后补入）

实测：从 GitHub 克隆 `mangpai-anchor` 可恢复 244 文件、HEAD 与锚点一致（浅克隆即可），
但克隆内 `run_all.py --gate` 首跑 18/21（check_refs/refs_md_fresh/run_tools_test 未过）——
清单与引用校验含环境相关状态，**恢复后不是开箱全绿**。标准恢复序列：

1. `git clone`（浅克隆可，做取证核验时用完整克隆＋比对 tag 对象）；
2. `python tools/run_all.py --refresh` 重生成清单；
3. `python tools/run_all.py --gate` 复跑须 21/21；
4. **向本账本追加新 entry**：记录恢复后清单 md5 与全部 data_hashes 现算值（禁抄旧值——
   v3.2.6 entry 曾因抄旧值致指针失效，v3.2.6-r2 已修正并实练本流程）；
5. 在恢复副本上重打 commit＋tag，推送 `mangpai-anchor`。

## 条目

见 `anchors.json`。当前 2 条：v3.2.6、v3.2.6-r2（修正 PR 哈希，append-only）。
