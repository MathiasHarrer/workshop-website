---
# Title, summary, and page position.
linktitle: Vorbereitung
weight: 1
# icon: book
icon_pack: fas

# Page metadata.
title: Vorbereitung
type: book  # Do not modify.
---

---

<style>
code{
  color: #2a7792;
}
.hljs{
  font-size: 16px
}
h1 {color: #2a7792;}
</style>

<br>

## R & RStudio


Für den Workshop ist eine aktuelle Installation von R (Version 4.0.0 oder größer) und RStudio auf dem PC notwendig. Beide Programme können hier heruntergeladen werden:

- **R**: Download der Version für [Windows](https://cran.r-project.org/bin/windows/base/) oder [macOS](https://cran.r-project.org/bin/macosx/).

- **RStudio**: Download der kostenlosen Version von [RStudio Desktop](https://www.rstudio.com/products/rstudio/).

Nachdem beide Programme installiert wurden, sollte kurz geprüft werden, ob RStudio "startbereit" ist. Dazu muss einfach RStudio geöffnet werden; wird in der Console (unten links) die heruntergeladene R Version angezeigt (zusammen mit Lizenz- und anderen Basisinformationen), kann R auf dem Computer genutzt werden.

Mehr Information zur Installation von R/RStudio finden sich [hier](https://bookdown.org/MathiasHarrer/Doing_Meta_Analysis_in_R/discovering-R.html#install-R).

<br>

## Vorbereitung des Analysenordners


Um die Praxisbeispiele während des Workshops selbst durchlaufen zu können, sollte zuvor bereits ein **Ordner** mit allen wichtigen Materialen zusammengestellt werden. Dieser Ordner sollte an einem gut auffindbaren Ort abgespeichert werden, und einen aussagekräftigen Namen tragen (z.B. `rct-workshop`).

Während des Workshops werden neue Daten generiert, die dann ebenfalls in dem erstellten Ordner abgespeichert werden. Zu Beginn sollten jedoch bereits diese Materialen abgespeichert sein:

- 📄 `script.R` (Leeres Skript für geschriebenen Code)
- 📁 `data`
  - 📄 `data.csv` (Datensatz für die Praxisbeispiele)
  - 📄 `imp.rda` (Imputierter Datensatz)

Dieser Ordner kann [hier](/workshop/rct-evaluation-in-r/vorbereitung/rct-workshop.zip) im zip-Format heruntergeladen werden. Zudem können alle Datensätze unter dem Reiter [Material](/workshop/rct-evaluation-in-r/material) eingesehen und heruntergeladen werden.


<br>

## "Bring Your Data"


Der Workshop und alle seine Praxisbeispiele sind auf den o.g. Datensatz zugeschnitten. Besonders R-Anfängern möchten wir ans Herz legen, den Workshop mit diesen bereitgestellten Daten zu durchlaufen.

Es besteht jedoch auch die Möglichkeit, Übungen und Praxisbeispiele mit einem eigenen RCT-Datensatz auszuprobieren. Dafür sollten jedoch Folgendes beachtet werden:

- **Der Datensatz sollte nach den Grundsätzen von "tidy data" vorstrukturiert sein**. Für eine Einführung in das "tidy data"-Konzept siehe Wickham ([2014](https://www.jstatsoft.org/article/view/v059i10)). Zudem sollte ein Codebook erstellt werden; diesem Codebook folgend sollten alle Variablennamen konsistent formatiert sein. Ein besonderes Augenmerk liegt dabei auf der Kodierung des Messzeitpunktes; bespielsweise sollten Messzeitpunktangaben über alle Variablen konsistent sein (z.B. `depression_t1`, `depression_t2`, `depression_t3` für Depression zu Baseline, Post-Test und Follow-up). Ein Beispiel für einen Coding Guide findet sich [hier](/data-warehouse/coding-guide/); der Beispieldatensatz `data.csv` ist nach diesen Richtlinien aufbereitet.

- **Individuelles Troubleshooting ist nur eingeschränkt möglich**. Verfahren wie z.B. multiple Imputation müssen flexibel auf die vorliegenden Daten adaptiert werden; eine "one size fits all"-Lösung existiert hierfür nicht. Dies bedeutet, dass im Workshop erarbeiteter Code sich oft nicht automatisch und ohne Abänderungen auf das eigene Datenset übertragen lässt. Während des Workshops besteht nur begrenzt die Möglichkeit, auf Fehlermeldungen, Konvergenzprobleme etc. bei "mitgebrachten" Daten einzugehen. Im Notfall besteht jederzeit die Möglichkeit, auf die bereitgestellten Daten "umzusatteln", und sich nach dem Workshop mit der nötigen Muße dem eigenen Datensatz zu widmen.


<br>

## Checkliste


Hier zusammenfassend eine Checkliste der Dinge, die für den Workshop vorbereitet werden müssen:

- [x] Installation von R
- [x] Installation von Rstudio
- [x] Anlegen des Analysenordners auf dem PC
- [ ] Vorbereitung eigener Daten (_optional_)

<br>
