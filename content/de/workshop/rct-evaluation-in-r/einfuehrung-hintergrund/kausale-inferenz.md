---
# Title, summary, and page position.
linktitle: "Kausale Inferenz"
weight: 2
icon_pack: fas
date: "2021-01-25"

# Page metadata.
title: "Kausale Inferenz"
type: book  # Do not modify.
---

<style>
code{
  color: #2a7792;
}
.hljs{
  font-size: 16px
}
.minih{
  font-size: 1px;
  margin: 0px 0px 0px 0px;
}

.highlight {
    position: relative;
}
.highlight pre {
    padding: 15px;
}
.highlight-copy-btn {
    position: absolute;
    top: 7px;
    right: 7px;
    border: 0;
    border-radius: 4px;
    padding: 5px;
    font-size: 0.7em;
    line-height: 1.8;
    color: #fff;
    background-color: #777;
    min-width: 55px;
    text-align: center;
}
.highlight-copy-btn:hover {
    background-color: #666;
}
</style>

---



## Foliensatz {.minih}

<iframe src="https://drive.google.com/file/d/117bRqbY9wjSy8M7jakcYDjBxtYTMldmF/preview" width="640" height="480" allow="autoplay"></iframe>


<br></br>



## Praxis-Teil



### Notwendige Funktionen

{{< highlight R >}}

library(tidyverse)
library(ggplot2)

randomize = function(blocksize, N){
  block = rep(1:ceiling(N/blocksize), each = blocksize)
  a1 = data.frame(block, rand=runif(length(block)), envelope= 1: length(block))
  a2 = a1[order(a1$block,a1$rand),]
  a2$arm = rep(c("Arm 1", "Arm 2"),times = length(block)/2)
  assign = a2[order(a2$envelope),]
  return(assign$arm)
}

plotter = function(m1, m0, mui = 45){
  plot.data <- data.frame(M = c(m1, m0), Gruppe = rep(c("Gruppe 1", "Gruppe 2"),
                                       each = length(m1)))
  ggplot(plot.data, aes(x = M, fill = Gruppe)) + geom_density(alpha = 0.5) +
    geom_vline(xintercept = mui, linetype = "dotted") + xlab(expression(hat(mu))) +
    ylab("") + theme_minimal()
}

{{< / highlight >}}


<br>


### Simulation ($k$=1)

{{< highlight R >}}

library(tidyverse)
library(ggplot2)

randomize = function(blocksize, N){
  block = rep(1:ceiling(N/blocksize), each = blocksize)
  a1 = data.frame(block, rand=runif(length(block)), envelope= 1: length(block))
  a2 = a1[order(a1$block,a1$rand),]
  a2$arm = rep(c("Arm 1", "Arm 2"),times = length(block)/2)
  assign = a2[order(a2$envelope),]
  return(assign$arm)
}

plotter = function(m1, m0, mui = 45){
  plot.data <- data.frame(M = c(m1, m0), Gruppe = rep(c("Gruppe 1", "Gruppe 2"),
                                       each = length(m1)))
  ggplot(plot.data, aes(x = M, fill = Gruppe)) + geom_density(alpha = 0.5) +
    geom_vline(xintercept = mui, linetype = "dotted") + xlab(expression(hat(mu))) +
    ylab("") + theme_minimal()
}
{{< / highlight >}}


<br>


### Simulation ($k$=100)

{{< highlight R >}}
# Wiederhole 1. 100-mal
set.seed(123)
for (i in 2:100){

  alter <- rnorm(10, 45, 15) %>% round()
  gruppe <- randomize(blocksize = 2, N = 10)

  m1[i] <- mean(alter[gruppe == "Arm 1"], na.rm = TRUE)
  m0[i] <- mean(alter[gruppe == "Arm 2"], na.rm = TRUE)
  m.diff[i] <- mean(alter[gruppe == "Arm 1"]) - mean(alter[gruppe == "Arm 2"])

}

mean(m.diff)
plotter(m1, m0)
{{< / highlight >}}


<br>


### Simulation ($k$=10.000)

{{< highlight R >}}
# Wiederhole 1. 10.000-mal
set.seed(123)
for (i in 101:10000){

  alter <- rnorm(10, 45, 15) %>% round()
  gruppe <- randomize(blocksize = 2, N = 10)

  m1[i] <- mean(alter[gruppe == "Arm 1"], na.rm = TRUE)
  m0[i] <- mean(alter[gruppe == "Arm 2"], na.rm = TRUE)
  m.diff[i] <- mean(alter[gruppe == "Arm 1"]) - mean(alter[gruppe == "Arm 2"])

}

mean(m.diff)
plotter(m1, m0)

{{< / highlight >}}


<style>
h1 {color: #2a7792;}
</style>
