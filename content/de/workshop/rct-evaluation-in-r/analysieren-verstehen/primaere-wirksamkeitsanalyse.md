---
# Title, summary, and page position.
linktitle: "Primäre Wirksamkeitsanalyse"
weight: 3
date: "2021-01-25"

# Page metadata.
title: "Primäre Wirksamkeitsanalyse"
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


### Analysis of Covariance

{{< highlight R >}}

m.ancova <- lm(pss.1 ~ 1 + group + scale(pss.0), data = implist[[1]])
summary(m.ancova)

# Prüfe Voraussetzungen:
# - Varianzhomogenität: gleichförmige Streuung der Residuen um 0
#   (-> Residuals vs. Fitted Plot)
# - Normalverteilung der Residuen (-> Q-Q Plot)

# Residuals vs. Fitted
plot(m.ancova, 1)

# Q-Q Plot
plot(m.ancova, 2)

# Analysis of Variance des Modells
anova(m.ancova)

# Zum Vergleich: Fit ohne Kovariate
lm(pss.1 ~ 1 + group, data = implist[[1]]) %>%
  anova()


{{< /highlight >}}

<br>

### ANCOVA in Multipel Imputierten Daten

{{< highlight R >}}

# Liste muss Klasse "mitml.list" besitzen!
class(implist) = c("mitml.list", "list")

# Fitte ANCOVA-Modell in allen MI-Sets
with(implist, lm(pss.1 ~ 1 + group + pss.0)) %>%
  testEstimates() -> mi.ancova

# Extrahiere F-Werte
with(implist, lm(pss.1 ~ 1 + group + pss.0)) %>%
  map_dbl(~anova(.)$`F value`[1]) -> Fvalues

# Kombiniere F-Werte mit Rubin-Regeln
micombine.F(Fvalues, 1)


{{< /highlight >}}

<br>

### ANCOVA bei sekundären Endpunkten

{{< highlight R >}}

# Definiere Namen alles zu analysierenden Variablen
# (ohne Zeitangabe)
test.vars = c("pss", "cesd", "hadsa",
              "isi", "mbi", "pswq")

# Generiere Modellformeln für Post- und FU-Messzeitpunkt
formula.1 = paste(test.vars, ".1 ~ 1 + group + ", test.vars, ".0",
                  sep = "")
formula.2 = paste(test.vars, ".2 ~ 1 + group + ", test.vars, ".0",
                  sep = "")

# Nutze "map", um Funktion über alle _Formeln_(!) anzuwenden:
# 1. Berechne ANCOVA für spezifisches Outcome, unter Kontrolle der
#    Baselinemessung
# 2. Extrahiere Ergebnisse des Gruppenterms
# 3. Extrahiere F-Werte und aggregiere mit Rubin-Regeln
# 4. Gebe generiertem data.frame neue Variablennamen
# 5. Füge Spalten für Variable und MZP hinzu
as.list(c(formula.1, formula.2)) %>%
  map_dfr(function(x){

    with(implist, lm(as.formula(x))) %>%
      testEstimates() -> res
    res$estimates[2,] -> res.group

    with(implist, lm(as.formula(x))) %>%
      map_dbl(~anova(.)$`F value`[1]) -> Fvalues
    micombine.F(Fvalues, 1, display = FALSE) -> Fvalue.mi

    c(res.group, Fvalue.mi)
  }) %>%
  set_colnames(c("md", "SE", "t", "df.mi",
                 "p.t", "RIV", "FMI", "F", "p.F",
                 "df.1", "df.2")) %>%
  bind_cols(variable = rep(test.vars, 2),
            time = rep(1:2, each = 6), .) -> mi.ancova.full

# Speichere Ergebnisse als Excel-Sheet
write_xlsx(mi.ancova.full, "mi_ancova_full.xlsx")


{{< / highlight >}}
