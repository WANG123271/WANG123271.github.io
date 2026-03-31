# Mapping_homework

姓名：王屹江
学号：2024012731
---

# Part I：Bowtie Mapping

## （1）BWT原理与内存优化

Bowtie的核心算法基于Burrows–Wheeler Transform（BWT）和FM-index，其高效性的本质来源于“压缩 + 可索引”的字符串结构。
首先，BWT通过对参考基因组的所有后缀进行字典序排序，并对其进行重排，使得具有相同前缀的序列在变换后呈现局部聚集。这一性质使得原本需要在整个基因组范围内进行的匹配问题，转化为在一个连续区间内进行搜索。
其次，Bowtie结合FM-index实现Backward search（后向搜索）：从read的末端开始逐字符匹配，每匹配一个碱基，候选区间都会收缩，从而避免了传统算法中的全局扫描或复杂回溯，使得匹配复杂度接近O(m)（m为read长度）。

在内存优化方面，Bowtie采用多种工程策略：

* 利用FM-index对基因组进行压缩存储，大幅降低内存占用
* 使用2-bit编码表示DNA序列（A/C/G/T），减少存储空间
* 采用稀疏后缀数组采样，仅存储部分位置信息，以空间换时间
* 构建正向和反向互补双索引，避免重复计算

这些策略使Bowtie在保证速度的同时，将内存需求控制在较低水平。

---

## （2）Mapping流程与reads统计

### 操作命令

```bash
cd /home/test/mapping
bowtie -v 2 -m 10 --best --strata BowtieIndex/YeastGenome -f THA2.fa -S THA2.sam
```

---

### 统计命令

```bash
cut -f3 THA2.sam | grep -v "*" | sort | uniq -c
```

---

## 结果

```text
     18 chrI
     51 chrII
     15 chrIII
    194 chrIV
     25 chrIX
     12 chrmt
     33 chrV
     17 chrVI
    125 chrVII
     68 chrVIII
     71 chrX
     56 chrXI
    169 chrXII
     67 chrXIII
     58 chrXIV
    101 chrXV
     78 chrXVI
      1 LN:1078177
      1 LN:1090940
      1 LN:1091291
      1 LN:1531933
      1 LN:230218
      1 LN:270161
      1 LN:316620
      1 LN:439888
      1 LN:562643
      1 LN:576874
      1 LN:666816
      1 LN:745751
      1 LN:784333
      1 LN:813184
      1 LN:85779
      1 LN:924431
      1 LN:948066
      1 SO:unsorted
      1 VN:1.0.0
```

---

## 结果分析

1. 绝大多数reads成功比对到酵母16条染色体（chrI–chrXVI）及线粒体（chrmt），说明测序数据质量较好且参考基因组匹配度较高。
2. chrIV（194条）和chrXII（169条）reads数量显著偏高，可能原因包括：
   * 这些染色体长度较长，包含更多可比对区域
   * 存在较多高表达或高拷贝序列
3. chrmt reads较少（12条），符合线粒体基因组较小的特点。
4. 输出中出现“LN:”和“VN:”等条目，是由于未过滤SAM文件header部分（以“@”开头），说明统计时混入了非比对数据。

改进方法：

```bash
grep -v "^@" THA2.sam | cut -f3 | sort | uniq -c
```

---

# Part II：SAM/BAM文件解析

## （3.1）CIGAR string

CIGAR（Compact Idiosyncratic Gapped Alignment Report）用于描述read与参考序列之间的比对关系，是SAM文件中最核心的字段之一。

它通过一系列“长度+操作符”的组合，记录比对过程中发生的各种事件，包括：

* M：匹配或错配
* I：插入（相对于参考序列）
* D：缺失
* N：跳跃（常见于RNA剪接）
* S：soft clipping
* H：hard clipping


CIGAR不仅反映了比对长度，还揭示了序列差异和结构变化信息。

---

## （3.2）Soft clip

Soft clip指的是read两端部分序列未参与比对，但仍保留在原始read序列中的情况。

其特点有不计入比对区域、仍保存在SEQ字段中、通常出现在read两端等等。在CIGAR中，其用“S”表示。产生原因包括1.测序接头污染 2.测序质量下降 3.结构变异（如断点）等

---

## （3.3）Mapping quality

Mapping Quality（MAPQ）用于衡量read比对结果的可信度，是一个基于概率的质量评分。

其定义为：
MAPQ = -10 log10(P错误比对概率)

含义是MAPQ越高，比对越可靠。而MAPQ=0，通常表示多重比对。

同时质量也有一些影响因素，比如比对唯一性，序列复杂度，错配数量等等……

因此，MAPQ本质上反映了“该read是否唯一且可靠地定位到参考基因组”。

---

## （3.4）是否能推断参考序列

仅根据SAM/BAM文件，无法完整还原参考基因组序列，但可以进行部分推断。

原因如下：

1. SAM文件提供POS（起始位置）和CIGAR（比对方式）
2. MD tag可以描述错配信息

因此可以推断差异位置、重建比对区域结构等。但不能获得完整参考序列。

所以，必须结合参考基因组FASTA文件才能完全还原序列。

---

# Part III：BWA Mapping

## （4）流程与原理

```bash
bwa index sacCer3.fa
bwa mem sacCer3.fa THA2.fa > THA2-bwa.sam
```

BWA同样基于BWT算法，但在比对策略上进行了改进：

* 支持插入和缺失（indel）
* 采用seed-and-extend策略
* 对长reads更加友好

---

## 结果分析

生成的SAM文件记录了reads的比对位置、匹配情况及质量信息。大部分reads能够成功比对，少数reads未比对或存在多重匹配，这与真实测序数据特征一致。

---

# Part IV：Bowtie vs BWA对比

## 算法层面

* Bowtie：严格基于BWT + 精确匹配（不支持indel）
* BWA：基于BWT + seed扩展（支持indel）

## 性能特点

* Bowtie：速度极快，内存占用低
* BWA：稍慢，但比对更准确

## 适用场景

* Bowtie：短reads、无indel情况（如简单DNA-seq）
* BWA：真实测序数据（存在插入缺失）

## 总结

Bowtie强调计算效率，适用于快速初筛；BWA在精度和灵活性方面更优，更适合实际生物数据分析任务。两者体现了生物信息学中“速度与准确性权衡”的典型思想。

---

