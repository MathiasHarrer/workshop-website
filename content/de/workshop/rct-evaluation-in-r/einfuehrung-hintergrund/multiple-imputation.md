---
# Title, summary, and page position.
linktitle: "Multiple Imputation"
weight: 95
icon_pack: fas
date: "2021-01-25"

# Page metadata.
title: "Multiple Imputation"
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


<iframe src="https://drive.google.com/file/d/1GkEjXt17_Ot0ihwrNSB4WFuyqZkLkZSl/preview" width="736" height="552" allow="autoplay"></iframe>


<br></br>

## Praxis-Teil



### Missing Data Pattern

{{< highlight R >}}
md.pattern(data, rotate.names = TRUE)

library(mice)
library(miceadds)
library(tidyverse)
library(plot.matrix)
library(mitml)
{{< / highlight >}}


<br>


### Influx-Outflux-Plot

{{< highlight R >}}
flux(data)
fluxplot(data)
{{< / highlight >}}


<br>


### Ausschluss von Variablen

{{< highlight R >}}
imp0 <- mice(data, maxit = 0)
imp0$loggedEvents

outlist <- c(imp0$loggedEvents$out, "sess", "zuf.1")

imp.data <- data %>% select(-all_of(outlist))
{{< / highlight >}}


<br>


### Imputationsmatrix

{{< highlight R >}}
# Definiere eine "Inlist" (zwingend einzuschließende Variablen)
inlist <- c("sex", "age", "ethn", "child", "prevpyschoth", "ft.helps",
            "prevtraining", "rel", "degree", "inc", "pss.0")

# - mincor (Minimale Interkorrelation) = 0.01
# - minpuc (Proportion of Usable Cases) = 0.1
pred <- quickpred(imp.data,
                  mincor = 0.05,
                  minpuc = 0.1,
                  inc = inlist)

# Mittlere Anzahl an Prädiktoren
rowsums <- table(rowSums(pred))
rowsums

rowsums[-1] %>%
  {as.numeric(names(.)) %*% . / sum(.)}


# Setze Prädiktor "group" auf 0
pred[,"group"] = 0


# Visualisiere die Prädiktormatrix
plot(pred, main = "Imputation Matrix",
     col = c("grey", "blue"),
     xlab = "predicts",
     ylab = "is predicted",
     las = 2, cex.axis = 0.5)


# Nochmalige Nullimputation; zeige gewählte Methode
imp0 = mice(imp.data, maxit = 0,
            predictorMatrix = pred)
imp0$method


# Finde Variablen ohne Missings
no.missings = imp0$method == ""

# Setze Imputationsmethode auf "bygroup"
imp0$method %>%
  replace(., . != "", "bygroup") -> imp.method

# Definiere Imputationsfunktion für "bygroup" variablen
imp0$method[!no.missings] %>% as.list() -> imp.function

# Definiere Liste mit Gruppenvariable
rep("group", length(imp.method[!no.missings])) %>%
  set_names(names(imp.method[!no.missings])) %>%
  as.list() -> imp.group.variable
{{< / highlight >}}


<br>


### Imputation


{{< highlight R >}}
set.seed(123)
mice(imp.data,
     predictorMatrix = pred,
     method = imp.method,
     imputationFunction = imp.function,
     group = imp.group.variable,
     m = 25, maxit = 25) -> imp

save(imp, file = "data/imp.rda")

{{< /highlight >}}


<br>


### Diagnostik

{{< highlight R >}}
# Trace plots
plot(imp,
     layout = c(4, ceiling(sum(!no.missings)/2)))

# Kernel densities
densityplot(imp, ~ pss.1 + pss.2)
densityplot(imp, ~ pss.1 + pss.2 | as.factor(group))
{{< /highlight >}}


<style>
h1 {color: #2a7792;}
</style>
