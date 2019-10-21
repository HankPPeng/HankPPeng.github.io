---
title: arrange data using R
author: ''
date: '2019-10-21'
slug: arrange-data-using-r
categories: []
tags:
  - Study
description: ''
externalLink: ''
series: []
---

&emsp;最近需要处理数据，数量很多，之前试着用matlab处理，不过速度太慢了，python是个好选择，不过pycharm这个IDE让我挠头，不知道debug这个命令实在头疼。因此，改用R，之前用R只是用来blogdown，殊不知其实是数据处理的高手，处理速度非常非常快！

&emsp;我主要用R处理以下方面：重新命名数据文件；导入数据；去除0值；导出数据。以我几个月前测量所得的数据文件做了个例子，写了以下代码：

```
# rename_files

# files_path
path <- setwd("E:/desktop/data/target1/rename")

# rename files
old_filename <- dir(path, pattern = "0[0-9]{2}.csv")
new_filename <- gsub("(00)([0-9]{1})(.csv)", "1st_00\\2\\3", old_filename)

# data_document
path <- setwd("E:/desktop/data/target1")

# data_accuracy
options(digits=3)

# load_data
file_name <- dir(path, pattern = '1st_00[0-9]{1}.csv')
data <- matrix(NA, 0, 0)
for (i in 1:length(file_name)) {
  data_temp <- read.csv(file_name[i])
  data <- rbind(data, data_temp[, -1])
}

# rename and del zero values
names(data)[1] <- "Impedance"
names(data)[2] <- "Angle"
data <- data[-which(data$Impedance == 0),]
Firstline <- 1:length(rowSums(data))
data <- data.frame(Firstline, data[1]-300, data[2])

# output data
write.csv(data, file = "0fir_data_arrange.csv", row.names = FALSE)
```

&emsp;上面代码将7个数据文件重新命名后就开始整理刚开始提出的要求，获得的效果非常好，非常快。之后就可以用R对数据整理，matlab建模了，哈哈~~

<div style="text-align: center">
<img src="https://raw.githubusercontent.com/HankPPeng/HankPeng.com/master/images/2019-19-21-figure1.png">
<img src="https://raw.githubusercontent.com/HankPPeng/HankPeng.com/master/images/2019-19-21-figure2.png">
<img src="https://raw.githubusercontent.com/HankPPeng/HankPeng.com/master/images/2019-19-21-figure3.png">
</div>
