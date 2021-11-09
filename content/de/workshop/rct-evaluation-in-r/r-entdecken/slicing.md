---
# Title, summary, and page position.
linktitle: "Slicing & Indexing"
weight: 7
icon_pack: fas
date: "2021-01-25"

# Page metadata.
title: "Slicing & Indexing"
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

<iframe src="https://drive.google.com/file/d/1l0zw3xVi92drtC4YrP8ceI60inpuqLhY/preview" width="736" height="552" allow="autoplay"></iframe>


<br></br>

## Praxis-Teil

---


### Übungsaufgaben {#uebung}

Für die Übungsaufgaben muss der Übungsdatensatz `data` geladen sein, sowie das {tidyverse} package.

{{< highlight R >}}
library(tidyverse)
data <- read.csv("data/data.csv")
{{< / highlight >}}


**Anmerkung**: Die Übungsaufgaben werden zunehmend schwieriger. Vor allem für Anfänger ist es vollig normal, nicht alle Aufgaben auf Anhieb lösen zu können!

<br>

1. Log-transformiere die Variable `age` in `data` und speichere das Ergebnis unter dem Namen `age.log`.

<p>

2. Quadriere die Werte in `pss.1` und speichere das Ergebnis unter dem Namen `pss.1.squared`.

<p>

3. Berechne den Mittelwert und die Standardabweichung ($SD$) der Variable `cesd.2`. Nutze bei Bedarf das Internet um herauszufinden, welche Funktion in R die Standardabweichung berechnet.

<p>

4. Packe den Mittelwert und die Standardabweichung von `cesd.2` in eine Liste.

<p>


5. Hat die Variable `mbi.0` die passende Objektklasse `numeric`? Überprüfe dies mit R Code.

<p>

6. Lege im Dataframe `data` zwei neue Variablen an:
    - `age.50plus`, eine `logical`-Variable die mit `TRUE` und `FALSE` angibt, ob das Alter `age` einer Person $\geq$ 50 ist.
    - `pss.diff`, eine Variable die den Unterschied zwischen `pss.0` und `pss.1` für jede Person angibt.

<p>

7. Baue eine Pipe, die (1) alle männlichen Personen in der Interventionsgruppe herausfiltert; (2) die Variable `age` aus dem Dataframe herauszieht; und (3) den Mittelwert des ausgewählten Daten berechnet. Im Datensatz befindet sich die Gruppenzuordnung in `group`, wobei `1` für die Interventionsgruppe steht. Die Variable `sex` kodiert das Alter, wobei `1` für männlich steht.

<p>

8. Ändere den Wert von `ft.helps` in der dritten und vierten Zeile zu `NA`.

<p>


9. Zuletzt eine "harte Nuss": mit der `order` Funktion kann für Variablen ein Index gebildet werden. Dieser Index zeigt an, in welcher Reihenfolge die Elemente korrekt geordnet wären. Nutze die R Documentation (`?order`), um mehr über die Funktion zu erfahren. Nutze dann diese Funktion in einer eckigen Klammer, um `data` dem Alter `age` nach zu ordnen!




<br>

### Lösung


<br>

1. Log-transformiere die Variable `age` in `data` und speichere das Ergebnis unter dem Namen `age.log`.

{{< highlight R >}}
age.log <- log(data$age)
{{< / highlight >}}

---

&nbsp;

2. Quadriere die Werte in `pss.1` und speichere das Ergebnis unter dem Namen `pss.1.squared`.

{{< highlight R >}}
pss.1.squared <- data$pss.1^2
{{< / highlight >}}

---

&nbsp;

3. Berechne den Mittelwert und die Standardabweichung ($SD$) der Variable `cesd.2`. Nutze bei Bedarf das Internet um herauszufinden, welche Funktion in R die Standardabweichung berechnet.

{{< highlight R >}}
mean(data$cesd.2, na.rm = TRUE)
sd(data$cesd.2, na.rm = TRUE)
{{< / highlight >}}

---

&nbsp;

4. Packe den Mittelwert und die Standardabweichung von `cesd.2` in eine Liste.

{{< highlight R >}}
list(mean(data$cesd.2, na.rm = TRUE),
     sd(data$cesd.2, na.rm = TRUE))
{{< / highlight >}}

---

&nbsp;

5. Hat die Variable `mbi.0` die passende Objektklasse `numeric`? Überprüfe dies mit R Code.

{{< highlight R >}}
class(data$mbi.0)
{{< / highlight >}}

---

&nbsp;

6. Lege im Dataframe `data` zwei neue Variablen an:
    - `age.50plus`, eine `logical`-Variable die mit `TRUE` und `FALSE` angibt, ob das Alter `age` einer Person $\geq$ 50 ist.
    - `pss.diff`, eine Variable die den Unterschied zwischen `pss.0` und `pss.1` für jede Person angibt.

{{< highlight R >}}
data$age.50plus <- data$age >= 50
data$pss.diff <- data$pss.0 - data$pss.1
{{< / highlight >}}

---

&nbsp;

7. Baue eine Pipe, die (1) alle männlichen Personen in der Interventionsgruppe herausfiltert; (2) die Variable `age` aus dem Dataframe herauszieht; und (3) den Mittelwert des ausgewählten Daten berechnet. Im Datensatz befindet sich die Gruppenzuordnung in `group`, wobei `1` für die Interventionsgruppe steht. Die Variable `sex` kodiert das Alter, wobei `1` für männlich steht.

{{< highlight R >}}
data %>%
  filter(group == 1, sex == 1) %>%
  pull(age) %>%
  mean()
{{< / highlight >}}

---

&nbsp;

8. Ändere den Wert von `ft.helps` in der dritten und vierten Zeile zu `NA`.

{{< highlight R >}}
data[3:4, "ft.helps"] <- c(NA, NA)
{{< / highlight >}}

---

&nbsp;

9. Zuletzt eine "harte Nuss": mit der `order` Funktion kann für Variablen ein Index gebildet werden. Dieser Index zeigt an, in welcher Reihenfolge die Elemente korrekt geordnet wären. Nutze die R Documentation (`?order`), um mehr über die Funktion zu erfahren. Nutze dann diese Funktion in einer eckigen Klammer, um `data` dem Alter `age` nach zu ordnen!

{{< highlight R >}}
data[order(data$age),]
{{< / highlight >}}

---

<style>
h1 {color: #2a7792;}
</style>
