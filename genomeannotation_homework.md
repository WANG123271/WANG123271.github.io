# 人类基因组大小、基本组成与非编码RNA注释概述
王屹江 2024012731

---

## 1. 人类基因组的大小以及基本组成

### 1.1 人类基因组大小

人类单倍体基因组（haploid genome）的总大小约为 **3.1 Gb（gigabase）**，即约 **31 亿碱基对**。  
若按二倍体细胞计算，总 DNA 量约为单倍体的两倍，但通常在基因组学语境中，所说“人类基因组大小”指的是**单倍体参考基因组长度**。

### 1.2 基本组成

人类基因组并非“蛋白编码序列为主”，而是由以下部分构成：

<div align="center">表1.人类基因组组成部分</div>
| 组成部分 | 典型特征 | 说明 |
|---|---:|---|
| 蛋白质编码区（protein-coding exons） | 约占 1%–2% | 真正翻译成蛋白的序列比例很小 |
| 内含子（introns） | 占较大比例 | 位于基因内部，不直接编码蛋白 |
| 基因间区（intergenic regions） | 占较大比例 | 基因之间的非编码区域 |
| 重复序列（repetitive elements） | 约占约一半 | 包括 LINE、SINE、LTR、DNA transposon 等 |
| 调控序列（regulatory elements） | 分散存在 | 启动子、增强子、沉默子、绝缘子等 |
| 非编码 RNA 基因（ncRNA genes） | 数量较多 | 包括 miRNA、lncRNA、snRNA、snoRNA 等 |

### 1.3 

人类基因组的最显著特征之一是：**蛋白编码序列只占极少部分，而大量序列属于非编码区和重复序列**。这些区域虽然不直接翻译蛋白，但在基因表达调控、染色质结构维持、RNA 加工以及发育和疾病过程中具有重要作用。

---

## 2. 基因中的非编码 RNA 最新注释数量

> **重要说明**：  
> 不同数据库对“非编码 RNA”的定义不同，统计口径也不同，常见差异来自：  
> 1. 是否只统计“已人工审校（curated）”条目；  
> 2. 是否包含假基因相关转录本；  
> 3. 是否把某些小 RNA 归入更细分类；  
> 4. 是以 **NCBI Gene**、**Ensembl/GENCODE** 还是 **RNAcentral** 为准。

### 2.1 采用的统计口径

由于不同数据库（GENCODE、Ensembl、miRBase、RNAcentral）对非编码RNA的定义和统计标准不同，因此各类型数量存在一定差异。本作业以 GENCODE Release 49（2025）为主要参考标准。

### 2.2 
<div align="center">表2.非编码 RNA 的主要细分类型</div>
| 非编码 RNA 类型 | 数量 | 主要功能 |
|---|---:|---|
| miRNA | 1,879（GENCODE v49）或 ~2,600（miRBase） | 通过与靶 mRNA 配对，抑制翻译或促进 mRNA 降解，参与发育、分化和肿瘤调控。 |
| lncRNA | 35,899 | 长度通常 >200 nt，不编码蛋白，常通过染色质调控、转录调控或 RNA 互作影响基因表达。 |
| snoRNA | 942 | 主要参与 rRNA、snRNA 等的化学修饰，指导 2'-O-甲基化或假尿苷化。 |
| snRNA |1,901 | 是剪接体的重要组成部分，参与 pre-mRNA 剪接。 |
| rRNA | 47（核基因）+ 2（线粒体）≈ 49 | 核糖体的核心结构和催化成分，负责蛋白质合成。 |
| tRNA | 22（线粒体）+ 预测数百（~600，Ensembl） | 将特定氨基酸转运到核糖体，并通过反密码子识别 mRNA 密码子。 |
| piRNA | >10,000（数据库注释级） | 主要在生殖细胞中抑制转座子，维持基因组稳定性。 |
| scaRNA | 49 | 主要定位于 Cajal body，参与 snRNA 的加工与修饰。 |
| SRP RNA / 7SL RNA | 5 | 参与信号肽识别颗粒，介导新生肽链向内质网转运。 |
| Y RNA | ~4 | 参与 DNA 复制、RNA 稳定性及核糖核蛋白复合物相关过程。 |
| vault RNA | 4 | 与 vault 复合体相关，可能参与 RNA 调控和细胞应答。 |
| lincRNA / antisense RNA / sense intronic RNA 等 | 已包含在 lncRNA（35,899）中 | 属于长链非编码 RNA 的不同亚类，常参与转录调控和染色质调节。 |

注：以上统计主要基于 GENCODE Release 49（2025，GRCh38.p14），其中 small ncRNA 总数为 7,563，lncRNA 为 35,899。不同数据库统计口径存在差异，例如 miRNA 在 GENCODE 中为 1,879，而 miRBase 收录约 2,600 条。
根据 GENCODE Release 49（2025）统计，人类基因组中非编码 RNA 基因总数约为：
35,899（lncRNA） + 7,563（small ncRNA） ≈ 43,000+。


### 2.3 主流非编码 RNA 的功能概述

#### miRNA
miRNA 是最经典的小分子非编码 RNA，通常通过结合靶 mRNA 的 3'UTR 抑制其表达。它们在细胞增殖、分化、凋亡和疾病发生中具有广泛调节作用。

#### lncRNA
lncRNA 通常长度超过 200 个核苷酸，不编码蛋白，但可作为支架、诱饵、向导或信号分子参与调控。其功能高度多样，常涉及染色质状态和转录网络调节。

#### snoRNA
snoRNA 的经典功能是指导 rRNA 和 snRNA 的加工修饰，是核仁中 RNA 修饰系统的重要组成部分。

#### snRNA
snRNA 参与前体 mRNA 剪接，是剪接体的核心组分之一，对转录后加工至关重要。

#### rRNA 和 tRNA
rRNA 与 tRNA 是蛋白质翻译机器的核心分子，前者构成核糖体骨架并参与催化，后者负责氨基酸递送和密码子识别。

---

## 3. 我们可以用图1概括：

### <p align="center">图1：人类基因组组成示意图

```text
基因组总长度
├── 蛋白编码区（1%–2%）
├── 内含子
├── 基因间区
├── 重复序列
└── 非编码 RNA 基因
```

### 数据来源：

1. GENCODE Release 49 (2025, GRCh38.p14)
   https://www.gencodegenes.org/human/stats.html

2. Ensembl Release 115 (2025)
   https://www.ensembl.org

3. miRBase (latest release)
   https://www.mirbase.org

4. RNAcentral database
   https://rnacentral.org


