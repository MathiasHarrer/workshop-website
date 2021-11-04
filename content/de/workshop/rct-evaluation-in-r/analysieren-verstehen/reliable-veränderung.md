---
# Title, summary, and page position.
linktitle: "Reliable Veränderung"
weight: 5
date: "2021-01-25"

# Page metadata.
title: "Reliable Veränderung"
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


<iframe src="https://drive.google.com/file/d/1vEFbxXIPnLlSLhk3xdHK6ocYVY1q-SNg/preview" width="736" height="552" allow="autoplay"></iframe>


<br></br>

## Praxis-Teil

---


### Berechung des Reliable Change Index

{{< highlight R >}}
# Wir nutzen die 'rci'-Funktion, um den Reliable Change Index zu berechnen
rci <- function(y0, y1, ryy){

  diff = y1-y0
  sdiff = sqrt(2*((sd(y0)*sqrt(1-ryy))^2))
  return(diff/sdiff)

}

# Ausprobieren in einem Imputationsset
# Wir nehmen eine Reliabilität von ryy = 0.91 an
with(implist[[1]], rci(y0 = pss.0, y1 = pss.1, ryy = 0.91))

# Berechnung in allen Imputationssets mit 'purrr'
implist %>%
  map(function(x){

    # Berechne RCI für jede Person
    x$rci = with(x, rci(y0 = pss.0, y1 = pss.1, ryy = 0.91))

    # Berechne Reliable Improvement (RCI <= -1.96)
    x$ri = ifelse(x$rci <= -1.96, 1, 0)

    # Berechne Reliable Deterioriation (RCI >= 1.96)
    x$rd = ifelse(x$rci >= 1.96, 1, 0)

    # Return x
    x

  }) -> implist


# Berechnung der Anzahl/Raten von Reliable Improvement
# Dies kann über die 'table' Funktion erreicht werden
with(implist[[1]], table(group, ri))

# Berechnung in allen Imputationssets
# Wir runden die Fallzahlen am Ende. Dies dient nur dazu,
# diese später zu berichten.
implist %>%
  map(~ with(., table(group, ri))) %>%
  {Reduce(`+`, .)/25} %>%
  as.matrix() %>%
  round() -> table.ri
{{< / highlight >}}


<br>

### $\chi^2$-Test

{{< highlight R >}}
# Rechne zuerst mit gepooleten Ergebnissen, um sich mit der
# Funktion vertraut zu machen:
chisq.test(table.ri)

# Berechne Chi-Quadrat-Werte in allen Sets
implist %>%
  map_dbl(function(x){

    table <- with(x, table(group, ri))
    chisq <- chisq.test(table)$statistic; chisq

  }) -> chisq

# Aggregiere die Werte
micombine.chisquare(dk = chisq, df = 1)
{{< / highlight >}}



<br>

### Number Needed to Treat

{{< highlight R >}}
# Berechne n in Interventions- und Kontrollgruppe
n.ig <- sum(table.ri[2,])
n.cg <- sum(table.ri[1,])

# Berechne Anteil von reliable Improvement
p.ig <- table.ri[2,2]/n.ig
p.cg <- table.ri[1,2]/n.cg

# Berechne NNT als inverse Risikodifferenz
nnt <- (p.ig - p.cg)^-1
{{< / highlight >}}


<style>
h1 {color: #2a7792;}
</style>
