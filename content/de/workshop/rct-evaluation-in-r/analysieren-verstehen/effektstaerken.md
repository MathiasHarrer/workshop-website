---
# Title, summary, and page position.
linktitle: "Effektstärken"
weight: 8
icon_pack: fas
date: "2021-01-25"

# Page metadata.
title: "Effektstärken"
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



<iframe src="https://drive.google.com/file/d/1b2HtJddCwBPLe6KxH5Y2ZpZ0r8MOkT0R/preview" width="736" height="552" allow="autoplay"></iframe>


<br></br>

## Praxis-Teil

---


### Reshaping


{{< highlight R >}}
library(tidyverse)
library(purrr)
library(mitml)

# Anzahl der Imputationssets
m <- length(implist)

# Um within-Effektstärken zu berechnen, empfiehlt sich ein
# reshaping der Daten ins long-Format.
# - pivot_longer is hierfür eine gute Option.
# - wir fokussieren hier auf den prä-post-Unterschied.
implist %>%
  map(function(x){
    x %>%
      mutate(id = 1:nrow(.)) %>%
      select(id, group, pss.0, pss.1) %>%
      pivot_longer(c(pss.0, pss.1), names_prefix = "pss.",
                  names_to = "time", values_to = "pss")
  }) -> implist.long
{{< / highlight >}}


<br>

### Berechnung von $s_{\text{pooled}}$ ("agnostisch")

{{< highlight R >}}
# Zur Berechnung mit der "agnostischen" Methode benötigen wir nur
# die Standardabweichungen zu beiden Zeitpunkten.
# Wir fokussieren hier auf den Effekt der Interventionsgruppe
implist.long %>%
  map_dfr(function(x){
    x %>% filter(group == 1 & time == 0) -> pre
    x %>% filter(group == 1 & time == 1) -> post
    data.frame(sd(pre$pss), sd(post$pss))
  }) %>%
  colSums()/m -> sd.pre.post

sd.pooled.prepost <- sqrt((sd.pre.post[1]^2 + sd.pre.post[2]^2)/2)
sd.pooled.prepost

## > sd.pre.pss.
## >    5.313179
{{< / highlight >}}


<br>

### Berechung von $s_{\text{pooled}}$ ("modellbasiert")

{{<highlight R>}}
# Durchführung einer vollständigen Varianzdekomposition
# (repeated-measures ANOVA) in alle MI-Sets
implist.long %>%
  map(function(x){
    x %>%
      filter(group == 1) %>%
      aov(pss ~ time + Error(id), .) %>%
      {.[["Within"]]}
  }) -> m.rmanova.mi

# Berechnung von s_pooled aus den Within-Residualen des Modells
# (Standardfehler der Residuale)
m.rmanova.mi %>%
  map_dbl(~sqrt(sum(.$residuals^2)/.$df.residual)) %>%
  mean() -> sd.pooled.model

sd.pooled.model
## > 5.320216

# Die Ergebnisse der beiden Methoden sind bei diesem Beispiel
# nahezu identisch!
{{< / highlight >}}

<br>

### Berechnung der Within-Effektstärke

{{<highlight R>}}
testEstimates(m.rmanova.mi)
testEstimates(m.rmanova.mi) %>% confint()

#> Final parameter estimates and inferences obtained from 25 imputed data sets.
#>
#>        Estimate Std.Error   t.value        df   P(>|t|)       RIV       FMI
#> time1    -8.373     0.686   -12.211  3128.075     0.000     0.096     0.088
#>           2.5 %    97.5 %
#> time1 -9.717112 -7.028342

# Berechne die within-Effektstärke (wir nutzen hier sd.pooled.model)

-8.373/sd.pooled.model  # Within-Cohen's d
-9.717/sd.pooled.model  # Unteres Ende d. Konfidenzintervalls
-7.028/sd.pooled.model  # Oberes Ende d. Konfidenzintervalls

#> [1] -1.573808
#> [1] -1.82643
#> [1] -1.320999
{{< / highlight >}}

<style>
h1 {color: #2a7792;}
</style>
