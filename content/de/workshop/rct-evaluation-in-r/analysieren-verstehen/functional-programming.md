---
# Title, summary, and page position.
linktitle: "Functional Programming"
weight: 2
icon_pack: fas
date: "2021-01-25"

# Page metadata.
title: "Functional Programming"
type: book  # Do not modify.
---

<style>
code{
  color: #2a7792;
}
.hljs{
  font-size: 14px
}

</style>

---

## Slides

---

<iframe src="https://drive.google.com/file/d/117bRqbY9wjSy8M7jakcYDjBxtYTMldmF/preview" width="640" height="480" allow="autoplay"></iframe>


<br></br>

## Praxis-Teil

---


### Notwendige Funktionen



{{< highlight R >}}

library(tidyverse)
library(magrittr)
library(mice)
library(miceadds)
library(skimr)
library(mitml)
library(purrr)
library(writexl)


skimReport = function(data, round = FALSE){
  require(skimr)
  require(dplyr)
  require(purrr)
  x = skim(data)
  N = nrow(data)
  with(x, {
    skim_type == "factor" -> fac.mask
    vars = skim_variable[fac.mask]
    n.unique = factor.n_unique[fac.mask]
    strsplit(factor.top_counts, "\\, |\\:")[fac.mask] %>%
      purrr::map(function(x){
        as.numeric(x) -> x
        data.frame(category = x[seq(1, length(x), by = 2)],
                   count = x[seq(2, length(x), by = 2)]) %>%
          dplyr::mutate(percent = count/N)
      }) %>%
      {names(.) = vars;.} %>%
      map_df(~as.data.frame(.), .id = "variable")
  }) -> factors
  with(x, {
    skim_type == "numeric" -> num.mask
    data.frame(variable = skim_variable[num.mask],
               mean = numeric.mean[num.mask],
               sd = numeric.sd[num.mask],
               n = N-x$n_missing[num.mask],
               n.missing = n_missing[num.mask],
               perc.missing = n_missing[num.mask]/N)
  }) -> numerics
  if (round == TRUE){
    within(factors, {
      percent = round(percent*100, 2)
    }) -> factors
    within(numerics, {
      mean = round(mean, 2)
      sd = round(sd, 2)
      perc.missing = round(perc.missing*100, 2)
    }) -> numerics
  }
  dat = list(factors = factors,
             numerics = numerics)
  class(dat) = c("list", "skimReport")
  return(dat)
}

print.skimReport = function(x){
  cat("Categorial Variables \n ------------------ \n")
  cat("\n")
  print(x$factors)
  cat("\n")
  cat("Numeric Variables \n ------------------ \n")
  cat("\n")
  print(x$numerics)
}

{{< / highlight >}}


<br>

### Deskriptive Auswertung der Imputierten Daten


{{< highlight R >}}
# Konvertiere Imputationen (Klasse 'mids') in eine 'mitml'-Liste
implist <- mids2mitml.list(imp)
class(implist)

# Definiere kategorische Variablen
catvars = c("group", "sex", "ethn", "prevtraining", "prevpsychoth",
            "ft.helps", "rel", "degree", "inc", "child")

# In allen Imputationssets:
# Konvertiere alle definierten Variablen zum factor
implist %>%
  map(~mutate_at(., catvars, as.factor)) -> implist

# In allen Imputationssets:
# - Erstelle deskriptive Statistiken
# - Aggregiere alle numerischen Werte
implist %>%
  map(~skimReport(.)) -> descriptives

descriptives %>%
  map(~.$numerics[,-1]) %>%
  Reduce(`+`, .)/25 -> num.desc.full


# In der Interventionsgruppe:
# Aggregiere alle numerischen Werte
implist %>%
  map(~filter(., group == 1)) %>%
  map(~skimReport(.)) -> descriptives.ig

descriptives.ig %>%
  map(~.$numerics[,-1]) %>%
  Reduce(`+`, .)/25 -> num.desc.ig


# In der Kontrollgruppe:
# Aggregiere alle numerischen Werte
implist %>%
  map(~filter(., group == 0)) %>%
  map(~skimReport(.)) -> descriptives.cg

descriptives.cg %>%
  map(~.$numerics[,-1]) %>%
  Reduce(`+`, .)/25 -> num.desc.cg


# Extrahiere Variablennamen
variable = descriptives[[1]]$numerics[,1]


# Kombiniere alle Ergebnisse und speichere als Excel-Worksheet
cbind(variable, num.desc.full, num.desc.ig, num.desc.cg) %>%
  write_xlsx("imp_numeric_descriptives.xlsx")
{{< / highlight >}}
