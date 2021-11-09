---
# Title, summary, and page position.
linktitle: "Deskriptive Analyse & Dropout"
weight: 5
icon_pack: fas
date: "2021-01-25"

# Page metadata.
title: "Deskriptive Analyse & Dropout"
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


<iframe src="https://drive.google.com/file/d/1W12AjmHIs1lWHp8vfJhikx8ZOoGnwB7E/preview" width="736" height="552" allow="autoplay"></iframe>




<br></br>

## Praxis-Teil



### "Eyeballing" mit `skim`

{{< highlight R >}}
library(psych)
library(tidyverse)
library(skimr)

## Alle Daten:
skim(data)

## Interventionsgruppe:
data %>%
  filter(group == 0) %>%
  skim()

## Kontrollgruppe:
data %>%
  filter(group == 1) %>%
  skim()

## Histogramm des primären Outcomes (PSS-Stress)
multi.hist(data %>% select(pss.0, pss.1, pss.2), ncol = 3)
{{< / highlight >}}


<br>


### Dropout-Analyse

{{< highlight R >}}
## Gesamte Daten
with(data, {
  c(sum(is.na(pss.0)),
    sum(is.na(pss.1)),
    sum(is.na(pss.2)))
}) -> na.all

na.all.p <- na.all/nrow(data)

## Interventionsgruppe
data %>%
  filter(group == 1) %>%
  with({
    c(sum(is.na(pss.0)),
      sum(is.na(pss.1)),
      sum(is.na(pss.2)))
  }) -> na.ig

na.ig.p <- na.ig/nrow(data %>% filter(group == 1))

## Kontrollgruppe
data %>%
  filter(group == 0) %>%
  with({
    c(sum(is.na(pss.0)),
      sum(is.na(pss.1)),
      sum(is.na(pss.2)))
  }) -> na.cg

na.cg.p <- na.cg/nrow(data %>% filter(group == 0))

## Sammeln in Dataframe
na <- data.frame(na.all, na.all.p = na.all.p*100,
                 na.ig, na.ig.p = na.ig.p*100,
                 na.cg, na.cg.p = na.cg.p*100)

## Zeilennamen des Dataframe ändern
rownames(na) = c("t0", "t1", "t2")
na
{{< / highlight >}}



<style>
h1 {color: #2a7792;}
</style>
