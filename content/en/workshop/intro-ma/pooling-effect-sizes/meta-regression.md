---
# Title, summary, and page position.
linktitle: "Subgroup Analysis & Meta-Regression"
weight: 5
icon_pack: fas
date: "2022-01-25"

# Page metadata.
title: "Subgroup Analysis & Meta-Regression"
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

<object data="/media/workshop/ma/meta-regression.pdf" type="application/pdf" width="100%" height="500px">
</object>

**Keywords**: Fixed-Effects (Plural) Model, Ecological Fallacy


<br></br>

## Code

---

{{< highlight R >}}
library(meta)

# Copy and paste the entire code below into your Console.
# Then hit Enter.
thirdwave <- 
  structure(
     list(
            study = c("Call", "Cavanagh", "Danitz-Orsillo", 
                      "de Vibe", "Frazier", "Frogeli", 
                      "Gallego", "Hazlett-Stevens", "Hintz", 
                      "Kang", "Kuhlmann", "Lever Taylor", 
                      "Phang", "Rasanen", "Ratanasiripong", 
                      "Shapiro", "Song", "Warnecke"), 
            es = c(0.70, 0.35, 1.79, 0.18, 0.42, 0.63, 
                   0.72, 0.52, 0.28, 1.27, 0.10, 0.38, 
                   0.54, 0.42, 0.51, 1.47, 0.61, 0.60), 
            se = c(0.260, 0.196, 0.345, 0.117, 0.144, 
                   0.196, 0.224, 0.210, 0.168, 0.337, 
                   0.194, 0.230, 0.244, 0.257, 0.351, 
                   0.315, 0.226, 0.249), 
            rob = c("high", "low", "high", "low", "low", 
                    "low", "high", "low", "low", "low", 
                    "high", "low", "low", "low", "high", 
                    "high", "high", "low"),
            fu.weeks = c(4, 10, 2, 8, 5, 6, 6, 7, 
                         12, 1, 3, 4, 5, 6, 3, 2, 5, 2)), 
     row.names = c(NA, -18L), class = "data.frame")

# Run REM meta-analysis
rem <- metagen(es, se, study, thirdwave)

# Subgroup analysis
update.meta(rem, by = rob)

# Run meta-regression on follow-up in weeks
metareg(rem, ~ fu.weeks)
{{< /highlight >}}



<style>
h1 {color: #2a7792;}
</style>


