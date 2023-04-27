---
# Title, summary, and page position.
linktitle: "Selection Models"
weight: 4
icon_pack: fas
date: "2022-01-25"

# Page metadata.
title: "Selection Models"
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


## Slides {.minih}

<object data="/media/workshop/ma/selection.pdf" type="application/pdf" width="100%" height="500px">
</object>

**Keywords**: _P_-Curve, Uniform Distribution, _P_-Hacking

<br></br>

## Code

---

{{< highlight R>}}
# Install dmetar:
if (!require("remotes")) {
  install.packages("remotes")
}; remotes::install_github("MathiasHarrer/dmetar")

# Install metasens
install.packages("metasens")

# Load all required packages
library(dmetar)
library(meta)
library(metasens)

# Load 'ThirdWave' data set
data("ThirdWave")

# Run REM meta-analysis
rem <- metagen(TE, seTE, Author, ThirdWave)

# Create a funnel plot
funnel(rem)

# Run Egger's test
eggers.test(rem)

# Run limit meta-analysis
limitmeta(rem)
funnel(limitmeta(rem))

# Run P-curve
pcurve(rem)
{{< /highlight >}}


<style>
h1 {color: #2a7792;}
</style>


