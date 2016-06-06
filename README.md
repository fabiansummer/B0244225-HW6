---
title: "1928-1969間，小兒麻痺在美國各州的發生率變化"
author: "B0244213"
date: "2016年5月16日"
output: ioslides_presentation
---
#1928-1969間，小兒麻痺在美國各州的發生率變化

## 資料前處理 
```{r results='hide',message=FALSE,cache=T}
#讀csv資料
polio<-read.csv("POLIO_Incidence.csv",stringsAsFactors = F)
head(polio)

#將寬表格轉為長表格，以年(YEAR)/週(WEEK)為基準，各州名column打散變長
library(reshape2) #for melt()
polio.m<-melt(polio,id.vars = c('YEAR','WEEK'))
head(polio.m)

polio.m[polio.m$value=="-",]$value<-NA #處理缺值,將"-"轉為NA
polio.m$value<-as.numeric(polio.m$value) #將value欄位轉為數字
polio.sumYear<- #各州各年度加總，計算該年度的總發生率
  aggregate(value~YEAR+variable,data=polio.m,FUN=sum,na.rm=F)
head(polio.sumYear)
```

##視覺畫呈現
```{r results='hide',message=FALSE,cache=T}
#安裝ggplot2 package
install.packages("ggplot2")
library(ggplot2)
myData<-qplot(data=polio.m,x=YEAR,y=value,color=variable,facets = ~variable)
#以年份作為X軸，發生率為Y軸，再利用不同的國家做分群
myData<- myData + stat_smooth(method="lm")
myData
```
