# Iris 数据分析上机任务

## 一、环境准备

### Docker + R

1. 拉取镜像：

```bash
docker pull r-base
```

2. 启动容器：

```bash
docker run -it --name r_env r-base bash
```

3. 进入 R：

```r
R
```

---

## 二、iris 数据集基本信息

### 代码

```r
data(iris)
str(iris)
```

### 输出结果

```
'data.frame':   150 obs. of  5 variables:
 $ Sepal.Length: num
 $ Sepal.Width : num
 $ Petal.Length: num
 $ Petal.Width : num
 $ Species     : Factor w/ 3 levels "setosa","versicolor","virginica"
```

### 结论

* iris 数据集共有 5 列
* 列的数据类型：

  * Sepal.Length: numeric
  * Sepal.Width: numeric
  * Petal.Length: numeric
  * Petal.Width: numeric
  * Species: factor

---

## 三、按 Species 分组计算 Sepal.Length 的均值和标准差

### 代码

```r
result <- aggregate(Sepal.Length ~ Species, data=iris,
                    FUN=function(x) c(mean=mean(x), sd=sd(x)))

result_expanded <- data.frame(
  Species = result$Species,
  Mean = result$Sepal.Length[,1],
  SD = result$Sepal.Length[,2]
)

print(result_expanded)
write.csv(result_expanded, "sepal_length_stats.csv", row.names=FALSE)
```

### 输出结果 / CSV 内容

```
Species,Mean,SD
setosa,5.006,0.3524897
versicolor,5.936,0.5161711
virginica,6.588,0.6358796
```

### CSV 拷贝到桌面

```bash
# Windows PowerShell
docker cp r_env:/sepal_length_stats.csv "$Env:USERPROFILE\Desktop\sepal_length_stats.csv"
```

---

## 四、不同 Species 的 Sepal.Width One-way ANOVA 分析

### 代码

```r
anova_result <- aov(Sepal.Width ~ Species, data=iris)
summary(anova_result)
```

### 输出结果

```
             Df Sum Sq Mean Sq F value Pr(>F)
Species       2  11.35   5.672   49.16 <2e-16 ***
Residuals   147  16.96   0.115
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1
```

### 结论

* p-value < 0.001，说明不同 Species 的 Sepal.Width 存在显著差异。

---

## 五、总结

1. 数据集 iris 有 5 列，类型为 numeric / factor。
2. 按 Species 分组，Sepal.Length 均值和标准差计算完成，CSV 文件已生成并拷贝到桌面。
3. Sepal.Width 的 One-way ANOVA 显示不同物种间存在显著差异。
4. 所有步骤可在 Docker + R 环境中复现。
